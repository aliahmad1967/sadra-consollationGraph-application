<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://github.com/user-attachments/assets/0aa67016-6eaf-458a-adb2-6e31a0763ed6" />
</div>

# Run and deploy your AI Studio app

This contains everything you need to run your app locally.

View your app in AI Studio: https://ai.studio/apps/drive/1iyVN-2KYYZ5yNzulBWCSs04et1ZDu9tR

## Run Locally

**Prerequisites:**  Node.js


1. Install dependencies:
   `npm install`
2. Set the `GEMINI_API_KEY` in [.env.local](.env.local) to your Gemini API key
3. Run the app:
   `npm run dev`


# Below is a ** user guide**, written for **end users and developers**, explaining **step by step** how to run the project **locally with Docker and a free local LLM model**.


---

# Sadra Consolation Graph Application

### Local Deployment Guide (Docker + Free LLM)

This guide explains how to run the **Sadra Consolation Graph Application** locally using **Docker** and a **free, fully local Large Language Model (LLM)** without any paid APIs.

The setup uses:

* **Docker** for containerization
* **Ollama** for running free local LLMs (e.g. Llama 3, Mistral)

---

## 1. Overview

**What this setup provides**

* 100% local execution
* No OpenAI or paid API keys
* Offline-capable LLM inference
* Reproducible Docker-based environment

**Who this is for**

* Developers
* Researchers
* Students
* Anyone wanting a free local LLM setup

---

## 2. System Requirements

### Minimum

* OS: Linux / macOS / Windows (WSL2 recommended on Windows)
* RAM: 8 GB (16 GB recommended)
* Disk space: ~10 GB free

### Required software

* Docker
* Git
* Ollama

---

## 3. Clone the Repository

```bash
git clone https://github.com/aliahmad1967/sadra-consollationGraph-application.git
cd sadra-consollationGraph-application
```

---

## 4. Install and Run Ollama (Free Local Model)

### Install Ollama

Download from:

```
https://ollama.com
```

Verify installation:

```bash
ollama --version
```

### Download a free model

Recommended default:

```bash
ollama pull llama3
```

Other lightweight alternatives:

```bash
ollama pull mistral
ollama pull phi3
```

### Test the model

```bash
ollama run llama3
```

If the model responds, Ollama is working correctly.

---

## 5. Configure the Application to Use Ollama

This application **must not use OpenAI or paid APIs**.

### Ollama API endpoint

Ollama runs locally on:

```
http://localhost:11434
```

From inside Docker containers, use:

```
http://host.docker.internal:11434
```

### Example Python function for Ollama

```python
import requests

def ollama_chat(messages):
    response = requests.post(
        "http://host.docker.internal:11434/api/chat",
        json={
            "model": "llama3",
            "messages": messages,
            "stream": False
        }
    )
    return response.json()["message"]["content"]
```

Replace any OpenAI-related code with this logic.

---

## 6. Dockerfile

If the repository does not already include one, create a file named **`Dockerfile`** in the project root:

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

> Adjust `app.py` if your main file has a different name.

---

## 7. Docker Compose Configuration (Recommended)

Create **`docker-compose.yml`**:

```yaml
version: "3.9"

services:
  app:
    build: .
    ports:
      - "8000:8000"
    environment:
      - OLLAMA_BASE_URL=http://host.docker.internal:11434
```

This allows the Docker container to communicate with Ollama running on the host machine.

---

## 8. Build and Run the Application

```bash
docker compose build
docker compose up
```

When the build completes, the application will be available at:

```
http://localhost:8000
```

---

## 9. Verify Everything Is Working

Confirm the following:

* Docker container starts without errors
* Ollama is running
* No OpenAI or API key errors appear
* LLM responses are generated locally

---

## 10. Common Issues and Fixes

### Docker cannot reach Ollama

**Solution**

* Ensure Ollama is running
* Use `host.docker.internal` (not `localhost`) inside Docker

---

### Model is slow

**Solution**

* Use a smaller model:

```bash
ollama pull phi3
```

* Update the model name in the code

---

### Out-of-memory errors

**Solution**

* Close other applications
* Use `mistral` or `phi3`
* Increase system RAM if possible

---

## 11. Optional Improvements

* Add `.env` file for model selection
* Enable GPU support (`--gpus all`)
* Cache LLM responses
* Tune prompts for Arabic or domain-specific usage

---

## 12. Summary (Quick Start)

```bash
# 1. Install Docker & Ollama
# 2. Clone repo
git clone https://github.com/aliahmad1967/sadra-consollationGraph-application.git
cd sadra-consollationGraph-application

# 3. Pull free model
ollama pull llama3

# 4. Run app
docker compose up
```

---

## 13. License

See the repository license file for details.

---

