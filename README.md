
# Getting Started with Agentic Orchestration in Camunda 8
This repository provides a concise, practical guide to agentic orchestration with Camunda 8. It includes step-by-step instructions, 
code examples, and recommended practices to help you orchestrate intelligent agents with BPMN and the Camunda 8 stack.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Starting up Camunda 8 Run](#starting-up-camunda-8-run)
- [Create and deploy a BPMN process](#create-and-deploy-a-bpmn-process)
  - [Start a process instance (Modeler Play)](#start-a-process-instance-modeler-play)

## Prerequisites
- OpenJDK 21–23: Required to run Camunda 8 Java components.
- Docker 20.10.21+: Required to run Camunda 8 via Docker Compose.
- [Camunda Desktop Modeler](https://camunda.com/download/modeler/): To model BPMN and DMN diagrams.
- If you are running Linux:
  - Ubuntu 22.04+ (Linux)
- [Camunda 8 Run](https://downloads.camunda.cloud/release/camunda/c8run/8.8/): Download the latest release for your OS/architecture. Extracting the `.tgz` creates a directory with the `camunda-run` scripts.
- LLM access: [LM Studio](https://lmstudio.ai/) with a local LLM of your choice, or access to the OpenAI API or Google Gemini.

## Starting up Camunda 8 Run
Follow these steps to start Camunda 8 Run locally or via Docker using C8Run.

1) Extract and navigate to the C8Run folder
- After downloading the archive, extract it. A folder containing the `camunda-run`/`c8run` scripts will be created.
- Open a terminal and change into that folder (often named like `c8run-<version>`).

2) Start locally (no Docker)
- macOS/Linux (helper script):
  ```
  ./start.sh
  ```
- macOS/Linux (CLI):
  ```
  ./c8run start
  ```
- Windows (PowerShell or CMD):
  ```
  .\c8run.exe start
  ```

When startup completes, Operate should open automatically. If it doesn’t, navigate to:
http://localhost:8080/operate

3) Start with Docker Compose
This uses Docker Compose and is equivalent to `docker compose up -d` for the Camunda 8 stack managed by C8Run.

When started with Docker, access Operate at:
http://localhost:8088/operate

4) Stop services
- Local:
  ```
  ./c8run stop
  ```
- Docker:
  ```
  docker compose down
  ```

## Create and deploy a BPMN process
Follow these steps to model and deploy a minimal process from the Camunda Desktop Modeler.

1) Create a new BPMN diagram
- Open the Camunda Desktop Modeler and choose “New File” → “BPMN Diagram (Camunda 8)”.
- Set the process properties:
  - Name: `AI Fraud Check`
  - Process Id: `ai-fraud-check`

2) Model a minimal flow
- Add a Start Event and an End Event, connected by a sequence flow.
- Optionally rename the events to `Fraud Check Requested` and `Fraud Check Completed` (see screenshot below).

3) Save the diagram
- Save as `ai-fraud-check.bpmn` in your workspace folder.

**Reference diagram and interface of the Camunda Desktop Modeler:**
![AI Fraud Check BPMN](img/img.png)

4) Configure the deployment target
- Click the “Deploy” rocket icon in the bottom-left.
- In the deploy panel, select “Self-Managed”.
- Gateway address (Zeebe): `http://localhost:26500`
  - Works for both local C8Run and C8Run with Docker Compose (port 26500 is exposed).

5) Deploy
- Click “Deploy”. You should see a success message when deployment completes.

6) Verify the deployment
- Open Operate:
  - Local C8Run: http://localhost:8080/operate
  - Docker Compose: http://localhost:8088/operate
- In Operate, open “Processes” and confirm that `AI Fraud Check` is shown as deployed.

### Start a process instance
Use the Camunda Modeler’s Play button to start an instance of `ai-fraud-check` with an agreed test payload.

1) Click the “Play” button (▶) next to the Deploy icon.
2) Ensure Process Id is `ai-fraud-check`. Optionally set a Business Key.
3) Paste the following JSON into the Variables field:

```json
{
  "fullName": "Maria Heinrichs",
  "dob": "2023-06-08",
  "emailAddress": "thomas.heinrichs@miragon.io",
  "totalIncome": 10000,
  "totalExpenses": 640000,
  "largePurchases": ["House"],
  "charitableDonations": "Rotes Kreuz - 60 Euro im Jahr"
}
```

Notes
- For consistency across this guide, only use the variables listed above unless explicitly instructed otherwise.
- Ensure the JSON is valid (double quotes around keys/strings, no trailing commas).

4) Click “Start instance”. The process should start and complete immediately for this minimal flow.

Verify in Operate
- Open Operate and search for the process. If you don’t see the instance, enable “Include Completed”.
- You should see a completed instance of `AI Fraud Check`.

Troubleshooting
- Deployment fails: ensure Camunda 8 Run is started and reachable on `http://localhost:26500`.
- Wrong diagram type: create a “BPMN Diagram (Camunda 8)” and select “Self-Managed” as the target.
- Cannot start instance: verify the Process Id is `ai-fraud-check` and the variables JSON is valid.

**Congratulations! You have successfully created, deployed, and started your first process in Camunda 8.**


