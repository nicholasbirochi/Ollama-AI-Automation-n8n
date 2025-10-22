# 🧠 n8n + Ollama Integration

This repository connects an n8n workflow to Google Drive and an Ollama model. It includes the exported workflow and a Docker Compose configuration for n8n, PostgreSQL and Ollama; credentials and service endpoints need to be configured for your own environment.

## 📦 Project Structure

```
📁 project-root/
├── docker-compose.yaml              # Contains n8n + PostgreSQL + Ollama integration
├── Workflow.json                   # Exported n8n workflow
└── readme.md                       # Documentation
```

## 🚀 How It Works

1. **n8n** runs inside a Docker container with a PostgreSQL database.  
2. **Ollama** is included as a service in the Compose file. A host installation can be used instead if you adjust the workflow endpoint.  
3. The n8n workflow:
   - Reads files from a **Google Drive** folder  
   - Sends the file content to **Ollama**  
   - Receives the AI model response (e.g., LLaMA 3)  
   - Returns or stores the result (e.g., in a spreadsheet, database, or notification)

## ⚙️ Setup

### 1️⃣ Clone the project
```bash
git clone https://github.com/nicholasbirochi/Ollama-AI-Automation-n8n.git
cd Ollama-AI-Automation-n8n
```

### 2️⃣ Start the containers
```bash
docker compose -f docker-compose.yaml up -d
```

Containers:
- `n8n`: dashboard available at [http://localhost:5678](http://localhost:5678)
- `postgres`: internal database for n8n

> 🔸 The Compose file includes Ollama on port **11434**; review the workflow URL before executing it.  
> If it’s running outside Docker (on Windows/macOS/Linux), n8n can reach it using:
> ```
> http://host.docker.internal:11434/api/generate
> ```

## 🧩 Workflow Configuration

1. Access the **n8n dashboard**:  
   👉 [http://localhost:5678](http://localhost:5678)

2. Import the workflow file:
   ```
   Workflow.json
   ```
   > Menu → Import → Import from File

3. Edit the **Google Drive** node:
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

## ☁️ Environment Variables (in `docker-compose.yaml`)

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

## License

Original code and documentation are covered by [MIT](license).
