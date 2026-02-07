# Ollama AI Setup Guide (Ubuntu 24.04 – Predator Server)

![Ollama AI] https://seeklogo.com/images/O/ollama-logo-593420.png

![Ollama Logo](https://seeklogo.com/images/O/ollama-logo-593420.png)

https://cdn.clipart.email/5b14a8e44b41dcb877e45ef0e4cf263e_docker-logo-png-docker-logo-transparent-png-image_512.png
![Docker Logo](https://cdn.clipart.email/5b14a8e44b41dcb877e45ef0e4cf263e_docker-logo-png-docker-logo-transparent-png-image_512.png)

> 🌟 **World's most complete guide to install and run Ollama AI locally on Ubuntu!**  

---

## 🔗 About Me

- **LinkedIn:** [Raja Ramees](https://www.linkedin.com/in/raja-ramees-804a7b262)  
- **Company Website:** [Ready AI Resources](https://readyairesources.com/)  

---

## 🖥️ System Requirements

- Ubuntu 24.04 LTS (Server or Desktop)
- Minimum 8GB RAM recommended
- Docker installed for Web UI integration
- Stable Internet connection

![Ubuntu](https://assets.ubuntu.com/v1/29985a98-ubuntu-logo32.png)  
![Docker](https://www.docker.com/sites/default/files/d8/2019-07/Moby-logo.png)

---

## 1️⃣ Install Ollama CLI

Official binaries are available for Linux.

### Step 1: Download & Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh


### Step 1: Download & Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh

✅ This automatically installs the ollama binary to:

/usr/local/bin or

/home/$USER/.ollama/bin

Step 2: Check Ollama Service
systemctl status ollama

2️⃣ Start Ollama Local Server

Ollama exposes a local HTTP API for testing.

# Start the server
ollama serve

# Test with curl
curl http://localhost:11434/api/generate -d '{"model":"model-name-1","prompt":"Hello Ollama"}'


Expected output: JSON response from the model.

3️⃣ Run Ollama as a Systemd Service (Optional)

Automatically start Ollama on boot.

sudo nano /etc/systemd/system/ollama.service


Paste the following:

[Unit]
Description=Ollama AI Server
After=network.target

[Service]
User=YOUR_USERNAME
ExecStart=/usr/local/bin/ollama serve
Restart=always

[Install]
WantedBy=multi-user.target


Enable & start the service:

sudo systemctl daemon-reload
sudo systemctl enable --now ollama

4️⃣ Download Models & Run

Go to Ollama Website

Scroll down to the Linux download option

Run the chosen models on terminal:

raja@ready-Predator-PO3-620:~/predator/:ollama run llama3.2:1b
>>> type anything and it will give you an answer

5️⃣ Integrate Ollama with Docker (Web UI)

We can use Open Web UI to access Ollama in browser.

Step 1: Run Open Web UI Docker container

docker run -d -p 11437:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  --restart always \
  ghcr.io/open-webui/open-webui:main


⚠️ If permission denied:

sudo usermod -aG docker $USER
logout
login again


Step 2: Access UI in browser
http://10.0.0.195:11437

6️⃣ Fix Docker Gateway for Ollama UI

Check Docker gateway:

docker network inspect bridge


Example output:

"Gateway": "172.17.0.1"


Update Ollama systemd service:

sudo nano /etc/systemd/system/ollama.service


Paste:

[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/local/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=/usr/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin"
Environment="OLLAMA_HOST=172.17.0.1"  # Use "0.0.0.0" for full access

[Install]
WantedBy=default.target


Reload and restart:

sudo systemctl daemon-reload
sudo systemctl restart ollama
sudo systemctl status ollama


Finally, go to http://10.0.0.195:11437
 to access Ollama Web UI.
Click on Models to see downloaded models and use them in the UI.

🚀 You're Ready!

📌 References

Ollama Official Site

Open Web UI GitHub

Made with ❤️ by Raja Ramees
LinkedIn
 | Company

---

If you want, I can **also make a version with logos/icons for each step** to make it **visually stunning like a world-class GitHub README**, just like top AI repos.  

Do you want me to do that next?


🎉 Now you can run Ollama AI locally, use the web UI, and test models seamlessly!
