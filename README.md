# Claude Agent SDK Container

**Deploy Claude Agent SDK to your favorite cloud provider using standard Anthropic API keys!**

This repository containerizes the Claude Agent SDK, allowing you to run it on AWS, Google Cloud, Azure, or any cloud platform that supports Docker. Once deployed, you can interact with Claude through a web-based CLI or a REST API from any application or service!

**🤖 Multi-Agent Feature:** The `/query` endpoint includes a built-in example of multi-agent collaboration where a Canadian 🍁 and Australian 🇦🇺 agent discuss user requests and provide their unique perspectives. This demonstrates how to use the Claude Agent SDK's Task tool for subagent delegation. [See examples below](#examples) or customize the agents in `server.ts`.

Since you're here, we expect you already have Claude Code installed and are loving it as much as we are. But if you haven't installed it yet, you can get started here: [Claude Code Installation Guide](https://docs.claude.com/en/docs/claude-code/overview)

> 🔒 **Security Note**: This container exposes Claude AI with tools through an HTTP API. Like any AI integration, be mindful of prompt injection when handling untrusted input. Learn more about this important topic from [Simon Willison's articles on prompt injection](https://simonwillison.net/tags/prompt-injection/).

## ✨ Features

### 🌐 **Dual Access Modes**

- **Web CLI Interface**: Interactive browser-based terminal with real-time streaming
- **REST API**: Programmatic access for applications and integrations

### 🔐 **Security & Access Control**

- **GitHub OAuth Authentication**: Simple one-click GitHub login for web CLI
- **API Key Protection**: Secure REST API access with configurable API keys
- **GitHub Allowlisting**: Control web CLI access by GitHub usernames or organizations
- **JWT Session Management**: Secure cookie-based sessions with expiration

### 🔧 **GitHub Integration**

- **OAuth2 Flow**: Standard GitHub OAuth for seamless authentication
- **Organization Support**: Restrict access by GitHub organization membership
- **User Allowlists**: Fine-grained control with specific GitHub usernames

### 🚀 **Production Ready**

- **Docker-First**: Optimized multi-stage build for cloud deployment
- **Health Monitoring**: Built-in health checks and status endpoints
- **Graceful Shutdown**: Proper signal handling for container orchestration
- **Comprehensive Logging**: Detailed startup and access control logging

### 🔒 **Security First**

- **Automated Security Audit**: First-run scan for malicious code and vulnerabilities
- **Supply Chain Protection**: Detects compromised npm packages and Dockerfile attacks
- **Secret Detection**: Prevents hardcoded credentials in containers
- **Best Practices**: Follows security patterns (non-root user, minimal attack surface)

### 🛠️ **Developer Experience**

- **Real-time Streaming**: Character-by-character CLI response streaming
- **Multiple Models**: Support for Claude Sonnet 4.5 and Opus 4.1
- **Multi-Agent System**: Built-in example with Canadian 🍁 and Australian 🇦🇺 agents
- **Backward Compatible**: Preserves existing REST API functionality
- **Auto-testing**: Comprehensive test script validates full functionality

## 🚀 Quick Setup (4 Steps!)

### Step 1: Clone and Open with Claude Code

```bash
git clone https://github.com/receipting/claude-agent-sdk-container
cd claude-agent-sdk-container
claude
```

**🔒 First-Time Security Check:**

When you first open this repository, Claude Code will prompt you to run a security audit:

```
🔒 SECURITY AUDIT RECOMMENDED

You've just opened a repository from GitHub.

⚠️ IMPORTANT: Before running 'npm install' or 'docker build', it's
   recommended to check this repository for security issues.

💡 To perform the security audit, ask Claude Code:
   "Please perform the security audit for this repository"
```

**Tell Claude**: `Please perform the security audit for this repository`

Claude will spawn an AI-powered agent that intelligently analyzes:

- ✅ `package.json` for malicious install scripts and obfuscated code
- ✅ `Dockerfile` for security antipatterns (curl | bash, hardcoded secrets)
- ✅ Source code for hardcoded secrets, backdoors, and suspicious patterns
- ✅ `.claude/` configuration for malicious hooks

**Why AI-powered?** Unlike regex patterns, Claude understands context and intent, catches novel attack patterns, and explains WHY something is suspicious.

Once the security audit completes, you're safe to proceed with setup!

### Step 2: Run Automated Setup

Claude will tell you to run the setup script in a **separate terminal window**:

```bash
./setup-api-key.sh
```

**The script automatically handles:**

1. ✅ Enter your Anthropic API key (from https://console.anthropic.com/settings/keys)
2. ✅ Creating a unique GitHub App with one click (opens browser to GitHub)
   - Auto-generates unique name like `claude-agent-sdk-202510052056`
   - Or reuses existing credentials if found in `.env`
3. ✅ Configuring access control (your GitHub username auto-added!)
4. ✅ Generating a secure random API key
5. ✅ Writing all credentials to `.env` file

**What you'll do:**

- Paste your Anthropic API key from the console
- Click "Create GitHub App" in browser (literally one click!)
- Press Enter to accept your username in allowlist (or add more users)
- That's it - all credentials automatically saved!

**Traditional 15-step GitHub App setup reduced to ONE CLICK!**

### Step 3: Return to Claude Code

After `./setup-api-key.sh` completes, go back to Claude Code and tell it:

```
Please run ./test.sh
```

Claude will:

- ✅ Build the Docker container
- ✅ Run the container with your `.env` credentials
- ✅ Test all endpoints
- ✅ Confirm everything works

### Step 4: Open the Web CLI

Once Claude confirms the application is running, open your browser to:

**http://localhost:8080**

- Sign in with your GitHub account
- Use the real-time streaming CLI interface
- Ask Claude anything!

**That's it!** You're now running your own private Claude instance with OAuth security.

---

**🛠️ Smart Setup Detection:**

This repository includes an intelligent Claude Code hook that guides you through setup:

**Setup Status Hook** (`UserPromptSubmit`):

- Reminds you to run the security audit (optional but recommended)
- Checks if `.env` is configured
- Verifies Docker image is built
- Confirms container is running
- Shows clear status and next steps

Claude automatically sees your setup state and guides you - no manual checks needed!

### Manual Setup

<details>
<summary>Click here if you prefer to set things up manually (or if automated setup fails)</summary>

### Manual Step 1: Get Your Anthropic API Key

Visit https://console.anthropic.com/settings/keys and create a new API key.

COPY IT NOW - you won't be able to see it again!

### Manual Step 2: Create GitHub App

Go to [GitHub Settings > Developer settings > GitHub Apps](https://github.com/settings/apps/new) and create a new GitHub App:

> 💡 **Note**: Use a unique timestamped name like `claude-agent-sdk-202510052056` to avoid conflicts. If you want to reuse the same app across multiple machines/deployments, you can reuse the same GitHub App credentials by using the same Client ID and Client Secret in your `.env` file.

- **GitHub App name**: `claude-agent-sdk-YYYYMMDDHHMM` (use a unique timestamped name)
- **Homepage URL**: `http://localhost:8080`
- **Callback URL**: `http://localhost:8080/auth/github`
- **Request user authorization (OAuth) during installation**: ✅ **Check this box**
- **Webhook**: Uncheck "Active" (we don't need webhooks)
- **Permissions**:
  - Account permissions > Email addresses: **Read**
  - Account permissions > Profile: **Read**

After creating the app, copy the **Client ID** and generate a **Client Secret** for the next step.

**If the name is already taken:** You (or your organization) already have this app! Just use the existing app's credentials instead of creating a new one. Find it at [GitHub Settings > Apps](https://github.com/settings/apps).

### Manual Step 3: Create .env File in This Directory

```bash
cat > .env << 'EOF'
ANTHROPIC_API_KEY=sk-ant-api03-YOUR-API-KEY-HERE
CLAUDE_AGENT_SDK_CONTAINER_API_KEY=pick-any-random-string-as-your-api-key

# GitHub App Configuration (Required for web CLI)
GITHUB_CLIENT_ID=your_github_app_client_id
GITHUB_CLIENT_SECRET=your_github_app_client_secret

# Required: GitHub access control for web CLI (choose one or both)
ALLOWED_GITHUB_USERS=user1,user2,user3
# ALLOWED_GITHUB_ORG=yourcompany
EOF
```

**Required:**

- **ANTHROPIC_API_KEY**: Your Anthropic API key from https://console.anthropic.com/settings/keys
- **CLAUDE_AGENT_SDK_CONTAINER_API_KEY**: API key for REST endpoint protection
- **GITHUB_CLIENT_ID**: GitHub OAuth App Client ID
- **GITHUB_CLIENT_SECRET**: GitHub OAuth App Client Secret

**Required GitHub Access Control (for security):**

- **ALLOWED_GITHUB_USERS**: Comma-separated list of GitHub usernames allowed to access web CLI
- **ALLOWED_GITHUB_ORG**: GitHub organization name (users from this org can access web CLI)

**GitHub Access Behavior:**

- **At least one allowlist must be configured** - the container will not start without proper access control
- You must set either `ALLOWED_GITHUB_USERS` or `ALLOWED_GITHUB_ORG` (or both)
- This prevents unauthorized access to your Claude instance

### Manual Step 4: Run the app

Tell Claude Code:

```
Please build the Docker container, run it, and verify it's working
```

Or run manually:

```bash
./test.sh
```

**Note:** All scripts automatically sanitize directory names (lowercase, alphanumeric only) for Docker compatibility.

</details>

## 🎯 What You Get After Setup

Once setup completes, you'll have:

**🌐 Web CLI Interface** at `http://localhost:8080`

- Sign in with your GitHub account (one-click OAuth)
- Real-time streaming terminal interface
- Full conversational context maintained across messages

**🔧 REST API** at `http://localhost:8080/query`

- Secure API key authentication
- Built-in multi-agent collaboration system
- Easy integration with any application

### Try the Multi-Agent System

Test the built-in Canadian 🍁 and Australian 🇦🇺 agents:

```bash
curl -X POST http://localhost:8080/query \
  -H "Content-Type: application/json" \
  -H "X-API-Key: your-api-key-here" \
  -d '{"prompt": "Help me plan a vacation"}'
```

You'll see both agents discuss your request and provide their unique perspectives!

---

<details>

<summary>☁️ Deployment Options - Let Claude Code deploy this for you!</summary>

| Platform             | Service                              | “Deploy a Dockerized app” docs                                                                                                                                                                                                                  |
| -------------------- | ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AWS                  | App Runner                           | [Getting started with App Runner](https://docs.aws.amazon.com/apprunner/latest/dg/getting-started.html)                                                                                                                                         |
| AWS                  | Amazon ECS (Fargate)                 | [Getting started with Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/getting-started-fargate.html)                                                                                                                        |
| AWS                  | Elastic Beanstalk (Docker)           | [Deploying with Docker containers](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html)                                                                                                                            |
| AWS                  | Lightsail Containers                 | [Deploy and manage containers](https://docs.aws.amazon.com/lightsail/latest/userguide/amazon-lightsail-container-services.html)                                                                                                                 |
| Google Cloud         | Cloud Run                            | [Deploying container images to Cloud Run](https://cloud.google.com/run/docs/deploying)                                                                                                                                                          |
| Google Cloud         | Google Kubernetes Engine (GKE)       | [Quickstart: Deploy an app to a GKE cluster](https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster)                                                                                                                                |
| Google Cloud         | App Engine Flexible (custom runtime) | [Build custom runtimes (Dockerfile)](https://cloud.google.com/appengine/docs/flexible/custom-runtimes/build)                                                                                                                                    |
| Azure                | Container Apps                       | [Quickstart: Deploy your first container app](https://learn.microsoft.com/en-us/azure/container-apps/get-started) • [Deploy existing image](https://learn.microsoft.com/en-us/azure/container-apps/get-started-existing-container-image-portal) |
| Azure                | App Service (Web App for Containers) | [Quickstart: Run a custom container on App Service](https://learn.microsoft.com/en-us/azure/app-service/quickstart-custom-container)                                                                                                            |
| Azure                | Container Instances (ACI)            | [Quickstart: Deploy a container instance](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)                                                                                                    |
| Azure                | AKS (Kubernetes)                     | [Quickstart: Deploy an AKS cluster & app (CLI)](https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-cli)                                                                                                                  |
| Fly.io               | Machines / Launch                    | [Deploy with a Dockerfile](https://fly.io/docs/languages-and-frameworks/dockerfile/)                                                                                                                                                            |
| Railway              | Services                             | [Build from a Dockerfile](https://docs.railway.com/guides/dockerfiles)                                                                                                                                                                          |
| Render               | Web Services                         | [Docker on Render](https://render.com/docs/docker)                                                                                                                                                                                              |
| Porter               | Web Services                         | [Deploy from GitHub Repository](https://docs.porter.run/deploy/deploy-from-github-repo) • [See Porter deployment guide below](#deploying-on-porter)                                                                                             |
| DigitalOcean         | App Platform                         | [How to deploy from container images](https://docs.digitalocean.com/products/app-platform/how-to/deploy-from-container-images/)                                                                                                                 |
| Heroku               | Container Registry & Runtime         | [Container Registry & Runtime (Docker Deploys)](https://devcenter.heroku.com/articles/container-registry-and-runtime)                                                                                                                           |
| Kubernetes (generic) | —                                    | [Using kubectl to create a Deployment](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/)                                                                                                                         |

</details>

## 🚀 Deploying on Porter

Porter makes it easy to deploy this containerized application to your GCP account. Follow these steps:

### Prerequisites

- ✅ GCP account connected to Porter (you mentioned you already have this!)
- ✅ GitHub repository with this code (or fork [this repo](https://github.com/receipting/claude-agent-sdk-container))
- ✅ GitHub App credentials (from `./setup-api-key.sh` or manual setup)
- ✅ Anthropic API key from [console.anthropic.com](https://console.anthropic.com/settings/keys)

### Step 1: Install Porter GitHub App

1. Go to your Porter dashboard
2. Navigate to **Account Settings** → **GitHub Integration**
3. Install the Porter GitHub App and grant access to your repository

### Step 2: Create New Application

1. In Porter dashboard, click **Create New Application**
2. Select **Deploy from GitHub Repository**
3. Choose your repository: `claude-agent-sdk-container` (or your fork)
4. Select branch: `main` (or your preferred branch)
5. Set **Root Path**: `./` (default - this is correct for this repo)

### Step 3: Configure Build Settings

Porter will automatically detect the `Dockerfile` and use Docker build method. You can verify in **Configure Build Settings**:

- **Build Method**: Docker
- **Dockerfile Path**: `./Dockerfile` (default)
- **Build Context**: `./` (default)

### Step 4: Add Web Service

1. In **Add Application Services**, click **Add Service**
2. Select **Web** service type
3. Configure the service:
   - **Name**: `web`
   - **Port**: `8080`
   - **HTTP Path**: `/` (serves the web CLI)
   - **Health Check**: Enable
     - **Health Check Path**: `/health`
     - **Initial Delay**: `10` seconds
     - **Period**: `10` seconds
     - **Timeout**: `5` seconds

### Step 5: Add Environment Variables

Add all required environment variables in **Environment Variables** section:

**Required Variables:**

```bash
# Anthropic API Key (get from https://console.anthropic.com/settings/keys)
ANTHROPIC_API_KEY=sk-ant-api03-YOUR-API-KEY-HERE

# GitHub OAuth Configuration (from your GitHub App setup)
GITHUB_CLIENT_ID=your_github_app_client_id
GITHUB_CLIENT_SECRET=your_github_app_client_secret

# GitHub Access Control (REQUIRED - choose at least one)
ALLOWED_GITHUB_USERS=your-github-username,other-user
# OR
ALLOWED_GITHUB_ORG=your-organization-name
```

**Optional Variables:**

```bash
# API Key for REST endpoint protection (recommended)
CLAUDE_AGENT_SDK_CONTAINER_API_KEY=your-secure-random-api-key

# JWT Session Secret (auto-generated if not provided)
SESSION_SECRET=your-random-secret-here

# Server Port (default: 8080)
PORT=8080
```

**💡 Tip:** Use Porter's **Environment Groups** feature to manage environment variables across multiple environments (staging, production, etc.).

### Step 6: Configure Health Checks

Porter will automatically configure health checks based on the `porter.yaml` file, but you can verify:

- **Health Check Enabled**: ✅ Yes
- **Path**: `/health`
- **Expected Response**: JSON with `"status": "healthy"`

### Step 7: Deploy!

1. Click **Deploy App** button
2. Porter will:

   - Build your Docker image from the GitHub repository
   - Push it to Porter's container registry
   - Deploy to your GCP cluster
   - Set up load balancing and HTTPS
   - Configure health checks

3. **Authorize GitHub Actions**: Porter will prompt you to authorize creating a GitHub Actions workflow file. This enables automatic deployments on every push to your branch.

### Step 8: Access Your Application

Once deployed, Porter will provide:

- **Public URL**: `https://your-app-name.porter.run` (or your custom domain)
- **Web CLI**: Visit the URL in your browser and sign in with GitHub
- **REST API**: `https://your-app-name.porter.run/query`

### Using porter.yaml (Configuration as Code)

This repository includes a `porter.yaml` file for Configuration as Code. If you prefer to manage configuration via YAML:

1. The `porter.yaml` file is already in the repository
2. Porter will automatically detect and use it
3. You can still override settings in the Porter UI if needed

**Benefits of porter.yaml:**

- ✅ Version control your infrastructure configuration
- ✅ Consistent deployments across environments
- ✅ Easy to review changes in pull requests
- ✅ Reproducible deployments

### Updating Your Deployment

**Automatic Deployments:**

- Every push to your source branch triggers a new deployment
- Porter builds a new Docker image and deploys it
- Zero-downtime deployments with health checks

**Manual Updates:**

- Go to your application in Porter dashboard
- Click **Redeploy** to trigger a new build
- Or update environment variables and click **Save**

### Troubleshooting Porter Deployment

| Issue                            | Solution                                                                                                            |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Build fails**                  | Check build logs in Porter dashboard. Common issues: missing dependencies, Dockerfile errors                        |
| **Health check failing**         | Verify `/health` endpoint returns `{"status": "healthy"}`. Check environment variables are set correctly            |
| **GitHub OAuth not working**     | Verify `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET` are correct. Update GitHub App callback URL to your Porter URL |
| **"GitHub user not authorized"** | Check `ALLOWED_GITHUB_USERS` or `ALLOWED_GITHUB_ORG` includes your GitHub username/organization                     |
| **Container crashes on startup** | Check application logs in Porter. Usually missing required environment variables                                    |

### Porter-Specific Features

**Environment Groups:**

- Create separate environment groups for staging/production
- Share common variables across environments
- Override per-environment as needed

**Custom Domains:**

- Add your custom domain in Porter dashboard
- Porter automatically provisions SSL certificates
- Update GitHub App callback URL to match your domain

**Autoscaling:**

- Configure autoscaling based on CPU/memory usage
- Set min/max replicas for high availability
- Porter handles load balancing automatically

**Monitoring & Logs:**

- View real-time logs in Porter dashboard
- Set up alerts for health check failures
- Monitor resource usage and costs

### Next Steps

- ✅ Test the web CLI at your Porter URL
- ✅ Test the REST API with a curl command
- ✅ Set up a custom domain (optional)
- ✅ Configure autoscaling for production workloads
- ✅ Set up monitoring alerts

**🎉 Congratulations!** Your Claude Agent SDK is now running on Porter with automatic deployments, health checks, and HTTPS!

<details>
<summary>📚 Full Manual Instructions (if you really want to do it yourself)</summary>

## Manual Setup

### Prerequisites

- Docker installed on your machine
- Claude Code OAuth token from setup above

### Clone and Run

```bash
# Clone the repository
git clone <repository-url>
cd claude-code-sdk-container

# Copy and edit the .env file (NO QUOTES in values!)
cp .env.example .env
# Edit .env and add your actual tokens (without quotes)

# Build the Docker image (uses directory name for uniqueness)
docker build -t claude-code-$(basename "$(pwd)") .

# Run the container (use --env-file for .env file)
docker run -d --name claude-code-$(basename "$(pwd)") -p 8080:8080 --env-file .env claude-code-$(basename "$(pwd)")

# IMPORTANT: Check if container is actually running!
docker ps | grep claude-code-$(basename "$(pwd)")
# If not visible, check logs:
docker logs claude-code-$(basename "$(pwd)")
```

### Test It's Working

```bash
# Easy way - run the test script:
./test.sh

# Check for SDK updates:
./update.sh

# Or manually test:
# 1. First check health (no auth required) - should return JSON
curl http://localhost:8080/health

# 2. Test query endpoint (WORKING EXAMPLE - copy exactly!)
curl -X POST http://localhost:8080/query \
  -H "Content-Type: application/json" \
  -H "X-API-Key: your-api-key-here" \
  -d '{"prompt": "Say hello"}'

# Common mistakes to avoid:
# ❌ Missing quotes around JSON
# ❌ Smart quotes instead of straight quotes
# ❌ Missing -X POST
# ❌ Wrong header format
```

## API Usage

### Authentication

The `/query` endpoint requires an API key. You can provide it in two ways:

```bash
# Option 1: X-API-Key header
curl -H "X-API-Key: your-api-key-here"

# Option 2: Authorization Bearer header
curl -H "Authorization: Bearer your-api-key-here"
```

The health check endpoint (`/health`) is public and doesn't require authentication.

### Health Check (No Auth Required)

```bash
GET http://localhost:8080/health
```

Returns:

```json
{
  "status": "healthy",
  "hasToken": true,
  "sdkLoaded": true,
  "message": "Claude Code SDK API",
  "timestamp": "2025-09-18T23:30:00.000Z"
}
```

### Query Claude (Auth Required)

```bash
POST http://localhost:8080/query
Content-Type: application/json
X-API-Key: your-api-key-here

{
  "prompt": "Your question here",
  "options": {
    "model": "claude-sonnet-4-5"  // optional
  }
}
```

Returns:

```json
{
  "success": true,
  "response": "Claude's response",
  "messageCount": 3,
  "timestamp": "2025-09-18T23:30:00.000Z"
}
```

## Deployment

### Using Docker Compose

Create `docker-compose.yml`:

```yaml
version: '3.8'
services:
  claude-api:
    image: claude-code-$(basename "$(pwd)")
    container_name: claude-code-$(basename "$(pwd)")
    ports:
      - '8080:8080'
    environment:
      - CLAUDE_CODE_OAUTH_TOKEN=${CLAUDE_CODE_OAUTH_TOKEN}
    restart: unless-stopped
```

Then run:

```bash
docker-compose up -d
```

## Environment Variables

| Variable                             | Required    | Description                                                             |
| ------------------------------------ | ----------- | ----------------------------------------------------------------------- |
| `ANTHROPIC_API_KEY`                  | Yes\*       | Your Anthropic API key from https://console.anthropic.com/settings/keys |
| `CLAUDE_CODE_OAUTH_TOKEN`            | Yes\*       | Alternative: Claude Code OAuth token (requires Anthropic permission)    |
| `CLAUDE_AGENT_SDK_CONTAINER_API_KEY` | No\*\*      | API key for endpoint authentication                                     |
| `GITHUB_CLIENT_ID`                   | Yes\*\*\*   | GitHub App Client ID                                                    |
| `GITHUB_CLIENT_SECRET`               | Yes\*\*\*   | GitHub App Client Secret                                                |
| `ALLOWED_GITHUB_USERS`               | Yes\*\*\*\* | Comma-separated list of allowed GitHub usernames                        |
| `ALLOWED_GITHUB_ORG`                 | Yes\*\*\*\* | GitHub organization name for access control                             |
| `SESSION_SECRET`                     | No          | JWT signing secret (generate with: `openssl rand -hex 32`)              |
| `PORT`                               | No          | Server port (default: 8080)                                             |

\*Either `ANTHROPIC_API_KEY` or `CLAUDE_CODE_OAUTH_TOKEN` must be set. Use `ANTHROPIC_API_KEY` (recommended).
**If `CLAUDE_AGENT_SDK_CONTAINER_API_KEY` is not set, the `/query` endpoint will be publicly accessible. \***Required only for web CLI access. REST API works without GitHub App authentication.
\*\*\*\*At least one of `ALLOWED_GITHUB_USERS` or `ALLOWED_GITHUB_ORG` must be set for security.

### Alternative: OAuth Token Authentication (Requires Anthropic Permission)

If you have prior approval from Anthropic to use Claude Code OAuth tokens, you can use the OAuth setup approach:

**Prerequisites:**

```bash
npm install -g @anthropic-ai/claude-code
```

**Setup:**

```bash
./setup-tokens.sh
```

This script will:

- Get your Claude OAuth token via `claude setup-token`
- Set up GitHub App
- Configure access control
- Write credentials to `.env`

**Manual OAuth Setup:**

```bash
# Get OAuth token
claude setup-token

# Add to .env file
CLAUDE_CODE_OAUTH_TOKEN=sk-ant-oat01-YOUR-OAUTH-TOKEN-HERE
# (instead of ANTHROPIC_API_KEY)
```

**⚠️ Important:** OAuth tokens require prior approval from Anthropic. For most users, use `ANTHROPIC_API_KEY` instead.

### GitHub Access Control

Control who can access the web CLI interface by configuring GitHub allowlists:

```bash
# Allow specific GitHub usernames
ALLOWED_GITHUB_USERS=user1,user2,admin

# Allow users from a GitHub organization
ALLOWED_GITHUB_ORG=mycompany

# Combine both (user must match either usernames OR organization)
ALLOWED_GITHUB_USERS=admin,specialuser
ALLOWED_GITHUB_ORG=mycompany
```

**Access Control Behavior:**

- **At least one allowlist must be configured** - the container will not start without proper access control
- **Case insensitive**: GitHub usernames are normalized to lowercase
- **Organization check**: Currently checks user's public company field (basic implementation)
- **Error response**: Unauthorized users receive `GitHub user not authorized`

**Examples:**

```bash
# Organization-only access
ALLOWED_GITHUB_ORG=mycompany

# Specific users only
ALLOWED_GITHUB_USERS=alice,bob,charlie

# Mixed access (admin + entire organization)
ALLOWED_GITHUB_USERS=admin
ALLOWED_GITHUB_ORG=mycompany
```

**Container Naming**: Each deployment automatically gets a unique container name based on the directory name (e.g., `claude-code-my-project`). This allows multiple deployments without conflicts.

**Note**: For production use with organization membership, consider implementing proper GitHub API calls to check organization membership instead of relying on the public company field.

## Examples

### Multi-Agent Discussion (Built-in Feature)

The `/query` endpoint includes a built-in multi-agent system where two specialized agents discuss user requests:

- **Canadian Agent** 🍁: Friendly, polite, optimistic perspective with Canadian warmth
- **Australian Agent** 🇦🇺: Laid-back, practical, easy-going perspective with Aussie charm

The coordinator agent uses Claude's Task tool to delegate to each subagent, gather their perspectives, and synthesize a comprehensive response.

```bash
curl -X POST http://localhost:8080/query \
  -H "Content-Type: application/json" \
  -H "X-API-Key: your-api-key-here" \
  -d '{"prompt": "Help me plan a vacation"}'
```

**Example Response:**
The agents will discuss the request, each providing their unique cultural perspective, followed by a synthesized recommendation combining both viewpoints.

### Python

```python
import requests

response = requests.post('http://localhost:8080/query',
    headers={'X-API-Key': 'your-api-key-here'},
    json={'prompt': 'Explain quantum computing in simple terms'})
print(response.json()['response'])
```

### JavaScript

```javascript
const response = await fetch('http://localhost:8080/query', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-API-Key': 'your-api-key-here',
  },
  body: JSON.stringify({ prompt: 'Write a haiku about coding' }),
})
const data = await response.json()
console.log(data.response)
```

### cURL

```bash
curl -X POST http://localhost:8080/query \
  -H "Content-Type: application/json" \
  -H "X-API-Key: your-api-key-here" \
  -d '{
    "prompt": "What is the meaning of life?",
    "options": {
      "model": "claude-sonnet-4-5"
    }
  }' | jq .response
```

### Customizing the Multi-Agent System

The multi-agent implementation is in `server.ts` (lines 197-220). To customize:

1. **Modify agent personalities**: Edit the `prompt` field in each agent definition
2. **Add more agents**: Add new entries to the `agents` object
3. **Change coordination strategy**: Modify the `coordinatedPrompt` to change how agents collaborate
4. **Disable multi-agent**: Remove the `agents` and `coordinatedPrompt` for single-agent responses

**Example: Adding Your Own Agent**

Edit `server.ts` and add to the `agents` object:

```typescript
agents: {
  canadian_agent: { /* existing */ },
  australian_agent: { /* existing */ },
  your_custom_agent: {
    description: "Provides a [personality] perspective on the user's request",
    prompt: `You are a [personality description]. Use expressions like "[phrase1]", "[phrase2]".
Be [characteristics]. Give [type of advice].
Keep your responses concise (2-3 sentences) and always make it clear you're the [name] perspective.`,
    model: 'sonnet' as const
  }
}
```

Then update the `coordinatedPrompt` to include your new agent:

```typescript
const coordinatedPrompt = `The user has sent this request: "${prompt}"

Please coordinate with the canadian_agent, australian_agent, and your_custom_agent subagents...`
```

**How It Works:**

- The coordinator agent receives the user's query
- Uses Claude's Task tool to delegate to each subagent
- Each subagent provides their unique perspective
- Coordinator synthesizes all responses into a comprehensive answer

**Example Personalities to Try:**

- 🇬🇧 British agent (proper, witty, tea-enthusiast)
- 🇩🇪 German agent (efficient, precise, engineering-focused)
- 🇮🇹 Italian agent (passionate, expressive, food-oriented)
- 🇯🇵 Japanese agent (respectful, detail-oriented, harmony-focused)
- 🤠 Texan agent (bold, entrepreneurial, BBQ-loving)

## Troubleshooting

### Quick Debug Checklist

```bash
# 1. Is container running?
docker ps | grep claude-code

# 2. Check container logs
docker logs claude-code-$(basename "$(pwd)")

# 3. Test health endpoint (should work without auth)
curl http://localhost:8080/health

# 4. Test with your actual API key
curl -X POST http://localhost:8080/query \
  -H "Content-Type: application/json" \
  -H "X-API-Key: YOUR_ACTUAL_KEY_HERE" \
  -d '{"prompt": "test"}'
```

### Common Issues

| Issue                                           | Solution                                                                                            |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------- | -------------------------------- |
| **Container exits immediately**                 | Check logs: `docker logs claude-code-$(basename "$(pwd)")`. Usually bad OAuth token                 |
| **"Unauthorized - Invalid or missing API key"** | Your API key doesn't match. Check: `docker exec claude-code-$(basename "$(pwd)") env                | grep CLAUDE_AGENT_SDK_CONTAINER` |
| **Connection refused on port 8080**             | Container not running. Check: `docker ps`. Restart: `docker start claude-code-$(basename "$(pwd)")` |
| **Quotes in environment variables**             | Remove ALL quotes from .env file. Docker doesn't strip them!                                        |
| **"unhealthy" status**                          | OAuth token is wrong. Get correct one with: `claude setup-token`                                    |
| **Works locally but not from other container**  | Use `host.docker.internal:8080` instead of `localhost:8080`                                         |
| **Changes to .env not working**                 | Must restart container: `docker restart claude-code-$(basename "$(pwd)")`                           |

## Updating Claude Agent SDK

The container includes a specific version of the Claude Agent SDK. To update to the latest version:

```bash
# Run the update script
./update.sh

# This will:
# 1. Check for SDK updates
# 2. Update package if needed
# 3. Rebuild the container
# 4. Restart with new version
```

The update script handles everything automatically, including graceful container restart.

## Technical Details

- **Base Image**: Node.js 22 Alpine (optimized for size)
- **Container Size**: ~331MB
- **Memory Usage**: ~256MB
- **Supported Models**: All Claude Agent SDK models
- **SDK Version**: Locked at build time (use `./update.sh` to update)

## License

MIT

## Credits

Thanks to [cabinlab/claude-code-sdk-docker](https://github.com/cabinlab/claude-code-sdk-docker) for examples on implementing `setup-token` authentication flow.

</details>
