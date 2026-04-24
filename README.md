# S2TT: Speech-to-Text & Translation Continual Learning Platform

Welcome to the main repository for the **S2TT Crowdsourcing Platform**. This project facilitates a complete Human-in-the-Loop (HITL) workflow for automated speech recognition (ASR) and machine translation (MT). 

By collecting high-quality human corrections on ML-generated outputs, the platform feeds a Continual Learning pipeline designed to iteratively fine-tune the underlying models to improve their accuracy and domain adaptation over time.

## Repository Structure

This repository connects the frontend and backend services via Git submodules. 

* **[Frontend Submodule](./frontend):** A React, Vite, and TypeScript web application providing the user interface for crowdsourcing corrections, media uploads, and administrative reviews.
* **[Backend Submodule](./backend):** A FastAPI application powered by Celery workers, handling heavy ML inference, data management, and the MLflow QLoRA fine-tuning pipeline.

## High-Level Architecture & Workflow

The system is designed around a continuous feedback loop:

1. **Inference:** Users upload media via the frontend. The backend queues asynchronous Celery tasks to generate initial transcriptions (via Whisper) and translations (via Llama 3).
2. **Crowdsourcing Correction:** Users review, edit, and correct the machine-generated text using the frontend's synchronized timestamp editor.
3. **Administrative Validation:** Administrators review the pending user-submitted corrections and approve high-quality, verified data.
4. **Continual Learning Pipeline:** Approved corrections are aggregated to trigger an automated training pipeline. The models are fine-tuned via QLoRA, evaluated using backward transfer metrics (WER/BLEU), and logged via MLflow for deployment.

## Getting Started

Because this repository uses submodules, you must ensure you pull the code for both the frontend and backend when cloning.

```bash
# Clone the repository with submodules included
git clone --recursive https://github.com/6ill/speech-to-text-translation-en-id-continual-learning

# If you already cloned the repository normally, initialize the submodules:
git submodule update --init --recursive
```

Setup & Installation
Please refer to them directly to spin up the application:
- Frontend [Setup Instructions](https://github.com/6ill/speech-to-text-translation-frontend)
- Backend [Setup Instructions](https://github.com/6ill/api-speech-text-translation-crowdsourcing-app)

Core Technologies
- Client: React, TypeScript, Tailwind CSS, shadcn-ui, TanStack Query
- API & Asynchronous Workers: FastAPI, Python, Celery, Redis
- Data & Object Storage: PostgreSQL (SQLModel), MinIO/S3
- Machine Learning & MLOps: OpenAI Whisper, Llama 3, MLflow, HuggingFace PEFT (QLoRA)