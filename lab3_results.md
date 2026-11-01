# Lab 03 — What RAG Is Made Of: results

Run in Google Colab on 2026-11-01 using the unmodified instructor notebook
(`lab03_what_is_rag.ipynb`), models `intfloat/multilingual-e5-small` and `Qwen/Qwen2.5-1.5B-Instruct`.

## 1. Part 1: prediction and measured chunk counts

| Strategy | Predicted | Measured |
|---|---|---|
| article-level | 4 | 4 |
| sentence-level | 22 (6+6+5+5) | 22 |

The prediction was written before the cell was run.

## 2. Part 2: retrieval for `cards-lost`

Prediction before running: **to be filled in by the student.** The Part 2 and Part 3
predictions were not written down before the cells were run, so they are left blank
rather than reconstructed after seeing the output.

Measured:

| Query | Article-level top-1 | Sentence-level top-3 | Needle (`cards-lost:5`) retrieved? |
|---|---|---|---|
| literal: "How much does an instant replacement card cost at the branch?" | `cards-lost` (0.869) — correct | `cards-lost:4` 0.851 (wrong sentence), `cards-lost:5` 0.848 (needle), `cards-expiry:4` 0.828 | rank 2, not top-1 |
| paraphrase: "I lost my wallet this morning and need a working card by tonight - what will it cost me?" | `cards-expiry` (0.881) — wrong article | `cards-expiry:4` 0.864, `cards-lost:4` 0.861, `cards-pin-block:1` 0.860 | not in top 3 |

The literal sentence-level gap between the wrong top-1 and the needle is 0.003 (0.851 vs 0.848),
so the ranking is close to arbitrary there.

## 3. Part 3: generation

Question: "How much does an instant replacement card cost at the branch?"

- **Without context:** "I'm sorry, but I cannot provide you with that information as it may violate our terms of service to answer questions about specific products or services. However, you can usually find this information on the official website of the company or contact their customer support for assistance."
-   (middle of the line was cut off in the Colab output view; "specific" is reconstructed from the visible "s…ic")
-   - **With the needle sentence as the only fact:** "The cost of an instant replacement card at the branch is 3000 tenge."
 
    - What the no-context answer got wrong: it gave no price and invented a "terms of service" reason for refusing, instead of saying it has no access to this bank's policy, so it neither answered nor honestly stated what it was missing.
 
    - ## 4. Chunking choice
 
    - For a corpus like this one I would chunk by article, because the articles are short near-neighbours and a single sentence carries too little context to separate them: here the sentence-level literal query ranked a wrong sentence first by 0.003, and the paraphrase query lost the needle at both levels. This is based on two queries on one article, not a full evaluation.
 
    - ## AI-use declaration
 
    - This lab was prepared with Claude (Anthropic) acting as an assistant: it read the assignment in Canvas, opened the instructor's notebook in Google Colab, ran all cells, read the printed outputs, and drafted this file. The code and models are the instructor's, unchanged. The Part 2 and Part 3 predictions were not made before running and are left for the student to complete. The student is responsible for checking this file before submitting it.
    - 
