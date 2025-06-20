# Manim Code Generation with Fine-tuned QWEN Model

This project fine-tunes a QWEN2.5-Coder model on the ManimBench dataset to generate high-quality Manim animation code from natural language descriptions.

## Overview

- **Base Model**: QWEN2.5-Coder-1.5B-Instruct (4-bit quantized)
- **Training Method**: QLoRA with Unsloth optimizations
- **Dataset**: ManimBench v1 (417 samples)
- **Deployment**: Ollama-compatible GGUF format
- **GPU Requirement**: 16GB VRAM (tested on NVIDIA A4500)

## Features

- 🚀 Efficient 4-bit quantized training with Unsloth
- 📊 Generates syntactically valid Manim code
- 🔧 Easy deployment with Ollama
- 🧪 Comprehensive testing suite
- 💾 Memory-efficient training (10-12GB VRAM usage)

## Quick Start

### Prerequisites

1. NVIDIA GPU with 16GB+ VRAM
2. Python 3.8+
3. Ollama installed (for deployment)
4. Virtual environment `manim-env` created

### Installation & Training

```bash
# Clone the repository
cd manim-post-training

# Create virtual environment (if not exists)
python -m venv manim-env

# Run the complete pipeline
./setup_and_train.sh
```

This script will:
1. Install all dependencies
2. Prepare the ManimBench dataset
3. Fine-tune the model (~30-45 minutes)
4. Convert to Ollama format
5. Run validation tests

### Using the Model

#### With Ollama
```bash
# Basic usage
ollama run manim-coder "Create an animation showing a circle transforming into a square"

# More examples
ollama run manim-coder "Animate a sine wave being drawn"
ollama run manim-coder "Show the Pythagorean theorem visually"
```

#### Interactive Mode
```bash
# Activate environment
source manim-env/bin/activate

# Run interactive mode
python test_inference.py --interactive
```

## Project Structure

```
manim-post-training/
├── prepare_data.py          # Dataset preparation script
├── fine_tune.py             # Universal training script for multiple models
├── universal_tokenizer_setup.py  # Model-specific tokenizer configuration
├── tokenizer_configs.md     # Reference guide for model tokenizers
├── convert_to_ollama.py     # Ollama conversion script
├── test_inference.py        # Testing and inference utilities
├── evaluate_model.py        # Comprehensive evaluation with metrics
├── evaluate_capacity.py     # Compare base vs fine-tuned capacity
├── detect_overfitting.py    # Check for memorization vs learning
├── setup_and_train.sh       # One-click setup and training
├── requirements.txt         # Python dependencies
├── data/                    # Processed datasets
│   ├── train.json
│   └── test.json
├── models/                  # Model outputs
│   ├── lora_model/         # LoRA adapters
│   ├── merged_model/       # Full merged model
│   └── manim-coder.gguf    # Ollama model
├── logs/                    # Training logs
├── test_results.txt         # Inference test results
├── evaluation_report.txt    # Comprehensive evaluation metrics
├── capacity_evaluation_report.txt  # Capacity comparison report
└── overfitting_report.txt   # Overfitting detection results
```

## Training Details

### Model Configuration
- **LoRA Rank**: 16
- **Target Modules**: q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj
- **Learning Rate**: 2e-4 with cosine scheduler
- **Batch Size**: 4 with gradient accumulation (effective batch size: 16)
- **Epochs**: 3
- **Max Sequence Length**: 2048 tokens

### Expected Performance
- **Training Time**: ~30-45 minutes on 16GB GPU
- **Memory Usage**: 10-12GB VRAM
- **Final Loss**: ~0.5
- **Code Validity**: >90% syntactically valid outputs

### Expected Evaluation Metrics

After fine-tuning on ManimBench, you should expect:

| Metric | Expected Range | Description |
|--------|---------------|-------------|
| **Perplexity** | 5-15 | Lower is better, <10 is good |
| **Syntax Valid Rate** | >90% | Percentage of syntactically correct code |
| **Has Manim Objects** | >95% | Uses Circle, Text, etc. |
| **Has Animations** | >90% | Uses play(), animate, etc. |
| **ROUGE-L F1** | 0.3-0.5 | Code similarity to references |
| **BERTScore F1** | 0.7-0.9 | Semantic similarity |
| **Generation Time** | 1-3s | Per code snippet |

These metrics are automatically tracked in Weights & Biases for easy monitoring.

## Dataset Information

The ManimBench v1 dataset contains:
- **Total Samples**: 417 (317 train, 100 test)
- **Complexity Distribution**:
  - Basic: 62%
  - Intermediate: 33%
  - Advanced: 5%
