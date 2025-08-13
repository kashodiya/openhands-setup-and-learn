# OpenHands Guide
This is a guide for setting up and using OpenHands. 

## Important Note
- These instructions are to run OpenHands in AWS Launchpad environment. 
- Since you can not run Docker on AWS Workspace, you need to create EC2 and install docker in it. 

## Resources
- OpenHands GitHub repo: https://github.com/All-Hands-AI/OpenHands
- OpenHands home page: https://docs.all-hands.dev/
- OpenHands YouTube Channel: https://www.youtube.com/@All-Hands-AI

## Setup your environment
- Install docker (ask ChatGPT or LLM on how to)

## Setup using Docker (recommanded option)
- Create a folder of your choice and location
- In that folder create docker-compose.yml file with following content.
```yaml
services:
  openhands-app:
    image: docker.all-hands.dev/all-hands-ai/openhands:0.50
    container_name: openhands-app
    pull_policy: always
    stdin_open: true  # equivalent to -i
    tty: true  # equivalent to -t
    environment:
      - SANDBOX_RUNTIME_CONTAINER_IMAGE=docker.all-hands.dev/all-hands-ai/runtime:0.50-nikolaik
      - LOG_ALL_EVENTS=true
    volumes:
      - ./.openhands:/.openhands
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - "8150:3000"
    extra_hosts:
      - "host.docker.internal:host-gateway"

  litellm:
    image: ghcr.io/berriai/litellm:main-latest
    container_name: litellm
    volumes:
      - ./litellm-config.yml:/app/config.yaml
    restart: unless-stopped
    environment:
      - LITELLM_API_KEY=${LITELLM_API_KEY}
      - AWS_REGION=us-east-1
      - PORT=80
    command: --config /app/config.yaml --detailed_debug
```
- Create litellm-config.yml
```yaml
model_list:
  - model_name: Claude3
    litellm_params:
      model: anthropic.claude-3-haiku-20240307-v1:0
  - model_name: NovaPro1
    litellm_params:
      model: bedrock/amazon.nova-pro-v1:0
  - model_name: Claude3.7
    litellm_params:
      model: us.anthropic.claude-3-7-sonnet-20250219-v1:0
  - model_name: Claude4
    litellm_params:
      model: us.anthropic.claude-sonnet-4-20250514-v1:0
  - model_name: ClaudeOpus4.1
    litellm_params:
      model: us.anthropic.claude-opus-4-1-20250805-v1:0

litellm_settings:
  modify_params: True
  drop_params: true
```
- Optional step: Change the port number from 8150 to whatever you need.
- Run the docker compose using this command (Note to replace any-key-you-like with some random key):
```bash
export LITELLM_API_KEY=any-key-you-like
docker-compose up -d
```
- Access your OpenHands app at: http://localhost:8150

## Set LLM
- In OpenHands web app click gear icon at bottom left
- Ensure that you are on LLM tab
- Enter Custom Model as: litellm_proxy/Claude4
- Enter Base URL as: http://litellm
- Enter API Key as: Whatever you used as any-key-you-like

## How to connect to your personal GitHub?
- Login to GitHub
- Create a PAT (Personal Access Token) (see section belwo)
- In OpenHands web app click gear icon at bottom left
- Click on Integrations tab
- Enter GitHub Token
- Click Save Changes (bottom right)

## Create GitHub Personal Access Token (PAT)
1. Go to **GitHub.com** → Sign in
2. Click your **profile picture** (top-right) → **Settings**
3. Scroll down → Click **Developer settings**
4. Click **Personal access tokens** → **Tokens (classic)**
5. Click **Generate new token** → **Generate new token (classic)**
6. Fill out:
   - **Note**: Describe what it's for (e.g., "CLI access")
   - **Expiration**: Choose duration
   - **Scopes**: Select needed permissions (e.g., `repo` for repository access)
7. Click **Generate token**
8. **Copy the token immediately** (you won't see it again)

