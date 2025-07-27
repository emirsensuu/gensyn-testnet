# Gensyn Testnet Node Setup Guide

## 🎯 Overview

This comprehensive guide helps you set up and manage a Gensyn testnet node on a GPU server. The Gensyn network is a decentralized compute network for AI training, and running a node allows you to participate in the network and earn rewards.

## 💻 System Requirements

| Component | Minimum Requirement | Recommended |
|-----------|-------------------|-------------|
| **CPU** | `arm64` or `amd64` | `amd64` |
| **RAM** | 25 GB | 32 GB+ |
| **GPU** | RTX 3090, RTX 4090, A100, or H100 | RTX 4090 or A100 |
| **Storage** | 50 GB | 100 GB+ |
| **Python** | 3.10 or higher | 3.11+ |
| **Network** | Stable internet connection | High-speed connection |

## 🚀 Quick Setup

### Step 1: Rent a GPU Server

1. **Visit Quick Pod**: Go to [Quick Pod Console](https://console.quickpod.io?affiliate=60f49a2e-463f-490d-a57a-497276b22a09)
2. **Create Account**: Sign up with your email and verify your account
3. **Add Funds**: Deposit funds using crypto or credit card
4. **Select Template**: Go to **Templates** → Select **CUDA 12.6**
5. **Configure Docker**: Clone the template and edit Docker options:
   ```bash
   -p 8888:8888 -p 3000:3000
   ```
6. **Choose GPU**: Select **RTX 4090** (recommended) or **RTX 3090**
7. **Create POD**: Click **Create POD** and wait for initialization

### Step 2: Connect and Prepare Server

1. **Access Terminal**: Click **Connect** → **Web Terminal**

2. **Install System Dependencies**:
   ```bash
   # Update package lists
   apt update && apt install -y sudo
   sudo apt update
   
   # Install essential packages
   sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof htop
   
   # Install Yarn for Node.js dependencies
   curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
   echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
   sudo apt update && sudo apt install -y yarn
   ```

3. **Install Node.js**:
   ```bash
   curl -sSL https://raw.githubusercontent.com/zunxbt/installation/main/node.sh | bash
   ```

### Step 3: Install and Configure Gensyn Node

1. **Clone Repository**:
   ```bash
   cd $HOME
   [ -d rl-swarm ] && rm -rf rl-swarm
   git clone https://github.com/gensyn-ai/rl-swarm.git
   cd rl-swarm
   ```

2. **Create Virtual Environment**:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Start tmux Session**:
   ```bash
   tmux new-session -d -s gensyn
   tmux attach-session -t gensyn
   ```

4. **Run the Swarm**:
   ```bash
   ./run_rl_swarm.sh
   ```

5. **Complete Setup Prompts**:
   - **Connect to Testnet?** → Type `Y` and press Enter
   - **Push models to Hugging Face?** → Type `Y` and press Enter
   - **Hugging Face token** → Paste your write token (get from [Hugging Face Settings](https://huggingface.co/settings/tokens))

6. **Detach from tmux**: Press `Ctrl + B`, then `D`

## 🔧 Advanced Setup

### Using the Auto-Restart Script

For better reliability, use the included auto-restart script:

1. **Make Script Executable**:
   ```bash
   chmod +x script.sh
   ```

2. **Start Auto-Restart Monitoring**:
   ```bash
   ./script.sh
   ```

3. **Useful Commands**:
   ```bash
   ./script.sh status    # Check node status
   ./script.sh logs      # View logs
   ./script.sh clean     # Clean up processes
   ./script.sh stop      # Stop auto-restart
   ```

### Managing tmux Sessions

```bash
# List sessions
tmux list-sessions

# Attach to session
tmux attach-session -t gensyn

# Detach from session (while inside)
Ctrl + B, then D

# Kill session
tmux kill-session -t gensyn
```

## 🔐 Security and Key Management

### ⚠️ Critical Warnings

- **NEVER lose your `swarm.pem` file** - it's your node's unique identity
- **Always use the same email** for your node to maintain consistency
- **Keep backups** of your `swarm.pem` in multiple secure locations
- **Never share your private keys** with anyone

### Managing Your Swarm Key

#### Method 1: Jupyter Lab (Recommended)

1. **Access Jupyter Lab**: Open `http://your-server-ip:8888` in your browser
2. **Upload/Download**: Use the file browser to manage your `swarm.pem`
3. **Backup**: Download and store in secure location

#### Method 2: Direct File Transfer

```bash
# Download from server
scp username@server-ip:~/rl-swarm/swarm.pem ./swarm.pem

# Upload to server
scp ./swarm.pem username@server-ip:~/rl-swarm/swarm.pem
```

#### Method 3: Cloud Storage

```bash
# Upload to cloud storage (example with rclone)
rclone copy swarm.pem remote:backups/

# Download from cloud storage
rclone copy remote:backups/swarm.pem ./
```

## 🌐 Remote Access Setup

### Option 1: Cloudflare Tunnel (Recommended)

1. **Install Cloudflare Tunnel**:
   ```bash
   wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
   sudo dpkg -i cloudflared-linux-amd64.deb
   ```

2. **Create Tunnel**:
   ```bash
   cloudflared tunnel --url http://localhost:3000
   ```

3. **Follow Authentication**: Complete the Cloudflare authentication process
4. **Access**: Use the provided public URL to access your node

### Option 2: SSH Port Forwarding

```bash
# Forward local port to remote server
ssh -L 3000:localhost:3000 username@server-ip

# Then access via http://localhost:3000
```

## 📊 Monitoring Your Node

### Check Node Status

```bash
# Using the script
./script.sh status

# Manual checks
tmux list-sessions
pgrep -af "python.*run_rl_swarm"
nvidia-smi  # Check GPU usage
```

### View Logs

```bash
# Real-time logs
./script.sh logs

# Manual log viewing
tail -f ~/gensyn_auto_restart.log
```

### Monitor Resources

```bash
# GPU usage
nvidia-smi -l 1

# System resources
htop

# Disk usage
df -h
```

## 🔧 Troubleshooting

### Common Issues

#### Node Won't Start
```bash
# Check if tmux session exists
tmux list-sessions

# Kill existing sessions
tmux kill-session -t gensyn

# Clean up processes
pkill -f "python.*run_rl_swarm"
pkill -f "python.*main"

# Restart
./script.sh start
```

#### Authentication Issues
```bash
# Remove cached auth data
rm -rf modal-login/temp-data/*.json

# Re-authenticate
./run_rl_swarm.sh
```

#### GPU Memory Issues
```bash
# Check GPU memory
nvidia-smi

# Restart if memory is stuck
sudo systemctl restart nvidia-persistenced
```

#### Network Issues
```bash
# Check connectivity
ping 8.8.8.8

# Check DNS
nslookup google.com

# Restart network (if needed)
sudo systemctl restart networking
```

### Performance Optimization

1. **GPU Memory**: Ensure you have at least 2GB free VRAM
2. **CPU Priority**: Run with nice priority for better performance
3. **Network**: Use stable, high-speed internet connection
4. **Storage**: Use SSD storage for better I/O performance

## 📈 Earning and Rewards

### How Rewards Work

- **Participation**: Earn rewards by participating in training rounds
- **Performance**: Better hardware = higher potential rewards
- **Uptime**: More uptime = more opportunities to earn
- **Network**: Rewards distributed based on contribution to the network

### Tracking Your Earnings

- Monitor your node's participation in the Gensyn dashboard
- Check logs for successful round completions
- Track your Hugging Face model uploads

## 🆘 Support and Community

### Getting Help

- **Documentation**: [Gensyn Docs](https://docs.gensyn.ai)
- **Discord**: [Gensyn Community](https://discord.gg/gensyn)
- **Twitter**: [@0xemir_](https://x.com/0xemir_)
- **GitHub**: [rl-swarm Repository](https://github.com/gensyn-ai/rl-swarm)

### Useful Commands Reference

```bash
# Node management
./script.sh start      # Start node
./script.sh stop       # Stop auto-restart
./script.sh status     # Check status
./script.sh logs       # View logs
./script.sh clean      # Clean processes

# tmux management
tmux list-sessions     # List sessions
tmux attach -t gensyn  # Attach to session
tmux kill-session -t gensyn  # Kill session

# System monitoring
nvidia-smi             # GPU status
htop                   # System resources
df -h                  # Disk usage
free -h                # Memory usage
```

## 📺 Video Tutorial

[Watch the complete setup video here](https://github.com/user-attachments/assets/df4c57ae-7044-4691-a01c-28ac687a93ff)

---

**Happy mining! 🚀**

Remember to star the repository: [https://github.com/gensyn-ai/rl-swarm](https://github.com/gensyn-ai/rl-swarm)
