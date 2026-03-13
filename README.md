# Task 3 — The National Anthem Tokenization Test

Model Used:
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

Encoding English Transliteration
```
abctokz encode --model artifacts/task3_anthem_bpe --input data/input_english_national_anthem.txt
```
output
```bash
Encoding: Jana Gana 
Mana Adhinayaka Jaya
         He
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ Ja    │ 63 │
│   1 │ ##n   │ 20 │
│   2 │ ##a   │  1 │
│   3 │ ##    │  0 │
│   4 │ ##G   │  0 │
│   5 │ ##a   │  1 │
│   6 │ ##n   │ 20 │
│   7 │ ##a   │  1 │
│   8 │ ##    │  0 │
│   9 │ ##M   │  0 │
│  10 │ ##a   │  1 │
│  11 │ ##n   │ 20 │
│  12 │ ##a   │  1 │
│  13 │ ##    │  0 │
│  14 │ ##A   │  0 │
│  15 │ ##d   │  8 │
│  16 │ ##h   │ 14 │
│  17 │ ##i   │ 18 │
│  18 │ ##n   │ 20 │
│  19 │ ##a   │  1 │
│  20 │ ##y   │ 28 │
│  21 │ ##a   │  1 │
│  22 │ ##k   │  0 │
│  23 │ ##a   │  1 │
│  24 │ ##    │  0 │
│  25 │ ##J   │  0 │
│  26 │ ##a   │  1 │
│  27 │ ##y   │ 28 │
│  28 │ ##a   │  1 │
│  29 │ ##    │  0 │
│  30 │ ##H   │  0 │
│  31 │ ##e   │ 11 │
└─────┴───────┴────┘
 Encoding: Bharata  
   Bhagya Vidhata   
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ B     │ 52 │
│   1 │ ##h   │ 14 │
│   2 │ ##a   │  1 │
│   3 │ ##r   │ 22 │
│   4 │ ##a   │  1 │
│   5 │ ##t   │ 24 │
│   6 │ ##a   │  1 │
│   7 │ ##    │  0 │
│   8 │ ##B   │  0 │
│   9 │ ##h   │ 14 │
│  10 │ ##a   │  1 │
│  11 │ ##g   │ 12 │
│  12 │ ##y   │ 28 │
│  13 │ ##a   │  1 │
│  14 │ ##    │  0 │
│  15 │ ##V   │  0 │
│  16 │ ##i   │ 18 │
│  17 │ ##d   │  8 │
│  18 │ ##h   │ 14 │
│  19 │ ##a   │  1 │
│  20 │ ##t   │ 24 │
│  21 │ ##a   │  1 │
└─────┴───────┴────┘
  Encoding: Punjab  
   Sindhu Gujarat   
      Maratha       
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ P     │  0 │
│   1 │ ##u   │ 26 │
│   2 │ ##n   │ 20 │
│   3 │ ##j   │  0 │
│   4 │ ##a   │  1 │
│   5 │ ##b   │  6 │
│   6 │ ##    │  0 │
│   7 │ ##S   │  0 │
│   8 │ ##i   │ 18 │
│   9 │ ##n   │ 20 │
│  10 │ ##d   │  8 │
│  11 │ ##h   │ 14 │
│  12 │ ##u   │ 26 │
│  13 │ ##    │  0 │
│  14 │ ##G   │  0 │
│  15 │ ##u   │ 26 │
│  16 │ ##j   │  0 │
│  17 │ ##a   │  1 │
│  18 │ ##r   │ 22 │
│  19 │ ##a   │  1 │
│  20 │ ##t   │ 24 │
│  21 │ ##    │  0 │
│  22 │ ##M   │  0 │
│  23 │ ##a   │  1 │
│  24 │ ##r   │ 22 │
│  25 │ ##a   │  1 │
│  26 │ ##t   │ 24 │
│  27 │ ##h   │ 14 │
│  28 │ ##a   │  1 │
└─────┴───────┴────┘
 Encoding: Dravida  
    Utkala Banga    
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ D     │  0 │
│   1 │ ##r   │ 22 │
│   2 │ ##a   │  1 │
│   3 │ ##v   │ 27 │
│   4 │ ##i   │ 18 │
│   5 │ ##d   │  8 │
│   6 │ ##a   │  1 │
│   7 │ ##    │  0 │
│   8 │ ##U   │  0 │
│   9 │ ##t   │ 24 │
│  10 │ ##k   │  0 │
│  11 │ ##a   │  1 │
│  12 │ ##l   │  0 │
│  13 │ ##a   │  1 │
│  14 │ ##    │  0 │
│  15 │ ##B   │  0 │
│  16 │ ##a   │  1 │
│  17 │ ##n   │ 20 │
│  18 │ ##g   │ 12 │
│  19 │ ##a   │  1 │
└─────┴───────┴────┘
 Encoding: Vindhya  
  Himachala Yamuna  
       Ganga        
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ V     │ 68 │
│   1 │ ##i   │ 18 │
│   2 │ ##n   │ 20 │
│   3 │ ##d   │  8 │
│   4 │ ##h   │ 14 │
│   5 │ ##y   │ 28 │
│   6 │ ##a   │  1 │
│   7 │ ##    │  0 │
│   8 │ ##H   │  0 │
│   9 │ ##i   │ 18 │
│  10 │ ##m   │  0 │
│  11 │ ##a   │  1 │
│  12 │ ##c   │  0 │
│  13 │ ##h   │ 14 │
│  14 │ ##a   │  1 │
│  15 │ ##l   │  0 │
│  16 │ ##a   │  1 │
│  17 │ ##    │  0 │
│  18 │ ##Y   │  0 │
│  19 │ ##a   │  1 │
│  20 │ ##m   │  0 │
│  21 │ ##u   │ 26 │
│  22 │ ##n   │ 20 │
│  23 │ ##a   │  1 │
│  24 │ ##    │  0 │
│  25 │ ##G   │  0 │
│  26 │ ##a   │  1 │
│  27 │ ##n   │ 20 │
│  28 │ ##g   │ 12 │
│  29 │ ##a   │  1 │
└─────┴───────┴────┘
 Encoding: Ucchala  
  Jaladhi Taranga   
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ U     │  0 │
│   1 │ ##c   │  0 │
│   2 │ ##c   │  0 │
│   3 │ ##h   │ 14 │
│   4 │ ##a   │  1 │
│   5 │ ##l   │  0 │
│   6 │ ##a   │  1 │
│   7 │ ##    │  0 │
│   8 │ ##J   │  0 │
│   9 │ ##a   │  1 │
│  10 │ ##l   │  0 │
│  11 │ ##a   │  1 │
│  12 │ ##d   │  8 │
│  13 │ ##h   │ 14 │
│  14 │ ##i   │ 18 │
│  15 │ ##    │  0 │
│  16 │ ##T   │  0 │
│  17 │ ##a   │  1 │
│  18 │ ##r   │ 22 │
│  19 │ ##a   │  1 │
│  20 │ ##n   │ 20 │
│  21 │ ##g   │ 12 │
│  22 │ ##a   │  1 │
└─────┴───────┴────┘
   Encoding: Tava   
  Shubha Name Jage  
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ T     │ 66 │
│   1 │ ##a   │  1 │
│   2 │ ##v   │ 27 │
│   3 │ ##a   │  1 │
│   4 │ ##    │  0 │
│   5 │ ##S   │  0 │
│   6 │ ##h   │ 14 │
│   7 │ ##u   │ 26 │
│   8 │ ##b   │  6 │
│   9 │ ##h   │ 14 │
│  10 │ ##a   │  1 │
│  11 │ ##    │  0 │
│  12 │ ##N   │  0 │
│  13 │ ##a   │  1 │
│  14 │ ##m   │  0 │
│  15 │ ##e   │ 11 │
│  16 │ ##    │  0 │
│  17 │ ##J   │  0 │
│  18 │ ##a   │  1 │
│  19 │ ##g   │ 12 │
│  20 │ ##e   │ 11 │
└─────┴───────┴────┘
   Encoding: Tava   
Shubha Ashisha Mage 
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ T     │ 66 │
│   1 │ ##a   │  1 │
│   2 │ ##v   │ 27 │
│   3 │ ##a   │  1 │
│   4 │ ##    │  0 │
│   5 │ ##S   │  0 │
│   6 │ ##h   │ 14 │
│   7 │ ##u   │ 26 │
│   8 │ ##b   │  6 │
│   9 │ ##h   │ 14 │
│  10 │ ##a   │  1 │
│  11 │ ##    │  0 │
│  12 │ ##A   │  0 │
│  13 │ ##s   │  0 │
│  14 │ ##h   │ 14 │
│  15 │ ##i   │ 18 │
│  16 │ ##s   │  0 │
│  17 │ ##h   │ 14 │
│  18 │ ##a   │  1 │
│  19 │ ##    │  0 │
│  20 │ ##M   │  0 │
│  21 │ ##a   │  1 │
│  22 │ ##g   │ 12 │
│  23 │ ##e   │ 11 │
└─────┴───────┴────┘
Encoding: Gahe Tava 
     Jaya Gatha     
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ G     │ 56 │
│   1 │ ##a   │  1 │
│   2 │ ##h   │ 14 │
│   3 │ ##e   │ 11 │
│   4 │ ##    │  0 │
│   5 │ ##T   │  0 │
│   6 │ ##a   │  1 │
│   7 │ ##v   │ 27 │
│   8 │ ##a   │  1 │
│   9 │ ##    │  0 │
│  10 │ ##J   │  0 │
│  11 │ ##a   │  1 │
│  12 │ ##y   │ 28 │
│  13 │ ##a   │  1 │
│  14 │ ##    │  0 │
│  15 │ ##G   │  0 │
│  16 │ ##a   │  1 │
│  17 │ ##t   │ 24 │
│  18 │ ##h   │ 14 │
│  19 │ ##a   │  1 │
└─────┴───────┴────┘
Encoding: Jana Gana 
Mangala Dayaka Jaya 
         He
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ Ja    │ 63 │
│   1 │ ##n   │ 20 │
│   2 │ ##a   │  1 │
│   3 │ ##    │  0 │
│   4 │ ##G   │  0 │
│   5 │ ##a   │  1 │
│   6 │ ##n   │ 20 │
│   7 │ ##a   │  1 │
│   8 │ ##    │  0 │
│   9 │ ##M   │  0 │
│  10 │ ##a   │  1 │
│  11 │ ##n   │ 20 │
│  12 │ ##g   │ 12 │
│  13 │ ##a   │  1 │
│  14 │ ##l   │  0 │
│  15 │ ##a   │  1 │
│  16 │ ##    │  0 │
│  17 │ ##D   │  0 │
│  18 │ ##a   │  1 │
│  19 │ ##y   │ 28 │
│  20 │ ##a   │  1 │
│  21 │ ##k   │  0 │
│  22 │ ##a   │  1 │
│  23 │ ##    │  0 │
│  24 │ ##J   │  0 │
│  25 │ ##a   │  1 │
│  26 │ ##y   │ 28 │
│  27 │ ##a   │  1 │
│  28 │ ##    │  0 │
│  29 │ ##H   │  0 │
│  30 │ ##e   │ 11 │
└─────┴───────┴────┘
 Encoding: Bharata  
   Bhagya Vidhata   
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ B     │ 52 │
│   1 │ ##h   │ 14 │
│   2 │ ##a   │  1 │
│   3 │ ##r   │ 22 │
│   4 │ ##a   │  1 │
│   5 │ ##t   │ 24 │
│   6 │ ##a   │  1 │
│   7 │ ##    │  0 │
│   8 │ ##B   │  0 │
│   9 │ ##h   │ 14 │
│  10 │ ##a   │  1 │
│  11 │ ##g   │ 12 │
│  12 │ ##y   │ 28 │
│  13 │ ##a   │  1 │
│  14 │ ##    │  0 │
│  15 │ ##V   │  0 │
│  16 │ ##i   │ 18 │
│  17 │ ##d   │  8 │
│  18 │ ##h   │ 14 │
│  19 │ ##a   │  1 │
│  20 │ ##t   │ 24 │
│  21 │ ##a   │  1 │
└─────┴───────┴────┘
 Encoding: Jaya He  
  Jaya He Jaya He   
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ Ja    │ 63 │
│   1 │ ##y   │ 28 │
│   2 │ ##a   │  1 │
│   3 │ ##    │  0 │
│   4 │ ##H   │  0 │
│   5 │ ##e   │ 11 │
│   6 │ ##    │  0 │
│   7 │ ##J   │  0 │
│   8 │ ##a   │  1 │
│   9 │ ##y   │ 28 │
│  10 │ ##a   │  1 │
│  11 │ ##    │  0 │
│  12 │ ##H   │  0 │
│  13 │ ##e   │ 11 │
│  14 │ ##    │  0 │
│  15 │ ##J   │  0 │
│  16 │ ##a   │  1 │
│  17 │ ##y   │ 28 │
│  18 │ ##a   │  1 │
│  19 │ ##    │  0 │
│  20 │ ##H   │  0 │
│  21 │ ##e   │ 11 │
└─────┴───────┴────┘
Encoding: Jaya Jaya 
    Jaya Jaya He    
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ Ja    │ 63 │
│   1 │ ##y   │ 28 │
│   2 │ ##a   │  1 │
│   3 │ ##    │  0 │
│   4 │ ##J   │  0 │
│   5 │ ##a   │  1 │
│   6 │ ##y   │ 28 │
│   7 │ ##a   │  1 │
│   8 │ ##    │  0 │
│   9 │ ##J   │  0 │
│  10 │ ##a   │  1 │
│  11 │ ##y   │ 28 │
│  12 │ ##a   │  1 │
│  13 │ ##    │  0 │
│  14 │ ##J   │  0 │
│  15 │ ##a   │  1 │
│  16 │ ##y   │ 28 │
│  17 │ ##a   │  1 │
│  18 │ ##    │  0 │
│  19 │ ##H   │  0 │
│  20 │ ##e   │ 11 │
└─────┴───────┴────┘
```
and

