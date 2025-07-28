# Persona-Driven Document Intelligence Analyst

## Project Overview
This project is an intelligent document analysis tool designed to meet the requirements of the Adobe Hackathon Challenge 1B. It acts as a sophisticated analyst that processes a collection of PDF documents and extracts the most relevant sections based on a specific Persona and their Job-to-be-Done.

The core innovation is a hybrid AI workflow that combines:
- The speed and precision of a CrossEncoder model for semantic search
- The nuanced, contextual "thinking" of a small language model (tinyllama) for persona-driven ranking

This ensures the final output is not just a list of relevant snippets, but a curated, intelligent, and actionable analysis that directly addresses the user's needs.

## Getting Started
These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites
Before you begin, ensure you have the following installed:
- Python 3.8+
- Ollama: The project uses Ollama to run the tinyllama model locally. Please download and install it from [ollama.com](https://ollama.com).
- Python Libraries: You will need to install several Python packages.

## Directory Structure
For the `run.py` script to work correctly, your project must follow this specific directory structure. The script will automatically find and process each Collection directory.

```
Your_Project_Folder/
├── app.py # The core analysis engine
├── run.py # The script to execute the analysis
├── Collection 1/ # First test case (e.g., Travel Planning)
│ ├── PDFs/ # Folder containing all PDFs for this collection
│ │ ├── doc1.pdf
│ │ └── doc2.pdf
│ └── challenge1b_input.json # Input configuration for this collection
│
├── Collection 2/ # Second test case (e.g., Academic Research)
│ ├── PDFs/
│ │ ├── paper1.pdf
│ │ └── paper2.pdf
│ └── challenge1b_input.json
│
└── ... (and so on for other collections)  
```

## Setup and Execution
Follow these steps to set up your environment and run the analysis.

### Step 1: Install Dependencies
Navigate to your project directory in the terminal and install the required Python libraries using pip:
```bash
pip install ollama sentence-transformers PyMuPDF
```

### Step 2: Set Up the AI Model
After installing Ollama, you need to pull the tinyllama model. This model is small, fast, and meets the sub-1GB requirement of the challenge.

Open your terminal and run the following command:
```bash
ollama pull tinyllama:1.1b-chat-v0.6-q2_K
```

Make sure the Ollama application is running in the background before you proceed to the next step.

### Step 3: Configure Your Analysis
For each analysis you want to run, you need to set up a Collection folder as described in the "Directory Structure" section above.

1. Create a folder (e.g., Collection 1).
2. Inside it, create a PDFs folder and place all the relevant PDF documents there.
3. Inside the Collection 1 folder, create a `challenge1b_input.json` file. This file defines the persona and task for the analysis.

Example `challenge1b_input.json`:
```json
{
  "challenge_info": {
    "challenge_id": "round_1b_001",
    "test_case_name": "travel_planner_test",
    "description": "South of France Travel Planning"
  },
  "documents": [
    { "filename": "South of France - Cities.pdf" },
    { "filename": "South of France - Cuisine.pdf" },
    { "filename": "South of France - Things to Do.pdf" },
    { "filename": "South of France - Tips and Tricks.pdf" }
  ],
  "persona": {
    "role": "Travel Planner"
  },
  "job_to_be_done": {
    "task": "Plan a trip of 4 days for a group of 10 college friends."
  }
}
```

### Step 4: Run the Analysis
Once your folders are set up and the Ollama application is running, you can execute the analysis.

1. Place the `run.py` and `app.py` files in the main project directory (at the same level as your Collection folders).
2. Open your terminal, navigate to the project directory, and run the following command:
```bash
python run.py
```

The script will automatically discover each Collection directory, read its `challenge1b_input.json`, process the associated PDFs, and save the final analysis to a `challenge1b_output.json` file inside that same collection directory.

## How It Works: The Hybrid AI Workflow
This project's intelligence comes from a carefully designed hybrid workflow that uses different AI models for their specific strengths:

1. **Intelligent Structure Extraction**: The script first analyzes the visual layout of each PDF to identify the actual section titles and the content that belongs to them, just like a human would. This avoids the issue of treating every paragraph as a generic "chunk."

2. **Fast Semantic Search (CrossEncoder)**: With structured sections in hand, the script uses a fast and powerful CrossEncoder model. This model performs a broad semantic search across all sections from all documents to find the top 20 most relevant candidates for the user's task. This step is extremely fast and provides a high-quality pool of potential solutions.

3. **Persona-Driven "Thinking" (tinyllama)**: This is the most critical step. The top 20 candidate sections are then passed to the tinyllama model. Its only job is to think like the specified persona. It performs a sophisticated pairwise comparison to re-rank the sections based on which is most useful for the specific job-to-be-done. This is where the nuanced, human-like intelligence comes from.

4. **Reliable Formatting (Python)**: Finally, the Python code takes the intelligently ranked list from the LLM and builds the final, perfectly structured JSON output, ensuring it always adheres to the required format.

This multi-stage process ensures the final output is not just a list of keywords, but a high-quality, actionable analysis that is intelligently ranked from the user's specific point of view.
