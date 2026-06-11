A Kubernetes MCP Server allows an AI assistant (ChatGPT, Claude, Cursor, Copilot, Open WebUI, etc.) to interact with your Kubernetes cluster using natural language instead of manually running kubectl commands. It acts as a bridge between the LLM and the Kubernetes API. <br>

You can ask: <br>
>Show me all failed pods in the cluster.

## Setup Kubernetes MCP Server.

### Start Kubernetes-MCP Server first:
```
npx kubernetes-mcp-server@latest    --port 8080
```
OR
docker-compose.yaml for `Kubernetes-MCP server` and `open-webui`
```
services:
## This is Gitlab MCP
  mcpo:
    image: python:3.11-slim
    container_name: mcpo-gitlab

    environment:
      GITLAB_PERSONAL_ACCESS_TOKEN: ${GITLAB_PERSONAL_ACCESS_TOKEN}
      GITLAB_API_URL: https://git.xxxx.io/api/v4

    command: >
      bash -c "
      pip install mcpo &&
      apt-get update &&
      apt-get install -y nodejs npm &&
      mcpo --host 0.0.0.0 --port 8000 --
      npx -y @zereight/mcp-gitlab
      "

    ports:
      - "8000:8000"

## This is kubernetes MCP
  kubernetes-mcp:
    image: node:22-alpine
    container_name: kubernetes-mcp
    restart: unless-stopped

    command: >
      sh -c "
      npm install -g kubernetes-mcp-server &&
      kubernetes-mcp-server --port 8080
      "

    ports:
      - "8080:8080"

    environment:
      KUBECONFIG: /root/.kube/config

    volumes:
      - ~/.kube/config:/root/.kube/config:ro

  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui

    ports:
      - "3000:8080"

    environment:
      HF_TOKEN: ${HF_TOKEN}

    volumes:
      - open-webui:/app/backend/data

    depends_on:
      - mcpo

volumes:
  open-webui:

```
Start containers:
```
sudo docker-compose up -d
```

### Login Open-WebUI and configure kubernetes-MCP server.
1. Settings -> Admin Panel -> Settings
2. Integrations -> Manage tool servers
3. Add 
<img width="597" height="585" alt="image" src="https://github.com/user-attachments/assets/a743faf5-17a1-44c9-af0f-77de6b3da787" />


4. Go to Workplace -> New Model
    - Model name
    - Select Base Model
    - Select Tool - `kubernetes MCP`
    - Save and Update

5. From Home page select the `kubernetes MCP` model.

### Test With prompt now.
**<img width="1276" height="460" alt="image" src="https://github.com/user-attachments/assets/b1763e7d-53db-44d3-9652-fd85e4d93613" />**