- **Average Code Length**: ~500 characters

## Testing & Evaluation

### Quick Tests
Run basic inference tests:
```bash
python test_inference.py
```

This will:
- Test 10 different animation scenarios
- Validate generated code structure
- Save results to `test_results.txt`

### Comprehensive Evaluation
Run full model evaluation with metrics:
```bash
python evaluate_model.py
```

This calculates:
- **Perplexity**: Model's uncertainty on test set
- **Code Quality Metrics**:
  - Syntax validity rate
  - Presence of required Manim components
  - Code structure analysis
- **Similarity Metrics**:
  - ROUGE scores (lexical similarity)
  - BERTScore (semantic similarity)
- **Performance Metrics**:
  - Generation time
  - Token statistics

Results are:
- Logged to Weights & Biases project `manim-post-train`
- Saved to `evaluation_report.txt`

### Metrics Dashboard

All training and evaluation metrics are logged to Weights & Biases:

1. **Training Metrics** (logged during fine-tuning):
   - Loss curves
   - Learning rate schedule
   - Gradient norms
   - Training speed (tokens/sec)

2. **Post-Training Metrics** (logged during evaluation):
   - Code quality scores
   - Generation performance
   - Model perplexity
   - Similarity to reference implementations

Access your metrics at: https://wandb.ai/[your-username]/manim-post-train

### Setting up Weights & Biases

```bash
# Login to wandb (one-time setup)
wandb login

# Or set API key
export WANDB_API_KEY=your_api_key_here

# Disable wandb (offline mode)
export WANDB_MODE=offline
```

## Evaluating Model Capacity

### Quick Check: Did Training Actually Work?

To determine if fine-tuning increased the model's capacity (vs just memorizing), run:

```bash
# Compare base vs fine-tuned model
python evaluate_capacity.py --visualize

# Check for overfitting
python detect_overfitting.py
```

### Key Indicators of True Capacity Increase

1. **Generalization to Novel Prompts** ✅
   - Model performs well on prompts very different from training data
   - Example: "Create a 4D hypercube projection" (not in training)

2. **Compositional Understanding** ✅
   - Can combine learned concepts in new ways
   - Example: Combines "rotating" + "hexagon" + "color change" correctly

3. **Consistent Quality Across Complexity** ✅
   - Maintains performance on simple → complex prompts
   - Doesn't just work on memorized examples

4. **Low Direct Memorization** ✅
   - Generates different code for same training prompts
   - Character-level similarity < 90% to training examples

5. **Robust to Distribution Shift** ✅
   - Performance degrades gracefully (not cliff-like) on OOD inputs
   - Still produces valid Manim code for unusual requests

### Red Flags of Overfitting

- 🚩 Exact reproduction of training examples
- 🚩 Fails completely on slightly modified prompts  
- 🚩 Can't interpolate between learned concepts
- 🚩 Dramatic performance drop on new prompt types

### Interpreting the Reports

**capacity_evaluation_report.txt** shows:
- Overall capacity improvement score (>20% is significant)
- Performance comparison across different test categories
- Specific improvements in code quality metrics

**overfitting_report.txt** shows:
- Memorization analysis (exact match rate should be <20%)
- Interpolation ability (novel outputs should be >70%)
- Distribution shift robustness (degradation should be <30%)
- Compositional generalization scores

## Troubleshooting

### CUDA Out of Memory
- Reduce batch size in `fine_tune.py`
- Increase gradient accumulation steps
- Use a smaller sequence length

### Ollama Model Creation Fails
- Ensure Ollama is running: `ollama serve`
- Check GGUF conversion succeeded
- Verify model path in Modelfile

### Poor Generation Quality
- Train for more epochs (increase NUM_EPOCHS)
- Adjust temperature in inference
- Fine-tune on additional Manim examples

## Advanced Usage

### Custom Training
```python
# Modify training parameters in fine_tune.py
BATCH_SIZE = 2  # Reduce for less VRAM
NUM_EPOCHS = 5  # Train longer
LEARNING_RATE = 1e-4  # Lower learning rate
```

### Custom Prompts
```python
# In test_inference.py, modify TEST_PROMPTS
TEST_PROMPTS = [
    "Your custom animation prompt",
    # Add more...
]
```

## Contributing

Feel free to:
- Add more training data
- Improve model architecture
- Enhance validation metrics
- Create better prompts

## License

This project uses:
- QWEN model (Apache 2.0)
- ManimBench dataset (check original license)
- Unsloth library (Apache 2.0)

## Acknowledgments

- Unsloth team for optimization library
- QWEN team for the base model
- ManimBench creators for the dataset
- Manim community for the animation framework