Encoding Devanagari scripts
```
abctokz encode --model artifacts/task3_anthem_bpe --input data/input_devanagari_national_anthem.txt
```
output
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
 Encoding: भारत भाग्य  
        विधाता
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ भा     │ 80 │
│   1 │ ##र   │ 42 │
│   2 │ ##त   │ 34 │
│   3 │ ##    │  0 │
│   4 │ ##भ   │ 40 │
│   5 │ ##ा    │ 45 │
│   6 │ ##ग   │ 30 │
│   7 │ ##्    │ 51 │
│   8 │ ##य   │ 41 │
│   9 │ ##    │  0 │
│  10 │ ##व   │ 44 │
│  11 │ ##ि    │ 46 │
│  12 │ ##ध   │ 36 │
│  13 │ ##ा    │ 45 │
│  14 │ ##त   │ 34 │
│  15 │ ##ा    │ 45 │
└─────┴───────┴────┘
  Encoding: पंजाब सिंधु  
      गुजरात मराठा      
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ प     │  0 │
│   1 │ ##ं    │  0 │
│   2 │ ##ज   │  0 │
│   3 │ ##ा    │ 45 │
│   4 │ ##ब   │  0 │
│   5 │ ##    │  0 │
│   6 │ ##स   │  0 │
│   7 │ ##ि    │ 46 │
│   8 │ ##ं    │  0 │
│   9 │ ##ध   │ 36 │
│  10 │ ##ु    │ 48 │
│  11 │ ##    │  0 │
│  12 │ ##ग   │ 30 │
│  13 │ ##ु    │ 48 │
│  14 │ ##ज   │  0 │
│  15 │ ##र   │ 42 │
│  16 │ ##ा    │ 45 │
│  17 │ ##त   │ 34 │
│  18 │ ##    │  0 │
│  19 │ ##म   │  0 │
│  20 │ ##र   │ 42 │
│  21 │ ##ा    │ 45 │
│  22 │ ##ठ   │  0 │
│  23 │ ##ा    │ 45 │
└─────┴───────┴────┘
Encoding: द्राविड़ उत्कल 
         बंग
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ द     │  0 │
│   1 │ ##्    │ 51 │
│   2 │ ##र   │ 42 │
│   3 │ ##ा    │ 45 │
│   4 │ ##व   │ 44 │
│   5 │ ##ि    │ 46 │
│   6 │ ##ड   │  0 │
│   7 │ ##़    │  0 │
│   8 │ ##    │  0 │
│   9 │ ##उ   │  0 │
│  10 │ ##त   │ 34 │
│  11 │ ##्    │ 51 │
│  12 │ ##क   │  0 │
│  13 │ ##ल   │  0 │
│  14 │ ##    │  0 │
│  15 │ ##ब   │  0 │
│  16 │ ##ं    │  0 │
│  17 │ ##ग   │ 30 │
└─────┴───────┴────┘
 Encoding: विंध्य हिमाचल 
       यमुना गंगा       
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ व     │ 81 │
│   1 │ ##ि    │ 46 │
│   2 │ ##ं    │  0 │
│   3 │ ##ध   │ 36 │
│   4 │ ##्    │ 51 │
│   5 │ ##य   │ 41 │
│   6 │ ##    │  0 │
│   7 │ ##ह   │  0 │
│   8 │ ##ि    │ 46 │
│   9 │ ##म   │  0 │
│  10 │ ##ा    │ 45 │
│  11 │ ##च   │  0 │
│  12 │ ##ल   │  0 │
│  13 │ ##    │  0 │
│  14 │ ##य   │ 41 │
│  15 │ ##म   │  0 │
│  16 │ ##ु    │ 48 │
│  17 │ ##न   │ 39 │
│  18 │ ##ा    │ 45 │
│  19 │ ##    │  0 │
│  20 │ ##ग   │ 30 │
│  21 │ ##ं    │  0 │
│  22 │ ##ग   │ 30 │
│  23 │ ##ा    │ 45 │
└─────┴───────┴────┘
 Encoding: उच्छल जलधि 
        तरंग
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ उ     │  0 │
│   1 │ ##च   │  0 │
│   2 │ ##्    │ 51 │
│   3 │ ##छ   │  0 │
│   4 │ ##ल   │  0 │
│   5 │ ##    │  0 │
│   6 │ ##ज   │  0 │
│   7 │ ##ल   │  0 │
│   8 │ ##ध   │ 36 │
│   9 │ ##ि    │ 46 │
│  10 │ ##    │  0 │
│  11 │ ##त   │ 34 │
│  12 │ ##र   │ 42 │
│  13 │ ##ं    │  0 │
│  14 │ ##ग   │ 30 │
└─────┴───────┴────┘
 Encoding: तव शुभ नामे 
         जागे
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ तव    │ 76 │
│   1 │ ##    │  0 │
│   2 │ ##श   │  0 │
│   3 │ ##ु    │ 48 │
│   4 │ ##भ   │ 40 │
│   5 │ ##    │  0 │
│   6 │ ##न   │ 39 │
│   7 │ ##ा    │ 45 │
│   8 │ ##म   │  0 │
│   9 │ ##े    │ 50 │
│  10 │ ##    │  0 │
│  11 │ ##ज   │  0 │
│  12 │ ##ा    │ 45 │
│  13 │ ##ग   │ 30 │
│  14 │ ##े    │ 50 │
└─────┴───────┴────┘
Encoding: तव शुभ आशीष 
         मागे
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ तव    │ 76 │
│   1 │ ##    │  0 │
│   2 │ ##श   │  0 │
│   3 │ ##ु    │ 48 │
│   4 │ ##भ   │ 40 │
│   5 │ ##    │  0 │
│   6 │ ##आ   │  0 │
│   7 │ ##श   │  0 │
│   8 │ ##ी    │  0 │
│   9 │ ##ष   │  0 │
│  10 │ ##    │  0 │
│  11 │ ##म   │  0 │
│  12 │ ##ा    │ 45 │
│  13 │ ##ग   │ 30 │
│  14 │ ##े    │ 50 │
└─────┴───────┴────┘
 Encoding: गाहे तव जय 
         गाथा
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ ग     │ 70 │
│   1 │ ##ा    │ 45 │
│   2 │ ##ह   │  0 │
│   3 │ ##े    │ 50 │
│   4 │ ##    │  0 │
│   5 │ ##त   │ 34 │
│   6 │ ##व   │ 44 │
│   7 │ ##    │  0 │
│   8 │ ##ज   │  0 │
│   9 │ ##य   │ 41 │
│  10 │ ##    │  0 │
│  11 │ ##ग   │ 30 │
│  12 │ ##ा    │ 45 │
│  13 │ ##थ   │  0 │
│  14 │ ##ा    │ 45 │
└─────┴───────┴────┘
  Encoding: जन गण   
    मंगलदायक जय हे     
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ जन    │ 73 │
│   1 │ ##    │  0 │
│   2 │ ##ग   │ 30 │
│   3 │ ##ण   │ 33 │
│   4 │ ##    │  0 │
│   5 │ ##म   │  0 │
│   6 │ ##ं    │  0 │
│   7 │ ##ग   │ 30 │
│   8 │ ##ल   │  0 │
│   9 │ ##द   │  0 │
│  10 │ ##ा    │ 45 │
│  11 │ ##य   │ 41 │
│  12 │ ##क   │  0 │
│  13 │ ##    │  0 │
│  14 │ ##ज   │  0 │
│  15 │ ##य   │ 41 │
│  16 │ ##    │  0 │
│  17 │ ##ह   │  0 │
│  18 │ ##े    │ 50 │
└─────┴───────┴────┘
 Encoding: भारत भाग्य  
        विधाता
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ भा     │ 80 │
│   1 │ ##र   │ 42 │
│   2 │ ##त   │ 34 │
│   3 │ ##    │  0 │
│   4 │ ##भ   │ 40 │
│   5 │ ##ा    │ 45 │
│   6 │ ##ग   │ 30 │
│   7 │ ##्    │ 51 │
│   8 │ ##य   │ 41 │
│   9 │ ##    │  0 │
│  10 │ ##व   │ 44 │
│  11 │ ##ि    │ 46 │
│  12 │ ##ध   │ 36 │
│  13 │ ##ा    │ 45 │
│  14 │ ##त   │ 34 │
│  15 │ ##ा    │ 45 │
└─────┴───────┴────┘
Encoding: जय हे जय हे 
        जय हे        
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ जय    │ 74 │
│   1 │ ##    │  0 │
│   2 │ ##ह   │  0 │
│   3 │ ##े    │ 50 │
│   4 │ ##    │  0 │
│   5 │ ##ज   │  0 │
│   6 │ ##य   │ 41 │
│   7 │ ##    │  0 │
│   8 │ ##ह   │  0 │
│   9 │ ##े    │ 50 │
│  10 │ ##    │  0 │
│  11 │ ##ज   │  0 │
│  12 │ ##य   │ 41 │
│  13 │ ##    │  0 │
│  14 │ ##ह   │  0 │
│  15 │ ##े    │ 50 │
└─────┴───────┴────┘
 Encoding: जय जय जय 
        जय हे        
┏━━━━━┳━━━━━━━┳━━━━┓
┃ Pos ┃ Token ┃ ID ┃
┡━━━━━╇━━━━━━━╇━━━━┩
│   0 │ जय    │ 74 │
│   1 │ ##    │  0 │
│   2 │ ##ज   │  0 │
│   3 │ ##य   │ 41 │
│   4 │ ##    │  0 │
│   5 │ ##ज   │  0 │
│   6 │ ##य   │ 41 │
│   7 │ ##    │  0 │
│   8 │ ##ज   │  0 │
│   9 │ ##य   │ 41 │
│  10 │ ##    │  0 │
│  11 │ ##ह   │  0 │
│  12 │ ##े    │ 50 │
└─────┴───────┴────┘
```

The tokenizer outputs a table containing:

* Token position
* Token text
* Token ID

---

# 5. Evaluation Metrics

## Token Count

Token count simply measures how many tokens the tokenizer produced for the input.

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





