# Company Summaries Project Specification

## Project Overview

This project demonstrates how to combine financial APIs with AI tools to create a data processing pipeline. Students will learn how to:
1. Fetch real-world data from financial APIs (yfinance)
2. Use the local Copilot CLI to generate AI-powered summaries
3. Specify which AI model to use when invoking the CLI
4. Complete a workflow without requiring direct access to LLM APIs

## Project Goal

Create a Python program that automatically generates AI-powered summaries of the Magnificent 7 technology companies. For each company, the program will gather company business information and then produce a concise 100-word markdown file containing a professional summary.

## Data Collection

The program should collect information for the following 7 technology companies (the Magnificent 7):
- Apple (AAPL)
- Microsoft (MSFT)
- Nvidia (NVDA)
- Google/Alphabet (GOOG)
- Amazon (AMZN)
- Meta (META)
- Tesla (TSLA)

For each company, gather:
- **Business Summary**: A detailed description of the company's business and operations

This focused approach emphasizes quality over quantity and provides cleaner, more reliable data.

## Processing Workflow

For each company:
1. Download the company's business summary from yfinance
2. Send this text to the Copilot CLI with a request to generate a concise 100-word summary
3. Specify gpt-4.1 as the AI model for generating summaries
4. Save the AI-generated summary as a markdown file in the `summaries/` folder named after the company ticker (e.g., `summaries/AAPL.md`)

## Output Format

Each output file should be saved in a `summaries/` folder and contain:
- The company ticker as a heading
- A 100-word AI-generated summary of the company's business

Example filenames: `summaries/AAPL.md`, `summaries/MSFT.md`, etc.

## Key Requirements

- **Automation**: The entire process should run with a single command
- **Consistency**: All 7 Magnificent 7 companies should be processed automatically
- **Model Selection**: Use gpt-4.1 model for all summarization
- **Simplicity**: The code should be readable and well-commented for student learning

## Tools & Dependencies

- **yfinance**: For fetching company business summaries
- **Anaconda Environment**: Use the FIE463 environment which already has yfinance installed. Call Python scripts only from this environment to ensure all dependencies are available.
- **Copilot CLI**: For generating AI-powered summaries (must be installed and accessible). 
    - Call the `copilot` CLI help to figure out how to specify the model and pass the prompt.
- **Python Standard Library**: For file operations and subprocess management

## Implementation

- Create a Python script named `get_summaries.py` that implements the above workflow in the same folder as this specification file.
- Make sure to log progress, e.g., for which company is data being fetched from Yahoo Finance, for which company the summary is being generated, and when files are saved.

## Success Criteria

- Program successfully generates summaries for all 7 Magnificent 7 companies
- Each summary is approximately 100 words
- Output files are clean, readable markdown
- gpt-4.1 model is used for all summarizations
- Code includes comments explaining key steps
- Program handles potential errors gracefully

