# Gensyn Testnet Node Guide

## 🎯 Quick Start

This guide helps you set up a Gensyn testnet node on a GPU server.

## 💻 System Requirements

| Component | Requirement |
|-----------|-------------|
| **CPU** | `arm64` or `amd64` |
| **RAM** | 25 GB minimum |
| **GPU** | RTX 3090, RTX 4090, A100, or H100 |
| **Python** | 3.10 or higher |

## 🚀 Setup Steps

### 1. Rent a GPU Server

1. Visit [Quick Pod](https://console.quickpod.io?affiliate=60f49a2e-463f-490d-a57a-497276b22a09)
2. Sign up with your email and verify your account
3. Add funds (crypto or credit card)
4. Go to **Templates** → Select **CUDA 12.6**
5. Clone the template and edit Docker options:
   ```
   -p 8888:8888 -p 3000:3000
   ```
6. Select **RTX 4090** GPU
7. Click **Create POD**

### 2. Connect to Your Server

1. Click **Connect** → **Web Terminal**
2. Install dependencies:
   ```bash
   apt update && apt install -y sudo
   sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof
   curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
   echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   sudo apt update && sudo apt install -y yarn
   ```

3. Install Node.js:
   ```bash
   curl -sSL https://raw.githubusercontent.com/zunxbt/installation/main/node.sh | bash
   ```

### 3. Install Gensyn Node

1. Clone the repository:
   ```bash
   cd $HOME && [ -d rl-swarm ] && rm -rf rl-swarm
   git clone https://github.com/gensyn-ai/rl-swarm.git && cd rl-swarm
   ```

2. Start a tmux session:
   ```bash
   tmux
   ```

3. Run the swarm:
   ```bash
   python3 -m venv .venv && . .venv/bin/activate && ./run_rl_swarm.sh
   ```

4. Answer the prompts:
   - **Connect to Testnet?** → Type `Y`
   - **Push models to Hugging Face?** → Type `Y`
   - **Hugging Face token** → Paste your write token

5. Detach from tmux: Press `Ctrl + B`, then `D`

## 🔐 Managing Your Swarm Key

**⚠️ Important Warnings:**
- **Do not lose your `swarm.pem` file** - it's your node's identity
- **Always use the same email** for your node to maintain consistency

### Import/Export via Jupyter Lab

You can import or export your `swarm.pem` file through Jupyter Lab:

1. **Access Jupyter Lab** at `http://your-server-ip:8888`
2. **Upload/Download** your `swarm.pem` file through the file browser
3. **Keep backups** of your `swarm.pem` in a secure location

## 🌐 Access Your Node (Optional)

If you want to access your node from outside the server:

1. **Install Cloudflare Tunnel:**
   ```bash
   wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
   sudo dpkg -i cloudflared-linux-amd64.deb
   ```

2. **Create a tunnel:**
   ```bash
   cloudflared tunnel --url http://localhost:3000
   ```

3. **Follow the prompts to authenticate with your email**

4. **The tunnel will provide a public URL to access your node**

## 📞 Support

For questions: [@0xemir_ on X](https://x.com/0xemir_)

## 📺 Video Guide

[Watch the full setup video here](https://github.com/user-attachments/assets/df4c57ae-7044-4691-a01c-28ac687a93ff)
