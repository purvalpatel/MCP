A Kubernetes MCP Server allows an AI assistant (ChatGPT, Claude, Cursor, Copilot, Open WebUI, etc.) to interact with your Kubernetes cluster using natural language instead of manually running kubectl commands. It acts as a bridge between the LLM and the Kubernetes API. <br>

You can ask: <br>
>Show me all failed pods in the cluster.

## Setup Kubernetes MCP Server.

### Start Kubernetes-MCP Server first:
```
npx kubernetes-mcp-server@latest    --port 8080
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
