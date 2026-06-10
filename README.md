# MCP ( Model context protocol)
Open protocol Allows AI models to connect external tools, data sources and appliations in standardized way.

```
HTTP : Standard way for browsers to talk to webservers.
MCP : Standard way for AI models to talk to tools.
```

### Before MCP:
```
chatgpt -   Github API 
Chatgpt -   JiraAPI
chatgpt -   PostgresAPI

Each integration required custom code.
```
### With MCP:

```
AI model
  |
MCP
  |
Github, Jira, Slack, DB
```
One protocol works with many tools.

### Use case:
- Suppose MCP server exposing to kubernetes.

User:     `Show all pods in namespace production` <br>
AI:       Calls MCP tool. `kubectl get pods` <br>
Returns pods. <br>

```
AI Client:
(Claude, Chatgpt, Cursor)
    | MCP
  MCP Server
    |
Github,
K8s,
PostgreSQL
```

- If we have download already trained model `Llama` then we dont require MCP, because it answers from the trained weights.
- If we want answers from live data from our external tools then MCP will be used.

###  Other  Use cases for Devops:
1. Infrastructure & devops
    User questions: <br>
    - Why My deployment failing ?
    - Which node has GPU memory pressure?
    - Show all failed pods ?
    - Restart deployment frontend.

2. Database Assistant
    - How many orders were placed yesterday?

3. Data Lake Analytics.
    - If your setup : `trino, Iceberg, s3`
    - User question : Show top 10 customers by revenue.

4. Github Assistant
    - Show all open issues assigned to me.

5. Document search
    - Connect: `PDFs, Confluence, sharepoint, wiki`
    - User question : what is our backup policy?

6. Monitoring and observability
    - Connect: `Prometheus, Grafana, Loki`
    - User question : Why is API latency High ?

7. Helpdesk/IT Support

8. Security operations.
    - Show failed logins of today.

## MCP vs. RAG
### RAG :
    - Read information only.

### MCP :
    - Perform actions fetch live data.
```
             User
              |
             LLM
              |
     +--------+--------+
     |                 |
    RAG               MCP
     |                 |
 Documents       Live Systems
                     |
      +--------------+-------------+
      |              |             |
 Kubernetes      Trino       GitHub
 PostgreSQL      AWS         Prometheus
```
MCP servers:
1. Kubernetes MCP
2. Trino MCP
3. Filesystem MCP
4. Github MCP
5. PostgreSQL MCP
6. Prometheus MCP

These MCP servers are different process/applications that expose tools and data to the AI models using MCP protocols.

### open-webui for local environment:

```
sudo docker run -d   --name open-webui   -e HF_TOKEN=hf_YDGBxXkoavqaejOccmECffNBasxcKbelWKXecJ   -p 3000:8080   -v open-webui:/app/backend/data   ghcr.io/open-webui/open-webui:main
```
- You can connect open-webui with the  local deployed LLM server and it works.
