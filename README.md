# Build an LLM from Scratch

> Coding a transformer-based language model from absolute zero — every line by hand, on a MacBook.

![Status](https://img.shields.io/badge/status-in%20progress-yellow)
![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![PyTorch](https://img.shields.io/badge/pytorch-2.x-red)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Why This Repo Exists

Most LLM tutorials hand you a library and tell you to call `model.generate()`. This repo does the opposite: **we build the whole thing from scratch.** No `transformers` library, no shortcuts — just PyTorch, math, and a relentless focus on understanding *every* line of code that goes into a modern language model.

The goal is not to compete with GPT-4. The goal is to walk out the other side knowing exactly how a transformer works, how it trains, how it generates text, and what every knob in the architecture actually does.

By the end, we'll have a small but real LLM that can answer questions about content it was trained on — running on a laptop, no GPU cluster required.

## What You'll Find Here

This repo is a step-by-step journey. Each phase has its own folder, its own code, and its own notes explaining *why* the code looks the way it does.

What we'll cover:

- **Tokenization** — building a BPE tokenizer from scratch
- **Embeddings** — token + positional, including learned and RoPE variants
- **Self-attention** — single-head, then multi-head, written from first principles
- **Transformer blocks** — LayerNorm, residual streams, feed-forward networks
- **The full GPT decoder** — stacking it all into a working model
- **Training loop** — AdamW, gradient clipping, LR schedules, mixed precision
- **Datasets & data loading** — tokenizing corpora, packing sequences, efficient batching
- **Sampling** — greedy, top-k, top-p (nucleus), temperature
- **Fine-tuning** — instruction tuning so the model can actually answer questions
- **Evaluation** — perplexity, qualitative checks, basic eval harnesses
- **(Stretch) Inference optimizations** — KV cache, quantization

## The Architecture

We're building a **decoder-only transformer** in the GPT family. Here's the high-level picture:

```
Input tokens
    │
    ▼
[Token Embeddings] + [Positional Embeddings]
    │
    ▼
┌─────────────────────────────────┐
│   Transformer Block × N         │
│   ┌─────────────────────────┐   │
│   │   LayerNorm             │   │
│   │   Multi-Head Attention  │   │
│   │   Residual              │   │
│   │   LayerNorm             │   │
│   │   Feed-Forward (MLP)    │   │
│   │   Residual              │   │
│   └─────────────────────────┘   │
└─────────────────────────────────┘
    │
    ▼
[Final LayerNorm]
    │
    ▼
[LM Head — projection to vocab]
    │
    ▼
Output logits
```

Starting target: **~10M parameters**, character-level or small BPE vocab, trained on a manageable corpus. We scale up from there as time, patience, and laptop thermals allow.

## Roadmap

### Phase 1 — Foundations (In Progress)
- [x] Set up dev environment (PyTorch + MPS on macOS)
- [x] Text preprocessing and tokenization
- [x] Building simple tokenizers (V1 and V2)
- [x] Byte Pair Encoding with tiktoken
- [x] Dataset with sliding window for input-target pairs
- [x] Token embeddings and positional embeddings
- [ ] Implement a bigram baseline model (sanity check)

### Phase 2 — Core Transformer
- [ ] Implement scaled dot-product attention
- [ ] Multi-head attention from scratch
- [ ] Feed-forward block, LayerNorm, residuals
- [ ] Assemble the full transformer block
- [ ] Train first ~10M parameter GPT on Shakespeare

### Phase 3 — Real Tokenization & Bigger Data
- [ ] Implement BPE tokenizer from scratch
- [ ] Train on TinyStories (or similar)
- [ ] Add positional encoding variants (learned vs RoPE)
- [ ] Mixed precision (bf16) training

### Phase 4 — Sampling & Generation
- [ ] Greedy decoding
- [ ] Top-k and nucleus (top-p) sampling
- [ ] Temperature, repetition penalty
- [ ] Build an interactive chat loop

### Phase 5 — Fine-tuning
- [ ] Curate an instruction dataset
- [ ] Implement supervised fine-tuning (SFT)
- [ ] Evaluate on held-out QA

### Phase 6 — (Stretch) Optimization
- [ ] KV cache for fast inference
- [ ] Basic quantization
- [ ] Compare with HuggingFace equivalents

## Project Structure

```
.
├── README.md
├── CLAUDE.md              # Project context for development
├── requirements.txt
├── setup.md               # Environment setup guide
├── notebooks/             # Step-by-step implementation notebooks
│   ├── 01_text_preprocessing.ipynb
│   ├── 02_simple_tokenizers.ipynb
│   ├── 03_bpe_tokenization.ipynb
│   ├── 04_dataset_dataloader.ipynb
│   └── 05_embeddings.ipynb
├── src/                   # Python modules (created after notebook validation)
│   ├── tokenizer/
│   ├── model/
│   ├── training/
│   ├── data/
│   └── inference/
├── data/                  # Raw datasets (gitignored)
├── checkpoints/           # Trained weights (gitignored)
├── docs/                  # Notes & explanations per phase
└── tests/                 # Unit tests
```

*Structure will evolve as the project grows.*

## Hardware

This entire repo is built on a **MacBook (16GB RAM, Apple Silicon)** to prove the point that you don't need a GPU farm to learn this stuff. The stack:

- **PyTorch with MPS backend** for GPU acceleration via Apple's Metal
- **Mixed precision (bf16)** where it helps
- Models small enough to actually train in reasonable time

Suggested specs to follow along comfortably:

- 8GB+ RAM (16GB+ recommended from Phase 3 onward)
- Python 3.10+
- macOS (MPS), Linux/Windows (CUDA or CPU)

For anything beyond Phase 5, free-tier Google Colab or Kaggle GPUs are a solid escape hatch.

## Quick Start

```bash
# Clone the repo
git clone https://github.com/<your-username>/llm-from-scratch.git
cd llm-from-scratch

# Set up environment (using uv for speed)
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt

# Start Jupyter and open the notebooks
jupyter lab
```

Start with `notebooks/01_text_preprocessing.ipynb` and work through in order.

*Detailed setup instructions in [setup.md](setup.md).*

## Learning Resources

This journey stands on the shoulders of giants. Highly recommended companions:

- **Andrej Karpathy** — *Let's build GPT: from scratch, in code, spelled out* ([YouTube](https://www.youtube.com/watch?v=kCc8FmEb1nY))
- **Sebastian Raschka** — *Build a Large Language Model (From Scratch)* (Manning, 2024)
- **Vaswani et al., 2017** — *Attention Is All You Need*
- **Radford et al., 2019** — *Language Models are Unsupervised Multitask Learners* (GPT-2)
- **nanoGPT** — Karpathy's reference implementation
- **The Annotated Transformer** — Harvard NLP

## Follow the Journey

I'm documenting this build in public. Each phase ships with notes, and key milestones will get longer write-ups.

- LinkedIn *(update with your handle)*
- YouTube *(update with your channel)*
- Phase-by-phase notes live in `docs/`

If you spot a bug, a better way to do something, or just want to follow along — open an issue or drop a star.

## Acknowledgments

Massive thanks to **Andrej Karpathy** and **Sebastian Raschka**, whose teaching makes journeys like this possible.

## License

MIT — see [LICENSE](LICENSE).
