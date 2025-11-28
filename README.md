---
title: LLM Analysis Quiz Solver
emoji: 🏃
colorFrom: red
colorTo: blue
sdk: docker
pinned: false
app_port: 7860
---

# LLM Analysis – Autonomous Quiz Solver Agent

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.121.3+-green.svg)](https://fastapi.tiangolo.com/)

This repository contains my implementation for the **TDS Project – LLM Analysis Quiz Solver**.  
The system is an autonomous agent capable of solving multi-step quiz tasks involving data extraction, transformation, analysis, and submission across dynamically linked quiz pages.

The agent uses a tool-augmented reasoning system powered by LangGraph + LangChain and integrates Google's **Gemini 2.5 Flash** model for decision-making and planning. A FastAPI server exposes the `/solve` endpoint, which IITM uses to trigger the quiz-solving pipeline.

---

## 🔍 Overview

The aim of the project is to build a fully autonomous pipeline that:

- Fetches quiz pages rendered with JavaScript  
- Downloads files (PDF, CSV, images, text)  
- Performs data cleaning and processing  
- Executes Python code dynamically  
- Generates visualizations if required  
- Submits answers to the evaluation server  
- Continues solving subsequent quiz pages until completion  

Once a POST request is received, the pipeline runs end-to-end without any human intervention.

---

## 🏗️ Architecture

The system is organized as a **LangGraph state machine**, which routes execution between:

- **FastAPI server** – Receives tasks & validates using email + secret  
- **LLM Agent** – Plans actions through Gemini 2.5 Flash  
- **Tools** – Perform scraping, downloading, execution, and submission tasks  

### Architecture Diagram

┌────────────────┐
│ FastAPI Server │ ← /solve endpoint
└──────┬─────────┘
▼
┌────────────────┐
│ LLM Agent │ ← Planning, coordination, reasoning
│ (LangGraph) │
└──────┬─────────┘
▼
┌────────┬──────────┬──────────┬──────────────┬───────────┐
│Scraper │Downloader│Code Exec │POST Submitter│Add Deps │
└────────┴──────────┴──────────┴──────────────┴───────────┘

yaml
Copy code

---

## ✨ Features

- ✔ Autonomous multi-step quiz solver  
- ✔ JavaScript rendering using Playwright  
- ✔ Handles PDFs, CSVs, images, JSON, HTML tables  
- ✔ Code execution in isolated environment  
- ✔ Auto-installation of missing Python packages  
- ✔ Smooth multi-URL quiz progression  
- ✔ Fully Dockerized and deployable  
- ✔ Background processing to avoid timeouts  

---

## 📁 Project Structure

LLM-Analysis-TDS-Project-2/
├── agent.py # LangGraph logic
├── main.py # FastAPI server
├── pyproject.toml # Project dependencies
├── Dockerfile # Docker container setup
├── tools/
│ ├── init.py
│ ├── web_scraper.py
│ ├── code_generate_and_run.py
│ ├── download_file.py
│ ├── send_request.py
│ └── add_dependencies.py
└── README.md

yaml
Copy code

---

## 📦 Installation

### Requirements
- Python **3.12+**
- Playwright  
- uv **or** pip  
- Git  

---

### Clone the Repository

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
Option A — Using uv (recommended)
bash
Copy code
pip install uv
uv sync
uv run playwright install chromium
Option B — Using pip
bash
Copy code
python -m venv venv
source venv/bin/activate   # macOS / Linux
venv\Scripts\activate      # Windows

pip install -e .
playwright install chromium
⚙️ Configuration
Create a .env file:

ini
Copy code
EMAIL=24f2008092@ds.study.iitm.ac.in
SECRET=<your_secret_here>
GOOGLE_API_KEY=<your_gemini_api_key>
Ensure the SECRET matches what you submitted in the Google Form.

🚀 Usage
Start the server:

bash
Copy code
uv run main.py
# OR
python main.py
The server runs at:

cpp
Copy code
http://0.0.0.0:7860
Test Endpoint
bash
Copy code
curl -X POST http://localhost:7860/solve \
  -H "Content-Type: application/json" \
  -d '{
        "email": "24f2008092@ds.study.iitm.ac.in",
        "secret": "<your_secret_here>",
        "url": "https://tds-llm-analysis.s-anand.net/demo"
      }'
If secret is correct:

json
Copy code
{ "status": "ok" }
🌐 API Endpoints
POST /solve
Starts autonomous quiz-solving.

GET /healthz
Returns service uptime + status.

🛠 Tools Overview
1. Web Scraper
Uses Playwright to render and extract JavaScript-heavy webpages.

2. File Downloader
Handles PDFs, CSVs, images, ZIPs, text files.

3. Python Code Executor
Executes generated Python scripts for analysis tasks.

4. Submit Tool
POSTs the computed answer to quiz submission endpoint.

5. Add Dependencies
Dynamically installs missing packages using uv.

🐳 Docker Deployment
Build image
bash
Copy code
docker build -t llm-agent .
Run container
bash
Copy code
docker run -p 7860:7860 \
  -e EMAIL="24f2008092@ds.study.iitm.ac.in" \
  -e SECRET="<your_secret_here>" \
  -e GOOGLE_API_KEY="<your_api_key>" \
  llm-agent
📄 License
This project is licensed under the MIT License.

Author: Amogh Sharma
Email: 24f2008092@ds.study.iitm.ac.in
Institution: IIT Madras
Course: Tools in Data Science (TDS)
