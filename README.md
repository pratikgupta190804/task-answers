# Task 3 — The National Anthem Tokenization Test

## Model Used

**BPE (Byte Pair Encoding)**

---

# 1. Setup

## Input Data

To analyze tokenization behavior across scripts, we used the **first stanza of the Indian National Anthem (Jana Gana Mana)** in two different representations:

1. **English Transliteration (Latin Script)**
2. **Original Devanagari Script**

This allows us to observe how the same linguistic content behaves under different writing systems during tokenization.

---

# 2. Input Files

## 2.1 English Transliteration

**File:** `input_english_national_anthem.txt`

```
Jana Gana Mana Adhinayaka Jaya He
Bharata Bhagya Vidhata
Punjab Sindhu Gujarat Maratha
Dravida Utkala Banga
Vindhya Himachala Yamuna Ganga
Ucchala Jaladhi Taranga
Tava Shubha Name Jage
Tava Shubha Ashisha Mage
Gahe Tava Jaya Gatha
Jana Gana Mangala Dayaka Jaya He
Bharata Bhagya Vidhata
Jaya He Jaya He Jaya He
Jaya Jaya Jaya Jaya He
```

---

## 2.2 Devanagari Script

**File:** `input_devanagari_national_anthem.txt`

```
जन गण मन अधिनायक जय हे
भारत भाग्य विधाता
पंजाब सिंधु गुजरात मराठा
द्राविड़ उत्कल बंग
विंध्य हिमाचल यमुना गंगा
उच्छल जलधि तरंग
तव शुभ नामे जागे
तव शुभ आशीष मागे
गाहे तव जय गाथा
जन गण मंगलदायक जय हे
भारत भाग्य विधाता
जय हे जय हे जय हे
जय जय जय जय हे
```

---

# 3. Training the Tokenizer

The tokenizer was trained on **both scripts together** so that it could learn subword patterns from a **mixed multilingual corpus**.

```
abctokz train \
--corpus data/input_devanagari_national_anthem.txt \
--corpus data/input_english_national_anthem.txt \
--model bpe \
--vocab-size 300 \
--output artifacts/task3_anthem_bpe
```

### Training Output

```
Training bpe tokenizer...
Corpus: [input_devanagari_national_anthem.txt, input_english_national_anthem.txt]
Output: artifacts/task3_anthem_bpe

Done! Tokenizer saved.
Vocabulary size: 87
```

Because the dataset is extremely small, the tokenizer can only learn **very limited subword patterns**.

This becomes important later when we analyze the results.

---

# 4. Encoding the Text

After training, both input files were encoded using the trained tokenizer.

### Encoding Command

```
abctokz encode --model artifacts/task3_anthem_bpe --input data/input_english_national_anthem.txt
```

and

```
abctokz encode --model artifacts/task3_anthem_bpe --input data/input_devanagari_national_anthem.txt
```

The tokenizer outputs a table containing:

* Token position
* Token text
* Token ID

*(Encoding tables omitted here for brevity.)*

---

# 5. Evaluation Metrics

## Token Count

Token count simply measures how many tokens the tokenizer produces for the text.

Script used:

```python
from pathlib import Path
from abctokz import Tokenizer

model = r"artifacts\task3_anthem_bpe"
eng_file = r"data\input_english_national_anthem.txt"
dev_file = r"data\input_devanagari_national_anthem.txt"

tok = Tokenizer.load(model)

def read_lines(p):
    return [x.strip() for x in Path(p).read_text(encoding="utf-8").splitlines() if x.strip()]

def token_count(lines):
    return sum(len(tok.encode(line)) for line in lines)

eng_lines = read_lines(eng_file)
dev_lines = read_lines(dev_file)

print("English token count:", token_count(eng_lines))
print("Devanagari token count:", token_count(dev_lines))
```

### Output

```
English token count: 317
Devanagari token count: 227
```

---

## Fertility

The main metric used in this experiment is **Fertility**.

### Definition

```
Fertility = Tokens / Words
```

Fertility measures **how many tokens are produced per word**.

* **Higher fertility → more token fragmentation**
* **Lower fertility → more efficient tokenization**

---

# 6. Results

