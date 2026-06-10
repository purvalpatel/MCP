### open-webui for local environment:

```
sudo docker run -d   --name open-webui   -e HF_TOKEN=hf_YDGBxXkoavqaejOccmECffNBasxcKbelWKXecJ   -p 3000:8080   -v open-webui:/app/backend/data   ghcr.io/open-webui/open-webui:main
```
- You can connect open-webui with the  local deployed LLM server and it works.


### Start Gitlab MCP server and open-webui:

1. Create Gitlab Token first:
Settings -> Access Token -> Create Token <br>
Scopes : api, read_repository, read_user, read_api

2. docker-compose.yaml
```
services:

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

3. Store credentials into .env
```
GITLAB_PERSONAL_ACCESS_TOKEN=glpat-k_hqQzYxAv_4F1xKgsdxWe
HF_TOKEN=hf_YDBxXkhoaqgaejOcsdcmECffNBasxxcKblWKvXecJ
```

4. Start container:
```
docker-compose up -d
```

5. Open in browser:
Gitlab MCP server : http://localhost:8000/docs <br>
Open-WebUI : http://localhost:3000 <br>

6. Configuration in open-webui.
- Profile 
- Admin Panel
- Settings
- Integrations  -> Manage tool servers
- Add Connections
        - Name : `Gitlab MCP Server`
        - URL : `http://10.xx.xx.xx:8000`
        - Auth: `None`

- Save.

- Navigate to `Workspace`
- New Model
        - Model Name : Gitlab-MCP
        - Select Tools : Gitlab MCP

- Save & Create.

Now Select Gitlab-MCP from the top in New chat and ask question. it will work.

> Note: `@modelcontextprotocol/server-gitlab` is deprecated and replaced by `@zereight/mcp-gitlab`.

### How to use ?
- What tools are available from the GitLab MCP server?
- Find GitLab user with username "purval"
- List all the projects available in devops group.
