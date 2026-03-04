# Company Summaries Demo

This project demonstrates how to combine financial APIs, data processing, and AI tools to automatically generate professional summaries of major technology companies.

## Overview

The `get_summaries.py` script:
1. Fetches business summaries for the 7 Magnificent 7 tech companies using yfinance
2. Uses the Copilot CLI with gpt-4.1 model to generate AI-powered 100-word summaries
3. Saves each summary as a markdown file (e.g., `AAPL.md`) in the `summaries/` folder

## Companies Covered

**Magnificent 7:**
- Apple (AAPL)
- Microsoft (MSFT)
- Nvidia (NVDA)
- Google/Alphabet (GOOG)
- Amazon (AMZN)
- Meta (META)
- Tesla (TSLA)

## Requirements

- Anaconda environment *FIE463*
- Copilot CLI: Local AI assistant
  - Must be installed and accessible in your PATH


## Usage

Run the script using the FIE463 conda environment:

```bash
cd company-summaries
conda run -n FIE463 python get_summaries.py
```

Or, if already in the FIE463 environment:

```bash
cd company-summaries
python get_summaries.py
```

## Output

The script generates markdown files in a `summaries/` subdirectory:

```
company-summaries/
├── summaries/
│   ├── AAPL.md
│   ├── MSFT.md
│   ├── NVDA.md
│   ├── GOOG.md
│   ├── AMZN.md
│   ├── META.md
│   └── TSLA.md
├── get_summaries.py
├── specification.md
└── README.md
```

Each file in `summaries/` contains:
- The company ticker as a heading
- A 100-word AI-generated summary of the company's business

### Example Output

```markdown
# AAPL

Apple Inc. is a multinational technology company headquartered in Cupertino that designs, manufactures, and markets smartphones, personal computers, tablets, wearables, and software products. It pioneered the smartphone industry and leads in consumer electronics. The company generates revenue from multiple product categories including services, which is increasingly important. Apple distributes products through retail, online, cellular carriers, and resellers, serving consumers and enterprise markets.
```

## Learning Outcomes

This project teaches students:

1. **API Integration**: How to fetch real-world data using yfinance
2. **Data Processing**: Combining multiple data sources into coherent text
3. **Tool Integration**: Using subprocess to invoke external tools (Copilot CLI)
4. **LLM Prompting**: Crafting effective prompts for AI-powered text generation
5. **Specification-Driven Development**: How to write clear specs for LLM code generation ("vibe coding")

## How It Works

### Step 1: Fetch Data
The script uses yfinance to retrieve:
- Company business summary from the ticker's info dictionary
- Recent news articles and headlines

### Step 2: Combine Information
All data is merged into a structured text document with clear sections.

### Step 3: Generate Summary
The combined text is passed to Copilot CLI with a specific instruction:
> "Summarize the following company information in 100 words: [text]"

### Step 4: Save Results
The AI-generated summary is saved to a markdown file named after the ticker.

## Project Files

- `specification.md` - High-level project specification (input for LLM code generation)
- `get_summaries.py` - Main Python implementation
- `README.md` - This file
- `summaries/*.md` - Generated summary files

## Notes

- The script assumes Copilot CLI is installed locally and available in PATH
- yfinance API responses may vary; the script handles missing data gracefully
- Summaries are generated in real-time; processing all 7 companies may take several minutes
