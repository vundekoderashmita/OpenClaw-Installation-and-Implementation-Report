# OpenClaw Installation and Implementation Report

*Technical Documentation of Installation Process, Challenges, and Solutions*

## Executive Summary

This report documents the installation and configuration of OpenClaw, an AI-powered Discord bot framework. The implementation encountered several technical challenges, primarily related to API quota limitations and model configuration issues. Through systematic troubleshooting, these issues were identified and resolved, providing valuable insights into the deployment of AI agent systems.

## 1. Introduction

### 1.1 Project Overview

OpenClaw is an AI agent framework that enables users to deploy intelligent bots across various platforms, like Telegram, WhatsApp and Discord. The system supports multiple AI providers and models, offering flexibility in deployment configurations.

### 1.2 Objectives

- Install and configure OpenClaw on a Windows system
- Integrate the bot with Discord for testing and deployment
- Document challenges encountered during implementation
- Identify solutions and best practices for future deployments

## 2. Installation Process

### 2.1 Prerequisites

Before beginning the installation, the following prerequisites were identified:

- Git installation for version control and repository access
- PowerShell (in this case) with appropriate execution policies
- Administrator privileges for gateway installation
- Active API keys from chosen AI provider (Google Gemini in this case)

### **2.2 Installation Steps**

**Step 1: Install Git**

Git was installed as the foundational requirement for accessing the OpenClaw repository.

**Step 2: Execute Installation Script**

The installation was initiated using PowerShell with the following command:

powershell

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

![option to change commands according to os](image.png)

option to change commands according to os

Upon executing this command, the installer prompted for Node.js installation. Node.js is a prerequisite for running the OpenClaw framework, and the download was completed as part of the installation process.

Initial execution encountered execution policy restrictions, requiring the following remediation:

powershell

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

**Step 3: Onboarding Configuration**

The onboarding process was initiated using the quickstart option:

powershell

```powershell
openclaw onboard
```

![image.png](image%201.png)

**3.1 Select AI Provider**

The system presented a list of available AI providers. For this implementation, **Google** was selected as the AI provider.

**3.2 API Key Configuration**

