# 🏥 Medical Chatbot with LLMs, LangChain, Pinecone & Flask

Welcome to the **AI-Powered Medical Chatbot** repository. This project showcases an end-to-end Retrieval-Augmented Generation (RAG) system designed to answer medical queries accurately and concisely. By utilizing a clinical handbook/medical book as our knowledge base, the chatbot yields context-aware answers rather than generic responses.

---

## 🚀 Key Features

*   **RAG Architecture (Retrieval-Augmented Generation):** Enhances LLM capabilities by sourcing relevant medical knowledge before generation.
*   **Vector Database:** Powered by **Pinecone** for extremely fast similarity searches on embedded document chunks.
*   **Hugging Face Embeddings:** Embeds text chunks via `sentence-transformers/all-MiniLM-L6-v2` local model (384 dimensions).
*   **Generative AI Pipeline:** Leverages **langchain-openai** (`gpt-4o`) to formulate responses with high accuracy.
*   **Web Console:** Sleek Flask-based web application with an interactive dynamic messaging interface.
*   **Production Ready CI/CD:** Ready for deployment to AWS (EC2/ECR) using GitHub Actions.

---

## 🛠️ Tech Stack & Libraries

- **Language:** Python 3.10+
- **Agentic / Orchestration Framework:** LangChain (`langchain-core`, `langchain-community`, `langchain-pinecone`, `langchain-openai`)
- **Web App Framework:** Flask
- **Vector Database:** Pinecone DB
- **Embedding Model:** Hugging Face `all-MiniLM-L6-v2` (Sentence Transformers)
- **Deployment & DevOps:** Docker, AWS ECR, AWS EC2, GitHub Actions

---

## 🧭 System Architecture Diagram

```
[Medical Reference (PDF)] ──> [Text Splitting (500 limit, 20 overlap)] ──> [Hugging Face Embeddings] ──> [Pinecone Index Store]
                                                                                                               │
                                                                                                               ▼
[Query Text] ──> [Hugging Face Embeddings] ──> [Cosine Similarity Search] ──> [Top 3 Retrieved Paragraphs]     │ (retriever)
      │                                                                                │                       │
      ▼                                                                                ▼                       ▼
[User Request] ───────────────────────────> [ChatPromptTemplate Prompt] ──────────────────────────────────────────> [gpt-4o Output]
```

---

## 📋 Installation & Local Setup

### Step 1: Clone the Repository
```bash
git clone https://github.com/entbappy/Build-a-Complete-Medical-Chatbot-with-LLMs-LangChain-Pinecone-Flask-AWS.git
cd Build-a-Complete-Medical-Chatbot-with-LLMs-LangChain-Pinecone-Flask-AWS
```

### Step 2: Establish the Development Environment
You can either initialize a standard Python virtual environment or use `conda`.

#### Option A: Standard Python `venv` (Recommended)
```bash
python -m venv venv
# On Windows (PowerShell/CMD)
.\venv\Scripts\Activate.ps1
# On MacOS/Linux
source venv/bin/activate
```

#### Option B: Conda (Alternative)
```bash
conda create -n medibot python=3.10 -y
conda activate medibot
```

### Step 3: Install Required Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Environment Variables Setup
Create a `.env` file in the root directory structure and add your API keys:

```ini
PINECONE_API_KEY = "your-pinecone-api-key-here"
OPENAI_API_KEY = "your-openai-api-key-here"
```

---

## 🏃 Running the Application

### Step 1: Ingest & Store Data in Pinecone
Place your reference medical PDFs in the `data/` directory. Run the indexing script to parse documents, create embeddings, and upload vectors to Pinecone:

```bash
python store_index.py
```

### Step 2: Launch the Flask Server
Run the Flask application on your local development machine:

```bash
python app.py
```

### Step 3: Access your Web Interface
Open your browser and navigate to:
```url
http://localhost:8080
```

---

## ☁️ AWS CD/CD Pipeline & Deployment

Deploying the service to Amazon Web Services using **GitHub Actions**, **Elastic Container Registry (ECR)**, and **Elastic Compute Cloud (EC2)**:

### 1. AWS Services Setup
1. **ECR:** Create a private Elastic Container Registry repository to save your Docker image.
2. **EC2:** Launch an Ubuntu EC2 virtual instance. Open Port `8080` in the Security Groups inbound rules.
3. **IAM User Setup:** Generate an IAM client user with deployment permissions:
   * `AmazonEC2ContainerRegistryFullAccess`
   * `AmazonEC2FullAccess`

### 2. Configure Docker on EC2 Machine
Run the following commands inside your EC2 terminal:
```bash
sudo apt-get update -y
sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

### 3. Register self-hosted runner
Go to your **GitHub Repository Settings** > **Actions** > **Runners** > **New self-hosted runner** and configure the setup process inside the EC2 machine.

### 4. Inject Secrets to GitHub Repo
Setup the following variables inside your GitHub Actions Repository secrets:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_DEFAULT_REGION` (e.g., `us-east-1`)
- `ECR_REPO` (Your AWS ECR repository URI)
- `PINECONE_API_KEY`
- `OPENAI_API_KEY`