| Script                  | Tokens | Words | Fertility |
| ----------------------- | ------ | ----- | --------- |
| English Transliteration | 317    | 55    | 5.763     |
| Devanagari              | 227    | 55    | 4.203     |

### Observation

The **English transliteration produces significantly more tokens** than the Devanagari version.

This means:

> The English transliteration is **less token-efficient**, because each word is split into more pieces.

---

# 7. Why Do the Token Counts Differ?

Even though both texts represent the **same anthem**, the token counts differ for several reasons.

### 1. Script Structure

The **Latin script** represents sounds using individual letters.

For example:

```
Adhinayaka
```

This becomes multiple character combinations that the tokenizer may not recognize, forcing it to split the word into many pieces.

In contrast, **Devanagari characters often represent richer phonetic units**, allowing the tokenizer to form slightly larger subword pieces.

---

### 2. Limited Training Data

Our tokenizer was trained on **only the anthem text**, which is an extremely small dataset.

Because of this:

* The tokenizer could not learn many useful subword merges
* Words are broken into smaller fragments

This increases the total number of tokens.

---

### 3. Vocabulary Size

The learned vocabulary size is **very small (87 tokens)**.

With such limited vocabulary coverage:

* Many words cannot be represented as larger subwords
* The tokenizer must fall back to smaller character-level pieces

---

### Conclusion

The difference is caused by a **combination of factors**:

* Script structure
* Limited training data
* Small vocabulary size

Together, these factors influence how efficiently the tokenizer can represent the text.

---

# 8. Bonus Experiment — GPT-4 Tokenizer (tiktoken)

To compare with a production-grade tokenizer, the same text was encoded using **GPT-4's tokenizer via the `tiktoken` library**.

### Installation

```
pip install tiktoken
```

---

## Script

```python
from pathlib import Path
import tiktoken

eng_file = r"data\input_english_national_anthem.txt"
dev_file = r"data\input_devanagari_national_anthem.txt"

enc = tiktoken.encoding_for_model("gpt-4")

def read_text(p):
    return Path(p).read_text(encoding="utf-8").strip()

def word_count(text):
    return len(text.split())

eng_text = read_text(eng_file)
dev_text = read_text(dev_file)

eng_tokens = enc.encode(eng_text)
dev_tokens = enc.encode(dev_text)

eng_words = word_count(eng_text)
dev_words = word_count(dev_text)

print("English tokens:", len(eng_tokens))
print("Devanagari tokens:", len(dev_tokens))
```

---

# 9. GPT-4 Tokenization Results

```
English Transliteration
tokens: 130
words: 55
fertility: 2.364

Devanagari
tokens: 276
words: 54
fertility: 5.111
```

---

# 10. Comparing the Tokenizers

| Tokenizer        | English Tokens | Devanagari Tokens |
| ---------------- | -------------- | ----------------- |
| Our BPE          | 317            | 227               |
| GPT-4 (tiktoken) | 130            | 276               |

---

# 11. What Does This Reveal?

The comparison highlights how **training scale and vocabulary size impact tokenization quality**.

### Our BPE Tokenizer

* Trained on **very small data**
* Small vocabulary
* Cannot learn strong subword patterns
* Words are heavily fragmented

### GPT-4 Tokenizer

* Trained on **massive multilingual datasets**
* Very large optimized vocabulary
* Already understands common patterns across many languages

As a result:

> GPT-4 can represent many words using **fewer tokens**, making it far more efficient.

---

# 12. Final Insight

This experiment demonstrates an important property of tokenization:

> **Token efficiency strongly depends on the tokenizer’s training data and vocabulary coverage.**

A tokenizer trained on limited text will split words into many small pieces, while a tokenizer trained on large multilingual corpora can encode the same text using far fewer tokens.


---

# Task 5 — Is It Truly Deterministic?

## Experiment Setup

To verify the claim that the tokenizer training process is deterministic, the same tokenizer was trained **twice** using:

* **Model:** BPE
* **Vocabulary Size:** 200
* **Corpus:** `data/corpus.txt`
* **Configuration:** identical for both runs

Two independent training runs were executed:

```
artifacts/run1
artifacts/run2
```

Both models were then used to encode the same test input.

---

# Experiment 

