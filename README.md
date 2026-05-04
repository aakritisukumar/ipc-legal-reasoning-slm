# ipc-legal-reasoning-slm

Enabling Chain-of-Thought reasoning in 1B parameter models through novel two-stage training methodology

## 🎯 Key Results

- ** accuracy improvement**  on critical IPC provisions
- **0 training cost** using free Google Colab
- **Zero factual hallucination** on covered legal sections

## 📊 Quick Comparison

| Model | Training Method | Accuracy | Has Reasoning | Training Time |
|-------|----------------|----------|---------------|---------------|
| Single-Stage | Facts + Reasoning together | 33% | ✓ | 3 min |
| No Reasoning | Facts only | 100% | ✗ | 3 min |
| **Two-Stage** | **Facts → Reasoning** | **100%** | **✓** | **10 min** |

## 🚀 Quick Start

### Option 1: Google Colab (Recommended)

1. Click the "Open in Colab" badge above
2. Upload `data/complete_ipc_dataset_500.json` when prompted
3. Run all cells
4. Training completes in ~10 minutes on free T4 GPU

### Option 2: Local Setup

```bash
git clone https://github.com/YOUR_USERNAME/ipc-legal-reasoning-slm.git
cd ipc-legal-reasoning-slm
pip install -r requirements.txt
jupyter notebook notebooks/ipc_training_complete.ipynb
```

## 💡 The Problem

Small language models (1B parameters) face capacity constraints when learning multiple complex patterns. Training with reasoning chains causes **catastrophic forgetting** of factual knowledge.

**Example:**
- ❌ Single-stage training: "Section 302 = culpable homicide, 10 years" (WRONG!)
- ✓ Two-stage training: "Section 302 = murder, death or life imprisonment" (CORRECT!)

## 🔬 The Solution

**Two-Stage Training Methodology:**

**Stage 1: Facts Only (5 epochs)**
- Train without reasoning chains
- Model focuses 100% capacity on memorizing legal facts
- Loss: 2.04 → 0.27 (87% reduction)

**Stage 2: Add Reasoning (3 epochs, lower LR)**
- Train with reasoning chains
- Reduced learning rate (5e-5) preserves facts
- Loss: 1.40 → 0.69 (51% reduction)

**Result:** Both factual accuracy AND reasoning structure ✓


## 📁 Repository Structure

```
ipc-legal-reasoning-slm/
├── README.md
├── requirements.txt
├── LICENSE
├── notebooks/
│   └── ipc_training_complete.ipynb    # Complete training pipeline
├── data/
│   ├── complete_ipc_dataset_500.json  # 500 reasoning examples
│   └── README.md
├── results/
│   └── evaluation_results.json        # Model comparison results
└── models/
    └── README.md                       # Model download instructions
```

## 📊 Dataset

- **Size:** 500 examples
- **Coverage:** 150 IPC sections (29% of total, 95% of real cases)
- **Format:** Question + 5-step reasoning + comprehensive answer
- **Sections:** Murder (302), dowry death (304B), theft (379), robbery (390-398), assault, kidnapping, etc.

## 🎓 Key Findings

1. **Catastrophic Forgetting:** Single-stage CoT training causes 67% factual hallucination in 1B models
2. **Sequential Learning:** Two-stage approach eliminates factual errors while maintaining reasoning
3. **Two Hallucination Types:** 
   - Factual (dangerous) - eliminated by two-stage training
   - Generative (annoying) - persists across all architectures
4. **Small Models Can Reason:** With proper training, 1B models achieve complex domain reasoning

## ⚠️ Limitations

- **Evaluation scope:** Tested on held-out data from same distribution
- **Coverage:** 150 out of 511 IPC sections (361 sections not covered)
- **Generalization:** Real-world performance on truly unseen queries unknown
- **Status:** Proof-of-concept for training methodology, not production system

## 🔮 Future Work

- [ ] Evaluate on truly unseen legal questions
- [ ] Expand to all 511 IPC sections
- [ ] Test on larger models (3B, 7B)
- [ ] Address generative hallucination
- [ ] Benchmark on IL-TUR dataset
- [ ] User studies with law students
