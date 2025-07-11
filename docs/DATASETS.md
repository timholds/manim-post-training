# Base Dataset (No Augmentation) - July 10, 2025 ✅

## Dataset Field Mapping and Unified Format

All datasets are converted to a unified conversation structure for training:

```json
{
  "conversations": [
    {
      "from": "system",
      "value": "You are a Manim code generator. Create clean, working Manim animations using ManimCE syntax. Always wrap code in Python code blocks."
    },
    {
      "from": "user", 
      "value": "<The Manim animation description/question>"
    },
    {
      "from": "assistant",
      "value": "```python\n<The Manim code>\n```"
    }
  ],
  "source": "<dataset_name>"
}
```

### Field Mappings by Dataset

| Dataset | Source Type | User Prompt Field | Code Field | Samples |
|---------|-------------|------------------|------------|---------|
| manimbench | Kaggle | `"Reviewed Description"` | `"Code"` | 400 |
| bespoke_manim | HuggingFace | `"question"` | `"python_code"` | 1000 |
| thanks_dataset | HuggingFace | `"input"` | `"output"` | 4400 |
| dan4life_aoc2024 | Local | `conversations[1].value` | `conversations[2].value` | 24 |
| szymon_ozog | Local | `conversations[1].value` | `conversations[2].value` | 29 |

Each sample includes a `"source"` field for tracking dataset origin, enabling quality analysis, debugging, and weighted sampling during training.

## Statistics
- **Total Raw Samples**: 5,812
- **After Deduplication**: 3,827 (34.2% reduction)
- **Training Set**: 3,445 samples (no augmentation, 1.0x)
- **Test Set**: 382 samples
- **Output Location**: `data_base_no_augmentation/`

## Source Distribution (After Deduplication)
- **Thanks Dataset**: 2,167 samples (62.9% of training set)
- **Bespoke Manim**: 903 samples (26.2% of training set)
- **ManimBench**: 375 samples (10.9% of training set)

## Key Features
- **No augmentation** - each unique sample appears exactly once
- **Deduplication by default** - removes 1,985 duplicate descriptions
- **Focused on quality** - clean, tight dataset without prompt variations

---

# Existing Datasets: 

## Successfully Integrated (July 9, 2025)
- **Bespoke Manim** (1000 examples) ✅
  - Source: https://huggingface.co/datasets/bespokelabs/bespoke-manim
  - Contains: code, rich descriptions, transcripts, some videos
  - Status: Fully integrated - 1,000 samples processed (schema fixed July 11, 2025)
  
- **Thanks Dataset** (4400 examples) ✅
  - Source: https://huggingface.co/datasets/thanhkt/manim_code
  - Contains: code, instructions
  - Status: Fully integrated - 4,395 samples processed
  
- **ManimBench** (417 examples) ✅
  - Source: https://www.kaggle.com/datasets/ravidussilva/manim-sft/data
  - Contains: code, reviewed descriptions, proper train/test split
  - Status: Fully integrated - 417 samples processed
  - Note: 100% unique content, no overlap with other datasets

## Current Dataset Statistics (WITH Source Tracking)
- **Total Raw Samples**: 7,434
- **Training Samples**: 16,747 (with 2.5x augmentation)
- **Test Samples**: 743
- **Output Location**: `data_formatted_with_sources/`

### Source Distribution in Training Set:
- Thanks Dataset: 9,955 samples (59.4%)
- ManimCodeGen: 3,616 samples (21.6%)
- Bespoke Manim: 2,253 samples (13.5%)
- ManimBench: 923 samples (5.5%)

## Deduplication Applied (July 10, 2025) ✅

### Deduplication Results
- **Original dataset**: 7,434 samples
- **After deduplication**: 3,827 samples (48.5% reduction!)
- **Duplicates removed**: 3,607
  - Cross-source duplicates: 1,272
  - Within-source duplicates: 1,032

### Final Source Distribution (Deduplicated)
- ManimBench: 414 samples (99.3% retained - highest quality)
- Bespoke Manim: 1,000 samples (100% retained)
- Thanks Dataset: 2,413 samples (54.9% retained)
- ManimCodeGen: 0 samples (100% were duplicates!)

### Key Findings
- **ManimCodeGen had 100% overlap** with other datasets (all 1,622 samples were duplicates)
- **ManimBench maintained highest retention** due to priority for reviewed descriptions
- **Thanks Dataset had significant internal duplication** (1,982 samples removed)

### Output Location
- **Deduplicated dataset**: `data_formatted_deduplicated/`
  - Training samples: 8,600 (with 2.5x augmentation)
  - Test samples: 382
- **Reports**: 
  - `deduplication_report.json` - Full statistics
  - `removed_duplicates.json` - Examples of removed items

## Successfully Integrated (July 11, 2025) - Dan4Life AoC2024
- **Dan4Life AoC2024** (24 examples) ✅
  - Source: https://github.com/Dan4Life/AoC2024_Videos
  - Contains: Advent of Code 2024 visualization code
  - Status: Fully integrated - 24 samples processed (23 unique days + 1 version)
  - Note: High-quality problem-solving animations, no overlap with existing datasets

## Current Dataset Statistics (INCLUDING Dan4Life)
- **Total Raw Samples**: 4,836
- **After Deduplication**: 2,851 (41.0% reduction)
- **Training Samples**: 2,566 (no augmentation)
- **Test Samples**: 285
- **Output Location**: `data_all_with_dan4life/`

### Source Distribution (After Deduplication):
- Thanks Dataset: 2,166 samples (75.9%)
- ManimBench: 379 samples (13.3%)
- Dan4Life AoC2024: 21 samples (0.7%)
- Bespoke Manim: 0 samples (loading error)