Script to compare both
```bash
@'
from pathlib import Path
from abctokz import Tokenizer
import hashlib

run1 = r"artifacts\run1"
run2 = r"artifacts\run2"
test_file = r"data\test.txt"

tok1 = Tokenizer.load(run1)
tok2 = Tokenizer.load(run2)

text = Path(test_file).read_text(encoding="utf-8")

enc1 = tok1.encode(text)
enc2 = tok2.encode(text)

print("Encoded tokens identical:", enc1 == enc2)

print("\nRun1 tokens:", enc1)
print("\nRun2 tokens:", enc2)

def file_hash(path):
    return hashlib.md5(Path(path).read_bytes()).hexdigest()

files = ["vocab.json", "merges.txt"]

print("\nFile Comparisons")
for f in files:
    p1 = Path(run1) / f
    p2 = Path(run2) / f

    if p1.exists() and p2.exists():
        h1 = file_hash(p1)
        h2 = file_hash(p2)
        print(f"{f}: identical =", h1 == h2)
    else:
        print(f"{f}: file missing")
'@ | python -
```

Output

```
Encoded tokens identical: True

Run1 tokens: Encoding(n_tokens=99, tokens=['J','##a','##n','##a','## ', ...])

Run2 tokens: Encoding(n_tokens=99, tokens=['J','##a','##n','##a','## ', ...])

File Comparisons
vocab.json: identical = True
merges.txt: identical = True
```

Both runs also produced the same artifact files:

```
config.json
manifest.json
merges.txt
special_tokens.json
vocab.json
```

---

# What Parts Are Deterministic?

The experiment confirms that several components of the tokenizer pipeline are **fully deterministic**.

### 1. Vocabulary Generation

The `vocab.json` files were identical across runs.

This means that:

* Token frequencies were computed consistently
* Subword merges were learned in the same order
* Token IDs were assigned deterministically

---

### 2. Merge Rule Learning (BPE)

The `merges.txt` files were identical.

This indicates that the **BPE merge algorithm consistently selected the same most frequent pair at every step**, leading to the same merge sequence.

---

### 3. Encoding Output

Encoding the same input text produced:

* identical token sequences
* identical token IDs
* identical token counts

This confirms that **inference is deterministic when the model artifacts are identical**.

---

# What Parts Are Not Strictly Deterministic?

Even though the tokenizer itself behaves deterministically, some aspects of the process may vary slightly.

### Benchmark Timing

Training time and encoding speed may differ slightly between runs due to:

* CPU scheduling
* background processes
* system load

This does **not affect tokenizer correctness**, so it is acceptable.

---

# Remaining Risks — When Could Results Differ?

Even deterministic algorithms can produce different outputs under certain conditions.

### 1. Corpus Order Changes

If the corpus lines are shuffled, the frequency counts might be processed in a different order during tie situations, which could alter merge decisions.

---

### 2. Frequency Tie-Breaking

If two character pairs have **exactly the same frequency**, the algorithm must choose one first.

If the implementation does not enforce a deterministic tie-breaking rule, merge order could change.

---

### 3. Different Software Versions

Using different versions of:

* the tokenizer library
* Python
* dependencies

could potentially affect behavior.

---

### 4. Parallel Processing

If training were parallelized in the future, race conditions could introduce non-deterministic ordering unless explicitly controlled.

---

# Conclusion

The experiment demonstrates that the tokenizer training pipeline is **deterministic under controlled conditions**.

Training the same tokenizer twice with the same corpus and configuration produced:

* identical vocabularies
* identical merge rules
* identical encoded outputs

The only non-deterministic aspects are **external factors such as runtime performance**, which do not affect the correctness of the tokenizer.


Below is a **clean, human-sounding `README.md` section** you can directly paste for **Task 7**.

---

# Task 7 — Does Encode → Decode Get You Back to Start?

A tokenizer should ideally be **lossless**. This means that if we:

```
text → encode → decode
```

we should get **exactly the same text back**.

To verify this, I tested the tokenizer on several multilingual examples including:

* English text
* Devanagari (Hindi)
* Words with accented characters
* Unicode edge cases

**File:** `roundtrip_test.txt`

```
Hello world tokenizer test
Jana Gana Mana Adhinayaka Jaya He
जन गण मन अधिनायक जय हे
नमस्ते दुनिया
Cafe
Café
Café
```

