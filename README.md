# Docker + Ollama + Qwen3:8B + LibreChat + Playwright MCP

This project demonstrates how to run a fully local AI-powered browser automation assistant using **LibreChat**, **Ollama**, **Qwen3:8B**, and **Playwright MCP**.

The AI agent can understand natural language instructions, launch a browser, perform actions using Playwright, and summarize the results.

---

# Architecture

```
User
   │
   ▼
LibreChat
   │
   ▼
Qwen3:8B (Ollama)
   │
   ▼
Playwright MCP Server
   │
   ▼
Chrome Browser
```

---

# Prerequisites

## Hardware

* 16 GB RAM or higher
* SSD with at least 20 GB free space
* GPU recommended (optional)

## Software

* Docker Desktop
* Git
* Node.js 20+
* Ollama

---

# Technology Stack

* Docker
* LibreChat
* Ollama
* Qwen3:8B
* Playwright MCP
* Chrome Browser

---

# Step 1 – Install Ollama

Install Ollama from:

https://ollama.com

or using brew
`brew install ollama`

Verify installation:

```bash
ollama --version
```

---

# Step 2 – Download the Model

Pull Qwen3:8B

```bash
ollama pull qwen3:8b
```

Verify:

```bash
ollama list
```

---

# Step 3 – Start Ollama

```bash
ollama serve
```

Default endpoint:

```
http://localhost:11434
```

---

# Step 4 – Clone LibreChat

```bash
git clone https://github.com/danny-avila/LibreChat.git

cd LibreChat
```

---

# Step 5 – Configure LibreChat

Create or edit:

```
librechat.yaml
```

Example MCP configuration:

```yaml
mcpServers:
  playwright:
    type: sse
    url: http://host.docker.internal:8931/sse
    timeout: 120000

mcpSettings:
    allowedDomains:
      - 'host.docker.internal:8931'
      - 'localhost:8931'
```

Example Ollama configuration:
```yaml
    - name: "Ollama"
      apiKey: "ollama"

      baseURL: "http://host.docker.internal:11434/v1"

      models:
        default:
          - "qwen3:8b"

        fetch: true

      titleConvo: true
      titleModel: "current_model"

      summarize: false
      summaryModel: "current_model"

      modelDisplayLabel: "Ollama"
```

To verify the connection for Ollama configuration, run the following in the terminal:

`docker exec -it LibreChat curl http://host.docker.internal:11434/api/tags`

Check out the updated project forked in my account `https://github.com/mfaisalkhatri/LibreChat`

---

# Step 6 – Configure Ollama in LibreChat

Example endpoint

```
http://host.docker.internal:11434
```

Select

* Provider: Ollama
* Model: qwen3:8b

---

# Step 7 – Start Playwright MCP

Open another terminal.

```bash
npx @playwright/mcp@latest \                
  --host 0.0.0.0 \
  --allowed-hosts "*" \
  --port 8931 \

```

Server starts at

```
http://localhost:8931
```

---

# Step 8 – Install Chrome

```bash
npx playwright install chrome
```

---

# Step 9 – Start LibreChat

```bash
docker compose up -d
```

Open

```
http://localhost:3080
```

Create your account.

---

# Create an AI Agent using LibreChat

- Add Qwen3:8b model to the Agent

# Recommended Agent Settings

| Setting           | Recommended Value |
| ----------------- | ----------------- |
| Temperature       | 0.1               |
| Top P             | 0.9               |
| Frequency Penalty | 0                 |
| Presence Penalty  | 0                 |
| Reasoning Effort  | Medium            |
| Reasoning Summary | Auto              |

- Add Playwright MCP server to the Agent
- Add the instructions from the `AI-Instructions.txt`
---

# Example Prompts

Open https://playwright.dev and click **Get Started**.

---

Navigate to GitHub and search for Playwright.

---

Open Google and search for "Playwright MCP".

---

Navigate to https://parabank.parasoft.com/parabank/index.htm
Locate "Username" field using "name=username"
Enter "john" into the "Username" field.
Locate "Password" field using "name=password"
Enter "demo" into the "Password" field.
Locator "Log In" button using "input[type="submit"]
Click on the "Log In" button
Verify that the "Accounts Overview" page is displayed
---

# Execution Flow

```
User Prompt
      │
      ▼
LibreChat
      │
      ▼
Qwen3:8B
      │
      ▼
Playwright MCP
      │
      ▼
Chrome
      │
      ▼
Browser Actions
      │
      ▼
Execution Results
      │
      ▼
AI Summary
```

---

# Troubleshooting

## MCP connection closed

Verify the MCP server is running.

```bash
npx @playwright/mcp@latest \                
  --host 0.0.0.0 \
  --allowed-hosts "*" \
  --port 8931 \
```

---

## Chrome not found

Install Chrome

```bash
npx playwright install chrome
```

---

## Ollama not detected

Ensure Ollama is running.

```bash
ollama serve
```

Verify:

```
http://localhost:11434
```

---

## LibreChat cannot reach MCP

If LibreChat runs inside Docker, use:

```
host.docker.internal
```

instead of

```
localhost
```

---

## Model is hallucinating

Reduce temperature to:

```
0.2
```

Use stricter instructions for the AI Agent.

---

# Useful Commands

Start Ollama

```bash
ollama serve
```

List models

```bash
ollama list
```

Download model

```bash
ollama pull qwen3:8b
```

Run model

```bash
ollama run qwen3:8b
```

Start Playwright MCP

```bash
npx @playwright/mcp@latest \                
  --host 0.0.0.0 \
  --allowed-hosts "*" \
  --port 8931 \
```

Install Chrome

```bash
npx playwright install chrome
```

Start LibreChat

```bash
docker compose up -d
```

Check LibreChat logs in docker

```bash
docker logs Librechat -f
```

Stop LibreChat

```bash
docker compose down
```