## Current Dataset Statistics (INCLUDING Szymon Ozog - July 11, 2025)
- **Total Raw Samples**: 5,865
- **After Deduplication**: 3,869 (34.0% reduction)
- **Training Samples**: 3,483 (no augmentation)
- **Test Samples**: 386
- **Output Location**: `data_formatted/`

### Source Distribution (After Deduplication):
- Thanks Dataset: 2,176 samples (56.2%)
- Bespoke Manim: 896 samples (23.2%) ✅ Fixed!
- ManimBench: 375 samples (9.7%)
- Dan4Life AoC2024: 21 samples (0.5%)
- Szymon Ozog: 15 samples (0.4%)

## Szymon Ozog Datasets (July 11, 2025) ✅

### Overview
Two specialized repositories with educational animations:
- **Information Theory Videos** 
  - Source: https://github.com/SzymonOzog/InformationTheory
  - YouTube playlist: https://www.youtube.com/watch?v=-j2Z2heUBYc&list=PL5XwKDZZlwaZi041-mB8zd6APQjb5AkBv
  - Topics: Entropy, communication systems, binary symmetric channel
  - Samples: 10 scenes extracted from 2 files
  
- **GPU Programming Videos**
  - Source: https://github.com/SzymonOzog/GPU_Programming
  - YouTube playlist: https://www.youtube.com/watch?v=c8mQYGbT310&list=PL5XwKDZZlwaY7t0M5OLprpkJUIrF8Lc9j
  - Topics: CUDA architecture, memory hierarchy, tensor cores, optimization
  - Samples: 19 scenes extracted from 18 files

### Integration Status
- **Total samples extracted**: 29 (28 after deduplication)
- **Samples with YouTube metadata**: 28/29
- **Description status**: Placeholder descriptions with YouTube metadata for transcript enhancement
- **Unique features**: VoiceoverScene integration, specialized technical content

### Technical Implementation
- Created `extract_szymon_ozog_enhanced.py` with YouTube URL mappings
- All samples marked with `needs_description_update: true` for transcript-based enhancement
- Metadata includes video titles, playlist URLs, and episode/index information
- Ready for on-demand transcript fetching during LLM description generation

## Dan4Life Dataset - Enhanced Approach with LLM Descriptions

### Why LLM-Generated Descriptions?
The initial approach of generating generic descriptions like "Visualize Advent of Code 2024 Day X" doesn't create meaningful training data. The user prompts should actually correspond to what the animation shows. For example:
- **Generic**: "Visualize the solution to Day 22"
- **LLM-Generated**: "Create an animation that demonstrates a pseudorandom number generator algorithm. Start by showing how XOR operations work with binary representations..."

### Two-Phase Processing Pipeline
1. **Phase 1: Code Extraction** (`extract_dan4life_enhanced.py`)
   - Extract all Manim code from repositories
   - Analyze code features (has_xor, has_graphs, main_elements)
   - Create placeholder descriptions
   - Store metadata for later processing

2. **Phase 2: LLM Description Generation** (`process_llm_descriptions.py`)
   - Batch process all code samples with LLM
   - Generate natural user requests based on code analysis
   - Mark samples with `description_generated_by: "llm_gpt4"`
   - Enable differential augmentation (3x for LLM vs 1.5x for human)

### Benefits of This Approach
- **Quality**: Descriptions actually match what the code produces
- **Efficiency**: Batch LLM calls instead of per-run generation
- **Flexibility**: Can update descriptions without re-extracting code
- **Tracking**: Metadata tracks description source for experiments
- **Augmentation**: LLM samples can have more aggressive variations

### Caching Strategy
Instead of calling LLM every time we run `prepare_data_enhanced.py`:
1. Extract code once and save with placeholders
2. Generate descriptions once with LLM (can be cached by code hash)
3. Store final dataset with descriptions
4. Only regenerate if new code is added

### YouTube Transcripts
Rather than storing transcripts now, it's better to:
1. Fetch transcripts when generating descriptions
2. Use `claude -p` to analyze code + transcript together
3. Generate more accurate descriptions with full context

# Potential Datasets for Future Integration
- Manim CE Examples, https://docs.manim.community/en/stable/examples.html, code yes, video yes, transcript no, date no  
- Manim Repository, https://themanimrepository.wordpress.com/, code yes, video yes, transcript no, date yes  
- Manim CE Awesome manim, https://github.com/ManimCommunity/awesome-manim (look at README to find those with GitHub/youtube pairs)  
- Dan4Life, videos yes https://www.youtube.com/@dan4life/videos, code yes https://github.com/Dan4Life/AoC2024_Videos, 
- A Little More Than An Introduction To Series - code yes https://github.com/JonathanWoollett-Light/a-little-more-than-an-introduction-to, video yes videos https://www.youtube.com/channel/UCze6YPZo6gzj-Nup2P59KUA, , transcript yes, date yes, 
- Kilacola (2) video yes https://www.youtube.com/channel/UCYiEcjVorHS78RgoqKiIFgQ, code yes https://github.com/kilacoda/videos, 
- Reducible (long videos) video yes, https://www.youtube.com/@Reducible/videos, code yes https://github.com/nipunramk/Reducible
- Kutuzova (5), videos yes https://www.youtube.com/@deeplearningthatworks/videos, code yes (ipynb) https://github.com/sgalkina/animations/tree/main/notebooks  
- Visualizing Deep Learning (2), videos yes (playlist) https://www.youtube.com/playlist?list=PLyPKqVSnetmEOp_g_hfabuRAs9ET-shl_, code yes https://github.com/vivek3141/dl-visualization  
- Vivek3141 (22) https://www.youtube.com/@vcubingx/videos, code yes https://github.com/vivek3141/videos (might have overlap with Visualizing Deep Learning series)


