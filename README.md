# 🧠 n8n + Ollama Integration  
A complete automation setup using **n8n**, **Ollama (local AI)**, and **Google Drive / OneDrive** to process files and generate intelligent responses.

## 📦 Project Structure

```
📁 project-root/
├── docker-compose.yml              # Contains n8n + PostgreSQL + Ollama integration
├── Workflow.json                   # Exported n8n workflow
├── Operation.mp4                   # Flow operation
└── README.md                       # Documentation
```

## 🚀 How It Works

1. **n8n** runs inside a Docker container with a PostgreSQL database.  
2. **Ollama** runs locally on your machine (or optionally as a Docker container).  
3. The n8n workflow:
   - Reads files from a **Google Drive** or **OneDrive** folder  
   - Sends the file content to **Ollama**  
   - Receives the AI model response (e.g., LLaMA 3)  
   - Returns or stores the result (e.g., in a spreadsheet, database, or notification)

## ⚙️ Setup

### 1️⃣ Clone the project
```bash
git clone <repository-url>
cd <project-folder>
```

### 2️⃣ Start the containers
```bash
docker compose up -d
```

Containers:
- `n8n`: dashboard available at [http://localhost:5678](http://localhost:5678)
- `postgres`: internal database for n8n

> 🔸 Ollama must be running locally on port **11434**.  
> If it’s running outside Docker (on Windows/macOS/Linux), n8n can reach it using:
> ```
> http://host.docker.internal:11434/api/generate
> ```

## 🧩 Workflow Configuration

1. Access the **n8n dashboard**:  
   👉 [http://localhost:5678](http://localhost:5678)

2. Import the workflow file:
   ```
   My workflow.json
   ```
   > Menu → Import → Import from File

3. Edit the **Google Drive** or **OneDrive** node:
   - Create a new credential (OAuth)
   - Select the folder you want to monitor
   - Set the operation to `List` or `Trigger on File Created`

4. In the **HTTP Request** node, keep:
   ```json
   {
     "model": "llama3",
     "prompt": "{{$json['text']}}"
   }
   ```
   URL:  
   ```
   http://host.docker.internal:11434/api/generate
   ```

## ☁️ Environment Variables (in `docker-compose.yml`)

```yaml
environment:
  - DB_TYPE=postgresdb
  - DB_POSTGRESDB_HOST=postgres
  - DB_POSTGRESDB_PORT=5432
  - DB_POSTGRESDB_DATABASE=n8n
  - DB_POSTGRESDB_USER=n8n
  - DB_POSTGRESDB_PASSWORD=n8n
  - GENERIC_TIMEZONE=America/Sao_Paulo
```

## 🧠 Example Flow (Google Drive → Ollama)

```text
[Google Drive Trigger] → [Download File] → [HTTP Request (Ollama)] → [Return Data]
```

### Flow Steps
1. A new file is added to Google Drive  
2. n8n downloads the file  
3. The content is sent to the LLaMA 3 model through the local API  
4. Ollama returns the analysis or summary  
5. The result is displayed or stored

## 🧰 Useful Commands

### Stop containers
```bash
docker compose down
```

### View n8n logs
```bash
docker logs n8n-n8-1
```

### Test Ollama
```bash
curl http://localhost:11434/api/tags
```

## 🧩 Next Steps

- [ ] Add a **result storage** node (e.g., Notion, PostgreSQL, Google Sheets)  
- [ ] Create a weekly automation trigger (for reports)  
- [ ] Add JWT authentication to the endpoint  
- [ ] Integrate with a local Flask or FastAPI service  

## 🧑‍💻 Author
**Nicholas Birochi**  
Automation and Data Workflows with Local AI 💡  
📍 Brazil | ⚙️ n8n | 🧠 Ollama | ☁️ Docker | 🔗 Data Automation