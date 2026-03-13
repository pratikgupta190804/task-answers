# Task 3 — The National Anthem Test

Model Used: BPE(Byte Pair Encoder)

## Setup

### Inputs

### 1. English Transliteration

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

### 2. Devanagari Script

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

## Training

The tokenizer was trained using both scripts to simulate a **multilingual vocabulary**.

```bash
abctokz train --corpus data/input_devanagari_national_anthem.txt --corpus data/input_english_national_anthem.txt --model bpe --vocab-size 300 --output artifacts/task3_anthem_bpe
```

Result:
```bash
Training bpe tokenizer...
  Corpus: ['data\\input_devanagari_national_anthem.txt', 
'data\\input_english_national_anthem.txt']
  Output: artifacts\task3_anthem_bpe
Done! Tokenizer saved to artifacts\task3_anthem_bpe
  Vocabulary size: 1
```

Training on both scripts ensures that the tokenizer learns **shared subword units across languages**, which allows us to compare tokenization behavior between Latin transliteration and Devanagari.

---

## Encoding

After training, each file was encoded using the trained tokenizer.

```bash
abctokz encode --model artifacts/task3_anthem_bpe --input data/input_english_national_anthem.txt
```
Result
```bash
Encoding: Jana Gana 
Mana Adhinayaka Jaya
 He Bharata Bhagya  
   Vidhata Pun...   
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ J     │  0 │
│   1 │ ##a   │  0 │
│   2 │ ##n   │  0 │
│   3 │ ##a   │  0 │
│   4 │ ##    │  0 │
│   5 │ ##G   │  0 │
│   6 │ ##a   │  0 │
│   7 │ ##n   │  0 │
│   8 │ ##a   │  0 │
│   9 │ ##    │  0 │
│  10 │ ##M   │  0 │
│  11 │ ##a   │  0 │
│  12 │ ##n   │  0 │
│  13 │ ##a   │  0 │
│  14 │ ##    │  0 │
│  15 │ ##A   │  0 │
│  16 │ ##d   │  0 │
│  17 │ ##h   │  0 │
│  18 │ ##i   │  0 │
│  19 │ ##n   │  0 │
│  20 │ ##a   │  0 │
│  21 │ ##y   │  0 │
│  22 │ ##a   │  0 │
│  23 │ ##k   │  0 │
│  24 │ ##a   │  0 │
│  25 │ ##    │  0 │
│  26 │ ##J   │  0 │
│  27 │ ##a   │  0 │
│  28 │ ##y   │  0 │
│  29 │ ##a   │  0 │
│  30 │ ##    │  0 │
│  31 │ ##H   │  0 │
│  32 │ ##e   │  0 │
│  33 │ ##    │  0 │
│  34 │ ##B   │  0 │
│  35 │ ##h   │  0 │
│  36 │ ##a   │  0 │
│  37 │ ##r   │  0 │
│  38 │ ##a   │  0 │
│  39 │ ##t   │  0 │
│  40 │ ##a   │  0 │
│  41 │ ##    │  0 │
│  42 │ ##B   │  0 │
│  43 │ ##h   │  0 │
│  44 │ ##a   │  0 │
│  45 │ ##g   │  0 │
│  46 │ ##y   │  0 │
│  47 │ ##a   │  0 │
│  48 │ ##    │  0 │
│  49 │ ##V   │  0 │
│  50 │ ##i   │  0 │
│  51 │ ##d   │  0 │
│  52 │ ##h   │  0 │
│  53 │ ##a   │  0 │
│  54 │ ##t   │  0 │
│  55 │ ##a   │  0 │
│  56 │ ##    │  0 │
│  57 │ ##P   │  0 │
│  58 │ ##u   │  0 │
│  59 │ ##n   │  0 │
│  60 │ ##j   │  0 │
│  61 │ ##a   │  0 │
│  62 │ ##b   │  0 │
│  63 │ ##    │  0 │
│  64 │ ##S   │  0 │
│  65 │ ##i   │  0 │
│  66 │ ##n   │  0 │
│  67 │ ##d   │  0 │
│  68 │ ##h   │  0 │
│  69 │ ##u   │  0 │
└─────┴───────┴────┘
```
Encoding Devanagari
```bash
abctokz encode --model artifacts/task3_anthem_bpe --input data/input_devanagari_national_anthem.txt
```
Result
```bash
Encoding: जन गण मन 
     अधिनायक जय हे 
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ जन    │ 73 │
│   1 │ ##    │  0 │
│   2 │ ##ग   │ 30 │
│   3 │ ##ण   │ 33 │
│   4 │ ##    │  0 │
│   5 │ ##म   │  0 │
│   6 │ ##न   │ 39 │
│   7 │ ##    │  0 │
│   8 │ ##अ   │  0 │
│   9 │ ##ध   │ 36 │
│  10 │ ##ि    │ 46 │
│  11 │ ##न   │ 39 │
│  12 │ ##ा    │ 45 │
│  13 │ ##य   │ 41 │
│  14 │ ##क   │  0 │
│  15 │ ##    │  0 │
│  16 │ ##ज   │  0 │
│  17 │ ##य   │ 41 │
│  18 │ ##    │  0 │
│  19 │ ##ह   │  0 │
│  20 │ ##े    │ 50 │
└─────┴───────┴────┘
```

---

## Metrics

### Token Count

script
 ```bash
  @'
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

eng_tokens = token_count(eng_lines)
dev_tokens = token_count(dev_lines)

print("English token count:", eng_tokens)
print("Devanagari token count:", dev_tokens)
'@ | python -
```
output
```bash
English token count: 318
Devanagari token count: 227
```

###Fertility

The key metric used in this experiment is **Fertility**, defined as:

```
fertility = tokens / words
```

This metric indicates how many tokens are produced per word.
Higher fertility means **more token fragmentation**, indicating lower tokenization efficiency.

---

## Results

| Script                  | Tokens | Words | Fertility |
| ----------------------- | ------ | ----- | --------- |
| English Transliteration | TBD    | TBD   | TBD       |
| Devanagari              | TBD    | TBD   | TBD       |

---

## Goal of the Experiment

This experiment evaluates how a multilingual tokenizer handles:

* Latin transliteration vs. native script
* Vocabulary allocation across scripts
* Token fragmentation differences

The results help reveal **biases in tokenizer vocabulary and training data coverage**.

---

## Bonus Experiment

For comparison, the same text can also be encoded using the tokenizer used by modern large language models via the **`tiktoken`** library. This provides a reference point for how production tokenizers handle Indic scripts.
