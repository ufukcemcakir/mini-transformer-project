This is a toy project I've developed to learn the basics of language model training. My goal is to approach the process in an iterative manner and step-by-step improve the work.

## V4
**What's changed?**

In V4, I downgraded some hyper-parameters again because I had another major update: I used the full work of Shakespeare, as gratiously prepared by the Project Gutenberg.

Summary of results:

- Successfully trained on ~1.4M tokens

- Model outputs became more structurally Shakespearean, including scene directions and character dialogue.

- Diversity decreased slightly, as the model leaned more on real Shakespeare patterns.

- N-gram overlap increased (especially trigrams), showing stronger corpus alignment and reduced randomness.

- Style similarity (~0.42) remained solid but slightly lower due to noisy Gutenberg formatting.

- Overall training remained stable with steadily improving loss and perplexity.

## V3
**What's changed?**

I have access to a new, strong hardware at the moment. That's why I wanted to try a harder, longer taking process to improve the model. I also added some new evaluation metrics. The changelog can be found below:

| Parameter    | v2   | v3    |
| ------------ | ---- | ----- |
| `vocab_size` | 2000 | 4000  |
| `block_size` | 128  | 256   |
| `batch_size` | 32   | 64    |
| `max_iters`  | 5000 | 10000 |


Version 3 has an evaluation suite to quantify text diversity, memorization, and stylistic similarity.

Added:

*distinct-1* and *distinct-2*

These measure token-level and bigram-level diversity in generated text.

*n-gram precision* *n-gram recall* *F1 score* *novel n-gram ratio*

This allows measuring how much of the model’s output overlaps with true Shakespeare vs. how much is newly generated.

Summary of results:

- Training converged smoothly with reduced loss and stable validation curves.

- Model generated stylistically Shakespearean text but with limited coherence due to small dataset size.

- Diversity metrics (distinct-1/2) showed healthy variability without mode collapse.

- N-gram overlap indicated strong mimicry of Shakespeare’s local phrasing and moderate creativity.

- Style-embedding similarity (~0.49) confirmed outputs were recognizably Shakespeare-like without memorization.

## V2
**What's changed?**
I did not change the hyper-parameters because I wanted to be able to compare the first two versions based on architectural features.

- Introduced a config dict to hold all the hyper-parameters.
- Changed the character-level tokenizer to a Byte Pair Encoding using HuggingFace tokenizer.
- Changed the tokenizer logic so that encoding and decoding functions can take different tokenizers.
- Changed the generation logic in the GPTLanguageModel class. Now it can take parameters like temperature and top_k.
- Implemented a different learning rate scheduler with warmup and cosine decay
- Added weight tying
- Plotted not only cross entropy loss but also perplexity
- Changed the decoder that generates text so that it is customizable with different prompts and temperatures

The results seem better in a general sense:

- Proper grammar and punctuation
- Shakespearean style
- Character names and context
- Proper dialogue format
- Stays on topic with royal/noble themes

## V1
It trains a small Transformer (≈10.7M parameters) capable of generating text in Shakespearean style. It reproduces core components of modern language models such as:

- Multi-head self-attention
- Layer normalization
- Residual connections
- Character tokenization and positional embeddings
- The training corpus is input.txt (≈1.1M characters) from the Tiny Shakespeare dataset

## Architecture Summary
- Model Size: ~10.74M parameters
- Context Window (block size): 128 characters
- Embedding Dimension: 384
- Attention Heads: 6
- Transformer Layers: 6

## Hyperparameters
| Hyperparameter      | Value     |
| ------------------- | --------- |
| Batch size          | 32        |
| Learning rate       | 3e-4      |
| Dropout             | 0.2       |
| Optimizer           | AdamW     |
| Training iterations | 5000      |
| Evaluation interval | 500 steps |
| Context length      | 128       |

## Results
| Step | Train Loss | Val Loss |
| ---- | ---------- | -------- |
| 0    | 1.88       | 1.97     |
| 1000 | 1.51       | 1.70     |
| 2500 | 1.34       | 1.56     |
| 4999 | **1.22**   | **1.50** |

After 5000 iterations, the model can generate coherent pseudo-Shakespearean text, although there are grammatical errors:
KING RICHARD III:
Neither is another.

QUEEN ELIZABETH:
Here's sociatpy counts, boy a fellow is marriage,
Make proaching in the friends, corrow right,
Which trives this, it best, gallest, die...
