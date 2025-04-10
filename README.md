# README.md

## 🧠 AI Server (MVP)

Цей проект — мінімальний AI-сервер на базі FastAPI, Ollama, FAISS (RAG) і Whisper.

### 📦 Компоненти:
- **FastAPI** — HTTP API
- **Ollama** — LLM (наприклад, LLaMA3)
- **FAISS** — пошук по векторному індексу
- **Whisper** — трансформація аудіо в текст

---

### 🚀 Розгортання

#### 1. Клонування репозиторію
```bash
git clone https://github.com/yourusername/ai-server.git
cd ai-server
```

#### 2. Запуск за допомогою docker-compose
```bash
docker-compose up --build
```

#### 3. Або окремо (лише FastAPI)
```bash
docker build -t ai-server-api .
docker run -p 8000:8000 --env-file .env ai-server-api
```

---

### 🔗 API endpoints

- `GET /` — перевірка статусу
- `POST /llm` — запит до LLM
  ```json
  { "prompt": "Привіт, як справи?" }
  ```
- `POST /rag` — запит до системи RAG
  ```json
  { "question": "Що таке FAISS?" }
  ```
- `POST /whisper` — завантаження `.wav` файлу для розпізнавання мовлення

---

### ⚙️ Вимоги
- Docker + docker-compose
- Ollama та Whisper повинні бути запущені як сервіси (налаштовуються в docker-compose.yml)
- Змінні середовища у `.env`

---

### ⚙️ Змінні середовища
`.env` файл:
```
OLLAMA_HOST=http://ollama:11434
RAG_ENDPOINT=http://faiss:8001/query
WHISPER_COMMAND=whisper --model base --language en
```

---

### 🔁 CI/CD (GitHub Actions)
Файл `.github/workflows/deploy.yml` запускає CI:
```yaml
name: CI/CD

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Lint and Test
        run: |
          echo "Add your linting and testing here"
```

> Створи файл `.github/workflows/deploy.yml` та встав цей код.

---

### 📦 docker-compose.yml
```yaml
version: '3.9'

services:
  api:
    build: .
    ports:
      - "8000:8000"
    env_file:
      - .env
    depends_on:
      - ollama
      - faiss
      - whisper

  ollama:
    image: ollama/ollama:latest
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama

  faiss:
    image: your-faiss-service:latest
    ports:
      - "8001:8001"

  whisper:
    image: your-whisper-service:latest
    command: ["--model", "base", "--language", "en"]

volumes:
  ollama_data:
```

> Створи файл `docker-compose.yml` та встав цей вміст.

---

👤 Автор: [Roman]

---

### ✅ Плани
- Додати авторизацію
- Підключити Qdrant замість FAISS
- CI/CD (GitHub Actions)
- K8s Helm чарти для продакшн-орієнтованого розгортання
- Інтеграція з корпоративними сервісами через API Gateway
