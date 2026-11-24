# Perplexity Explorer

An interactive, single-file browser application demonstrating how causal language models calculate **perplexity** token-by-token and predict next tokens.

Powered by a real language model running completely in the browser via [Transformers.js](https://huggingface.co/docs/transformers.js) — no backend, API keys, or frameworks required.

**Live Demo**: [https://spsr-dot.github.io/perplexity-explorer/](https://spsr-dot.github.io/perplexity-explorer/)

---

## Features

- **Section 1 – Type a Sentence**:
  - Run inference with **DistilGPT-2** (`Xenova/distilgpt2`, 8-bit quantized) in the browser via WebAssembly/ONNX.
  - Pre-filled sample sentences for instant comparison:
    - `"I love Python"`
    - `"Python love I"`
    - `"The capital of France is Paris"`
  - Start marker token (`50256`) prepended so the first word also receives a probability.

- **Section 2 – Probability $\rightarrow$ Perplexity**:
  - Full breakdown table with token IDs, decoded token strings (spaces rendered as `␣`), probabilities $P$, and inverse probabilities $1/P$.
  - Stable softmax computation (subtracting max logit) preventing numeric instability.
  - Step-by-step breakdown:
    - **Step 1**: Multiplication of all $1/P$ values.
    - **Step 2**: Total token count $N$.
    - **Step 3**: Large perplexity display calculated safely via log-space: $\exp(\text{mean}(-\ln P))$.

- **Section 3 – Next Token Predictions**:
  - Evaluates logits at the last position to show the top 8 candidates.
  - Interactive horizontal bars with exact percentage probabilities and visual highlight for the top token.

- **Section 4 – Try It by Hand (Simulated)**:
  - Interactive manual workbench to build custom token probability distributions.
  - Includes presets:
    - `"I love Python"` ($0.5, 0.8, 0.2$) $\rightarrow$ Perplexity **2.32**
    - `"All 1/8"` ($8\text{ tokens @ } 0.125$) $\rightarrow$ Perplexity **8.00**
    - `"One surprising word"` ($0.9, 0.9, 0.9, 0.01$) $\rightarrow$ Perplexity **3.42**
  - Live recalculation on every keystroke.

---

## Running Locally

Simply open `index.html` in any modern web browser or serve it with any static HTTP server:

```bash
# Python 3
python3 -m http.server 8000
```

Then visit `http://localhost:8000`.

---

## License

MIT
