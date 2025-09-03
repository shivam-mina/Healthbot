# AI Agent MVP — Health Check Orchestrator

This repo contains a minimal **AI-driven agent** that parses natural language health check commands, maps them to Kubernetes clusters from inventory, and runs predefined runbooks (`kubectl`-based).  

It’s designed as a **lightweight MVP** to demonstrate AI + automation orchestration with minimal infra.

---

## 🚀 Features
- Parse natural language commands like:  
check health of pcrf cluster 1
- Map clusters from `nodes.json` inventory.  
- Execute runbooks with `kubectl` to check:  
- Pod health  
- Deployment status  
- Node readiness  
- Return **structured JSON + summary** via REST API.  

---

## 📂 Repo Structure
.
├── api.py # FastAPI app exposing /command endpoint
├── orchestrator.py # Maps text → runbook → cluster
├── runbooks.py # Reusable health check logic
├── nodes.json # Cluster inventory
├── secrets/ # kubeconfigs (not in git)
├── Dockerfile # Container build
└── README.md # You are here

---

## 🔧 Requirements
- Python 3.10+
- `kubectl` installed and in PATH
- Access to target clusters (`secrets/*.kubeconfig`)
- FastAPI + Uvicorn

Install dependencies:
```bash
pip install fastapi uvicorn pyyaml
▶️ Run Locally
Start the API:
uvicorn api:app --reload
Test with curl:
curl -s -X POST http://localhost:8000/command \
     -H "Content-Type: application/json" \
     -d '{"text":"check health of pcrf cluster 1"}' | jq
Expected response:
{
  "summary": "All pods healthy on cluster pcrf-1",
  "checks": {
    "pods": "ok",
    "nodes": "ok"
  }
}
🐳 Run in Docker
Build and run:
docker build -t ai-agent-mvp .
docker run -p 8000:8000 \
  -v $(pwd)/secrets:/app/secrets:ro \
  ai-agent-mvp
📊 Roadmap
 Add more health check runbooks
 Integrate lightweight LLM parsing (local or API)
 Add correlation IDs + structured logs
 Extend inventory (nodes.json) with multiple clusters
 CI pipeline to lint + test
⚠️ Security Notes
Never commit kubeconfigs or secrets to Git.
Mount secrets/ into container as read-only.
Restrict API access in production (auth, RBAC).
📜 License
MIT

---

Do you want me to also include the **architecture diagram PNG** we created into this `READ
