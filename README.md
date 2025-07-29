# SimHash Text Similarity (Go)

A small Go project that computes **SimHash** fingerprints for two text files and compares them using **Hamming distance** and a simple **similarity percentage**.

---

## What is SimHash?

**SimHash** is a locality-sensitive hashing (LSH) technique for turning a document into a fixed-length bit fingerprint such that **similar documents produce similar fingerprints**.  
Closeness is measured by **Hamming distance** (the number of differing bits). Lower distance ⇒ more similar.

High-level idea:

1. Extract features from the text (e.g., words).
2. Hash each feature to a fixed-length bit string.
3. Build a weighted vector: for each bit position, **add** the feature weight if the bit is `1`, **subtract** if `0`.
4. The **sign** of each position (positive or non-positive) becomes a bit of the fingerprint.
5. Compare two fingerprints by counting differing bits (Hamming distance).

---

## How it works in this project

Pipeline for each file:

1. **Read file** → `ReadFile`
2. **Tokenize & clean** (lowercase + English stop-word removal) → `SplitAndClean`
3. **Count word frequencies** (weights) → `CountWordOccurences`
4. **Hash each word** using MD5 to a 128-bit binary string → `GetHashAsString`
5. **Build weighted vector** of length `NumHashBits` → `MakeWeightsVector`
6. **Generate SimHash fingerprint** by taking the sign of each position → `GenerateFingerprint`
7. **Compare fingerprints** with Hamming distance → `GetHammingsDistance`
8. **Similarity (%)** = `100 - (100 * hamming / NumHashBits)`

> In `main.go`, `NumHashBits` is set to **128** (recommended when using MD5).

---

## Notes

- Uses MD5 (128 bits); can be replaced with SHA-256
- Only term frequencies used (no TF-IDF)
- Basic stop-word list in English
