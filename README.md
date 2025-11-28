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

This repository contains my implementation of the **TDS Project – LLM Analysis Quiz Solver** for the BS in Data Science program at IIT Madras.  
It is an **autonomous agent** that solves multi-step quiz tasks involving data extraction, processing, analysis, and visualization.

The agent uses **LangGraph**, **LangChain**, and **Google Gemini 2.5 Flash** to orchestrate tool usage and handle complex reasoning during the quiz.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [Tools & Capabilities](#tools--capabilities)
- [Docker Deployment](#docker-deployment)
- [How It Works](#how-it-works)
- [License](#license)

---

## 🔍 Overview

The goal of this project is to build a **fully autonomous pipeline** that can:

- Fetch quiz pages (including JavaScript-rendered pages)
- Scrape websites & call APIs  
- Download files: PDFs, CSVs, images  
- Clean and preprocess data  
- Perform analysis, statistics, and visualization  
- Execute dynamically generated Python code  
- Submit answers automatically  
- Continue solving multiple quiz pages until completion  

Once the `/solve` endpoint receives a POST request, the entire workflow runs automatically.

---

## 🏗️ Architecture

The system is built using a **LangGraph state machine**, which links all components:

┌─────────────┐
│ FastAPI │ ← Handles POST requests with quiz URLs
│ Server │
└──────┬──────┘
│
▼
┌─────────────┐
│ Agent │ ← LangGraph orchestrator using Gemini 2.5 Flash
│ (LLM) │
└──────┬──────┘
│
├────────────┬────────────┬─────────────┬──────────────┐
▼ ▼ ▼ ▼ ▼
[Scraper] [Downloader] [Code Exec] [POST Req] [Add Deps]

yaml
Copy code

### Key Components
- **FastAPI (`main.py`)** – Validates secrets, receives quiz tasks  
- **LangGraph Agent (`agent.py`)** – Controls reasoning, planning, and state  
- **Tools (`tools/` folder)** – Modular actions like scraping, downloading, executing code  
- **LLM** – Gemini 2.5 Flash with built-in rate limiting

---

## ✨ Features

- ✅ Autonomous multi-step decision making  
- ✅ Playwright-based JS rendering  
- ✅ Python code generation & execution  
- ✅ Downloads + processes all major file formats  
- ✅ On-the-fly dependency installation  
- ✅ Robust retries & error handling  
- ✅ Docker-ready for HuggingFace/Cloud deployment  
- ✅ Respects Gemini API rate limits  

---

## 📁 Project Structure

LLM-Analysis-TDS-Project-2/
├── agent.py
├── main.py
├── pyproject.toml
├── Dockerfile
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

### Prerequisites
- Python **3.12+**
- Playwright
- `uv` (recommended) or pip
- Git

---

### Step 1 — Clone the Repository

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
Step 2 — Install Dependencies
Option A — Using uv (Recommended)
bash
Copy code
pip install uv
uv sync
uv run playwright install chromium
Start server:

bash
Copy code
uv run main.py
Option B — Using pip
bash
Copy code
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac

pip install -e .
playwright install chromium
⚙️ Configuration
Create a .env file:

ini
Copy code
EMAIL=24f2008092@ds.study.iitm.ac.in
SECRET=<your_secret_here>
GOOGLE_API_KEY=<your_gemini_api_key_here>
🚀 Usage
Start the server:

bash
Copy code
uv run main.py
# OR
python main.py
Server runs at:

cpp
Copy code
http://0.0.0.0:7860
Test the Endpoint
bash
Copy code
curl -X POST http://localhost:7860/solve \
  -H "Content-Type: application/json" \
  -d '{
    "email": "24f2008092@ds.study.iitm.ac.in",
    "secret": "<your_secret_here>",
    "url": "https://tds-llm-analysis.s-anand.net/demo"
  }'
Response:

json
Copy code
{ "status": "ok" }
🌐 API Endpoints
POST /solve
Triggers the entire quiz-solving process.

GET /healthz
Basic health-check.

🛠 Tools & Capabilities
1. Web Scraper (get_rendered_html)
Full JS rendering via Playwright

Extracts HTML content after network idle

2. File Downloader (download_file)
Handles PDFs, CSVs, images, zip files, etc.

3. Python Code Executor (run_code)
Safely executes isolated Python code.

4. POST Request Tool (post_request)
Submits answers to quiz endpoints.

5. Dependency Installer (add_dependencies)
Automatically installs missing libraries.

🐳 Docker Deployment
Build
bash
Copy code
docker build -t llm-analysis-agent .
Run
bash
Copy code
docker run -p 7860:7860 \
  -e EMAIL="24f2008092@ds.study.iitm.ac.in" \
  -e SECRET="<your_secret_here>" \
  -e GOOGLE_API_KEY="<your_api_key>" \
  llm-analysis-agent
HuggingFace Spaces Deployment
Create new Space → Select Docker template

Upload repo

Add secrets:

EMAIL

SECRET

GOOGLE_API_KEY

Space builds automatically

🧠 How It Works
1. Request Reception
Validate secret

Start background task

2. Agent Initialization
State machine loads tools & LLM.

3. Task Loop
pgsql
Copy code
Analyze → Choose Tool → Execute Tool → Evaluate → Repeat
4. Completion
Agent stops when no next quiz URL is found.

📄 License
This project is licensed under the MIT License.

Author
Amogh Sharma
BS in Data Science (IIT Madras)
Email: 24f2008092@ds.study.iitm.ac.in
