# Strands Decider - Installation & Quickstart Guide

`strands-decider` is a high-speed, local classification engine designed for AI agent workflows. Instead of slowly generating open-ended text token-by-token like a standard LLM, it evaluates a statement against a fixed list of options and instantly calculates a mathematical match score for each choice in a single forward pass.

This guide uses `uv` by Astral to manage specific Python versions and execute fast isolated installations.

---

## 🛠️ Installation with uv

Follow these steps to set up a clean Python environment and install the required tools on macOS using `uv`.

### 1. Create a Specific Python Virtual Environment
Open your Terminal and run the following commands to initialize an environment locked to your target Python version (e.g., Python 3.12):

```bash
# Create a dedicated directory
mkdir strands-decider-handson
cd strands-decider-handson

# Create a virtual environment using a specific Python version (-p flag)
uv venv -p 3.12 .venv

# Activate the virtual environment
source .venv/bin/activate
```
*(Note: If you need a different version, simply swap out `3.12` for your required version, like `3.11` or `3.13`).*

### 2. Install the Package via uv
Install the package runtime directly using `uv pip`:

```bash
uv pip install strands-decider
```
> **Note:** This installation is extremely fast and lightweight. It sets up the execution runtime and CLI bindings. The heavy AI weights (~4.5 GB) are not downloaded until you execute your first decision task.

---

## 🚀 Quickstart & Testing

Run this command to test your installation. On the **very first run**, the runtime will automatically reach out to Hugging Face to stream down and cache the underlying model architecture.

```bash
strands-decider ask StrandsAgents/strands-decider-2B-hobson-v19 \
  --state "Help! My payouts have been failing for 3 days!" \
  --choice "Which team should handle this?=billing,sales,retail"
```

### Understanding the Output
Once the processing completes, you will see a text matrix printed directly to your console:

```text
choice_0 -> billing (confidence 0.768)
    billing                  0.845
    retail                   0.091
    sales                    0.064
```

* **The Winner (`choice_0 -> billing`):** The model declares `billing` as the most appropriate destination for this input.
* **The Match Score Matrix:** The values next to each choice function like a percentage match out of `1.00`. The model identifies an 84.5% structural match for `billing`.
* **The Confidence Score (`0.768`):** This value is a calculated mathematical margin of certainty. It tells you how cleanly the winning choice outperformed the runners-up. 

📖 **Looking for more experimentation scenarios?** Additional multi-domain classification scripts, sample inputs, and configuration commands can be found in the adjacent [ASKS.md](./ASKS.md) file.

---

## ⚙️ macOS Performance Optimization

The underlying library will inspect your system topology at runtime. If you are operating on **Apple Silicon (M1/M2/M3/M5)** chips, the matrix math engine will automatically map tensor execution to your integrated **MPS (Metal Performance Shaders)** pipeline, taking advantage of your Mac's unified high-speed memory.

### Optimizing Kernels
If you see a notice regarding a missing `causal_conv1d` reference implementation fall-back in your terminal, the engine is running on generic software math paths. While functional, you can speed up processing execution significantly for production workloads by installing the hardware-optimized kernel extension locally if your environment meets compilation requirements.