An API key was required to authenticate with the Google Gemini service. The API key was obtained from **Google AI Studio** ([https://aistudio.google.com/](https://aistudio.google.com/)).

To obtain the API key:

![image.png](image%202.png)

1. Navigate to Google AI Studio
2. Sign in with Google account
3. Click on "Get API Key"
4. Create a new API key or use an existing one
5. Copy the API key

The API key was then pasted into the OpenClaw configuration prompt.

**3.3 Choose Model**

From the available Google Gemini models, **gemini-2.5-flash** was selected for this deployment.

**3.4 Select Platform**

**Discord** was selected as the deployment platform for the bot.

**Step 4: Discord Bot Setup**

The Discord integration required detailed configuration through the Discord Developer Portal.

**4.1 Create the Discord Application**

1. Navigate to the Discord Developer Portal ([https://discord.com/developers/applications](https://discord.com/developers/applications))
2. Click "New Application"
3. Provide a name for the application
4. Click "Create"

![image.png](image%203.png)

**4.2 Create Bot User**

1. Open the "Bot" tab in the left sidebar
2. Click "Add Bot"
3. Confirm the bot creation
4. Copy the Bot Token
    - **Important:** Keep this token secure and do not share it publicly

**4.3 Invite the Bot to Your Server**

Navigate to OAuth2 → URL Generator in the Developer Portal:

**Required Scopes:**

- `bot`
- `applications.commands`

**Bot Permissions:**

- Select `administrator` (provides full permissions for bot functionality)

Copy the generated OAuth2 URL and paste it into a web browser. Select the server where you want to invite the bot and authorize the permissions.

**4.4 Obtain Discord Channel ID**

Enable Developer Mode in Discord:

1. Go to User Settings (gear icon)
2. Navigate to Advanced settings
3. Enable "Developer Mode"

Get the Channel ID:

1. Right-click on the desired text channel where the bot will operate
2. Select "Copy ID" from the context menu
3. Save this Channel ID for configuration

**4.5 Complete OpenClaw Discord Configuration**

Return to the OpenClaw onboarding process and provide:

- Discord Bot Token (obtained in step 4.2)
- Channel ID (obtained in step 4.4)

The system validates the credentials and completes the Discord integration setup.

**Step 5: Gateway Installation and Configuration**

The OpenClaw gateway required administrator privileges for installation. The following commands were executed sequentially:

powershell

```powershell
openclaw gateway install
```

powershell

```powershell
openclaw gateway start
```

powershell

```powershell
openclaw gateway run
```

These commands successfully deployed the gateway service and launched the web-based user interface.

**Step 6: Bot Pairing and Authentication**

Final integration steps:

1. Initiated a direct message with the Discord bot using “@botname”
2. The bot responded with a pairing code
3. Entered the pairing code in the OpenClaw web interface
4. Completed authentication, linking the Discord bot to the OpenClaw gateway

## 3. Successful Task Implementation

### 3.1 File Organization Task

The bot was tested with a practical file management task to verify full functionality.

**Task Objective:**

Organize a folder named "references" on the desktop containing mixed file types into categorized subfolders.

**Initial Request:**

`@openclaw can you organise the folder named "references" on my desktop`

**Bot Interaction Process:**

The bot demonstrated intelligent task clarification by asking for:

1. Specific organization preferences (sort by name/date/type, create subfolders, delete duplicates)
2. Full folder path for accurate access
3. Confirmation of proposed subfolder structure

![{D49CE0EF-5F6E-416E-ADD4-54D87361D0CA}.png](D49CE0EF-5F6E-416E-ADD4-54D87361D0CA.png)

**User Requirements:**

- Create subfolders based on file types
- Delete duplicate files
- Folder path: `C:\Users\DELL\Desktop\references`

**Files in Original Folder:**

- **Text files:** apple.txt, hello.txt
- **PDF files:** A_Systematic_Review_on_the_Use_of_AI_and_ML_for_Fighting_the_COVID-19_Pandemic.pdf, s40747-021-00424-8.pdf, s40747-021-00424-8 (1).pdf
- **Image files:** eye.jpg, eye - Copy.jpg, tree.avif, view.jpg
- **Video files:** sea.mp4, war.mp4, wierd.mp4

![image.png](image%204.png)

**Bot's Execution Plan:**

1. Created four subfolders: `Text`, `PDFs`, `Images`, `Videos`
2. Identified and flagged duplicate files for deletion
3. Moved files to respective subfolders based on file type
4. Deleted duplicate files (eye - Copy.jpg, s40747-021-00424-8 (1).pdf)

**Results:**

The bot successfully:

- Created all four subfolders
- Moved all 9 unique files to their appropriate categories
- Removed 2 duplicate files
- Organized the folder from 12 items to 4 well-structured subfolders

![image.png](image%205.png)

## 4. Challenges Encountered and Solutions

### 4.1 API Quota Exhaustion

**Issue:**

The API key was tied to a Google Cloud free-tier project with strict quota limits. Even simple messages like "hi" triggered quota exhaustion errors, causing immediate bot failure.

**Error Messages:**

`Agent failed before reply: All models failed
429 RESOURCE_EXHAUSTED
generate_content_free_tier_requests`

**Solution:**

1. Upgraded to a paid-tier Google Cloud project
2. Generated a new API key with adequate quota
3. Updated the API key in OpenClaw configuration

### 4.2 Provider Cooldown State

**Issue:**

After repeated quota errors, the system entered a cooldown state that blocked even valid paid-tier API keys.

**Error Message:**

`Provider google is in cooldown (all profiles unavailable)`

**Solution:**

1. Waited for the automatic cooldown period to expire (15-30 minutes)
2. Restarted the gateway service

### 4.3 Agent State Caching Problem

**Issue:**

Despite selecting gemini-2.5-flash in the UI, logs showed the system was using gemini-3-pro-preview. Agent configurations are cached separately from UI settings and don't update automatically.

**Cache Location:**

`~/.openclaw/agents/main`

**Solution:**

1. Stopped the OpenClaw gateway service
2. Deleted files in `~/.openclaw/agents/main`
3. Reconfigured settings through the UI
4. Restarted the gateway to generate fresh agent state

**Key Takeaway:**

Clear the agent cache whenever changing provider, model, or API key settings to ensure changes take effect immediately.

## 5. Conclusion

The implementation of OpenClaw offered valuable insights into deploying AI agent systems. Although the installation process was straightforward, configuration and operational challenges—particularly related to API quota limits and agent state caching—required careful troubleshooting. Free-tier API resources proved inadequate even for basic testing, and the caching mechanism revealed gaps between UI settings and actual runtime behavior, emphasizing the need to understand the system’s internal architecture.

Overall, the experience highlighted key best practices for successful AI agent deployment: proper resource planning, awareness of cache and state management, effective logging and monitoring, and clear documentation of troubleshooting steps. Future deployments would benefit from early adoption of paid-tier APIs, automated monitoring, and well-defined operational runbooks, enabling more stable, reliable, and scalable AI agent implementations.