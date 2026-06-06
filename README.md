# Mac Mini Local LLM Setup

This guide walks through setting up a local large language model on a Mac Mini using LM Studio and Opencode.

## Prerequisites

- Mac Mini
- Internet connection for initial model download

## Setup Steps

### 1. Install LM Studio

Download and install [LM Studio](https://lmstudio.ai) from their official website.

### 2. Download a Model

Search for and download your desired model within LM Studio. This setup uses:

**qwen3.6-35b-a3b**

### 3. Increase Context Window

Navigate to LM Studio settings and increase the model context window from the default `4096` to `256k`.

### 4. Enable Authentication

In LM Studio, enable authentication on the local model server for secure access.

### 5. Generate an API Key

Generate a new API key within LM Studio and store it securely — you'll need it shortly.

### 6. Install Opencode

Install Opencode via Homebrew:

```bash
brew install opencode
```

### 7. Start the Local Server

Load the model in LM Studio and start the local server.

### 8. Configure Opencode

Open `~/.config/opencode/opencode.jsonc` and replace its contents with:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://127.0.0.1:1234/v1"
      },
      "models": {
        "qwen/qwen3.6-35b-a3b": {
          "name": "qwen/qwen3.6-35b-a3b"
        }
      }
    }
  }
}
```

### 9. Connect to Local Server

Run `opencode` in your terminal, then execute the `/connect` command to connect to your local LM Studio server. Enter the API key generated in Step 5 when prompted.

### 10. You're All Set

Your Opencode session is now connected to your local LLM server and ready to use.

### 11. Expose Your Local LLM to Other Devices

You can access your local LLM from other devices on your network — such as a phone for chat or a laptop for code and chat — using Tailscale.

#### Step 1: Install Tailscale

Download and install [Tailscale](https://tailscale.com) (freemium, free for personal use) on your Mac Mini and on any device you want to connect from (laptop, phone, etc.).

#### Step 2: Join the Same Tailnet

Set up Tailscale on your Mac Mini (the server). Then join the same Tailscale network (called a `tailnet`) from your other devices.

#### Step 3: Find the Mac Mini's VPN IP

In the Tailscale dashboard or app, find the VPN IP address assigned to your Mac Mini.

#### Step 4: Configure Opencode on the Remote Device

On your laptop (or other device), edit `~/.config/opencode/opencode.jsonc` and replace the `baseURL` with the Mac Mini's VPN IP address (keeping the port `1234`):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "lmstudio": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "LM Studio (local)",
      "options": {
        "baseURL": "http://<MAC_MINI_VPN_IP>:1234/v1"
      },
      "models": {
        "qwen/qwen3.6-35b-a3b": {
          "name": "qwen/qwen3.6-35b-a3b"
        }
      }
    }
  }
}
```

#### Step 5: Connect

Run `opencode` on your laptop and execute the `/connect` command. Enter the API key generated in Step 5 of the main setup, and your session will connect to the Mac Mini's local LLM over Tailscale — working out of the box.

---

*This README was generated with the local LLM.*
