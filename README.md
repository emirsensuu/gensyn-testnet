<h2 align=center>Gensyn Testnet Node Guide</h2>

## 💻 System Requirements

| Requirement                        | Details                                                                                      |
|-------------------------------------|---------------------------------------------------------------------------------------------|
| **CPU Architecture**                | `arm64` or `amd64`                                                                          |
| **Recommended RAM**                 | 25 GB                                                                                       |
| **CUDA Devices (Recommended)**      | `RTX 3090`, `RTX 4090`, `A100`, `H100`                                                      |
| **Python Version**                  | Python >= 3.10 (For Mac, you may need to upgrade) 


## 🌐 Rent GPU
- Visit : [Quick Pod Website](https://console.quickpod.io?affiliate=60f49a2e-463f-490d-a57a-497276b22a09)
- Sign Up using email address
- Go to your email and verify your Quick Pod account
- Click on `Add` button in the corner to deposit fund
- You can deposit using crypto currency (from your EVM wallet) or using Credit Card
- Now go to `template` section and then select `CUDA 12.6` in the below
- Clone the CUDA 12.6, then edit the Docker Options as shown below.
- Now click on `Select GPU` and search `RTX 4090` and choose it
- Change your template via My Template Section
- Now choose a GPU and click on `Create POD` button
- Your GPU server will be deployed soon
- Now click on `Connect` option and then choose `Connect to web terminal`

## 🛜 Docker Options

```
-p 8888:8888 -p 3000:3000
```

## 📥 Installation

1. **Install `sudo`**
```bash
apt update && apt install -y sudo
```
2. **Install other dependencies**
```bash
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && sudo apt update && sudo apt install -y yarn
```
3. **Install Node.js and npm if not installed already**  
```bash
curl -sSL https://raw.githubusercontent.com/zunxbt/installation/main/node.sh | bash
```
4. **Clone this repository**
```bash
cd $HOME && [ -d rl-swarm ] && rm -rf rl-swarm; git clone https://github.com/gensyn-ai/rl-swarm.git && cd rl-swarm
```
5. **Create a `screen` session**
```bash
screen -S gensyn
```
6. **Run the swarm**
```bash
python3 -m venv .venv && . .venv/bin/activate && ./run_rl_swarm.sh
```
- It will ask some questions, you should send response properly
- ```Would you like to connect to the Testnet? [Y/n]``` : Write `Y`
- ```Would you like to push models you train in the RL swarm to the Hugging Face Hub? [y/N]``` : Write `N`
- When you will see interface like this, you can detach from this screen session

![Screenshot 2025-04-01 061641](https://github.com/user-attachments/assets/b5ed9645-16a2-4911-8a73-97e21fdde274)

7. **Detach from `screen session`**
- Use `Ctrl + A` and then press `D` to detach from this screen session.


## 🛜 Sign-In Options

1. Go ```chrome://flags/#unsafely-treat-insecure-origin-as-secure``` in your browser.
2. Search for "Insecure origins treated as secure" section.
3. Write here the Public IP and the exposed port 3000 of the device you rented.
4. To apply these changes, re-start your browser.

![How to check Exposed Port and Public IP](https://github.com/user-attachments/assets/18d118cc-8a61-463d-bbeb-9d8d83ed0548)

![http://<PUBLIC-IP>:<PORT>](https://github.com/user-attachments/assets/f4542346-837e-4304-9c50-78f0edfacc3e)

5. Then, go to your browser and search for ```<PUBLICIP>:<EXPOSEDPORT>```.