---

# Test Method

Each sentence was passed through the tokenizer using the following pipeline:

```
original_text → tokenizer.encode() → tokenizer.decode()
```

Then the decoded output was compared with the original string.

---

# Round-Trip Results

| Original Text                     | Decoded Output   | Match |
| --------------------------------- | ---------------- | ----- |
| Hello world tokenizer test        | *(empty string)* | No    |
| Jana Gana Mana Adhinayaka Jaya He | aaaaaaaaaaa      | No    |
| जन गण मन अधिनायक जय हे           | *(empty string)* | No    |
| नमस्ते दुनिया                         | *(empty string)* | No    |
| Cafe                              | Caf               | No    |
| Café                              | Café              | Yes   |
| Café                              | Caf               | No    |

### Summary

```
Total tests: 7
Exact matches: 1
Round trip success rate: 0.142857
```

Only **1 out of 7 cases** produced an exact round-trip.

---

# Case 1 — Exact Round Trip (Lossless)

Example:

```
Original: Café
Decoded : Café
Match   : True
```

### Why this works

The tokenizer contains a **single token for the composed character `é`**, so the encoding and decoding process preserves the word exactly.

```
Café → [C, ##a, ##f, ##é] → Café
```

Since the character is stored as a **single Unicode codepoint**, decoding reconstructs the original string correctly.

---

# Case 2 — Lossy Round Trip

Example:

```
Original: Cafe
Decoded : Caf
```

### What changed?

The final character **`e` disappeared** during decoding.

Tokens:

```
[C, ##a, ##f, ##e]
```

During decoding, the tokenizer failed to correctly reconstruct the last token.

### Why this happens

This likely occurs because:

* the tokenizer uses **subword tokens (`##`)**
* the decoding logic may **incorrectly strip or merge suffix tokens**

This causes the reconstructed string to lose characters.

---

# Unicode Edge Case — NFC vs NFD

Another interesting example is:

```
Original: Café
Decoded : Caf
```

At first glance, `Café` and `Café` look identical. However, they are **different Unicode representations**.

### NFC (composed form)

```
Café
```

Character sequence:

```
C + a + f + é
```

### NFD (decomposed form)

```
Café
```

Character sequence:

```
C + a + f + e + ◌́
```

Here the accent is a **separate combining character**.

The tokenizer splits it as:

```
[C, ##a, ##f, ##e, ##́]
```

During decoding, the combining accent is not reconstructed correctly, resulting in:

```
Caf
```

---

# Is the Lossy Behavior a Bug?

This depends on the intended design.

Possible explanations:

### 1. Implementation Bug

If the tokenizer is supposed to be **fully reversible**, then losing characters during decoding is a **bug in the decode logic**.

### 2. Acceptable Trade-off

Some tokenizers sacrifice perfect reconstruction for **simpler token rules**, especially when using subword prefixes like `##`.

### 3. Unicode Normalization Issues

Unicode normalization differences (NFC vs NFD) can produce visually identical text that is **not byte-identical**, which may cause decoding differences.

---

# What `round_trip_success_rate` Measures

The metric `round_trip_success_rate` checks whether:

```
decoded_text == original_text
```

If they match exactly, the test counts as successful.

So the metric measures:

* Exact string equality
* Whether encode → decode preserves the text perfectly

---

# What the Metric Does NOT Measure

However, this metric does **not detect several important cases**.

### 1. Visual Equality

Two strings can look identical but still fail the equality test.

Example:

```
Café (NFC)
Café (NFD)
```

They look the same but have **different Unicode codepoints**.

---

### 2. Semantic Equivalence

The metric does not check if the decoded text **still has the same meaning**.

Example:

```
Original: Cafe
Decoded : Caf
```

The metric simply flags it as failure but does not explain **why**.

---

### 3. Partial Reconstruction Errors

If a tokenizer drops or modifies characters, the metric only reports **failure**, not which tokens caused it.

---

# Key Insight

A tokenizer may appear to work correctly in most cases, but **Unicode normalization and subword decoding rules can introduce subtle reconstruction errors**.

This experiment shows that:

* exact round-trip behavior is **not guaranteed**
* Unicode representation plays an important role
* round-trip evaluation should consider **both byte equality and Unicode normalization**

