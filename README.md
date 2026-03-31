# nemoclaw-setup with ollama locally
Guide to setup NemoClaw locally with Ollama

NVIDIA NemoClaw is a tool designed to run OpenClaw autonomous AI agents securely within a sandboxed environment, often utilizing local LLMs via Ollama. It is geared towards Linux (Ubuntu 22.04+ recommended) and requires Node.js, Docker, and an NVIDIA API key.

Here is the step-by-step guide to installing NemoClaw with Ollama.

1.  **Prerequisites**
    Before installing NemoClaw, ensure your system has the following:
    Linux: Ubuntu 22.04 LTS recommended.
    Docker: Installed and running, with your user added to the docker group.
    Node.js: Version 20+ (Version 22 recommended).
    NVIDIA GPU: Required for local inference, with drivers installed.
    NVIDIA API Key: Obtained from build.nvidia.com. 

2.  **Install and Configure Ollama**
    NemoClaw routes local inference through Ollama. 

    a. Install Ollama
       _curl -fsSL https://ollama.com/install.sh | sh_

    b. Configure Ollama for Network Access: NemoClaw runs in a Docker container and needs to reach Ollama on the host. 
       Configure it to listen on all interfaces:
       _sudo mkdir -p /etc/systemd/system/ollama.service.d
       echo -e '[Service]\nEnvironment="OLLAMA_HOST=0.0.0.0:11434"' | sudo tee /etc/systemd/system/ollama.service.d/override.conf
       sudo systemctl daemon-reload
       sudo systemctl restart ollama_

    c. Pull a Model: Download a model suitable for your GPU (e.g., qwen3.5 or llama3):
       _ollama pull qwen3.5:4b_

3.  **Install Docker**
    https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

4.  **Install NemoClaw**
    Use the official one-line installer to install NemoClaw, which will also prompt you to configure the sandbox. 
    _curl -fsSL https://nvidia.com/nemoclaw.sh | sudo bash_

5.  **Onboarding and Sandbox Setup**
    Run the command:
    _nemoclaw onboard_
    
    The installer will initiate an interactive onboarding wizard. 
    a. Onboarding: The wizard will walk you through **creating a sandbox**, applying security policies, and configuring inference.
    b. Inference Provider: When asked, select Ollama as your provider.
    c. Model Selection: Choose the model you pulled in Step 2.
    d. Finish: Once completed, the script may ask to update your path or require you to open a new terminal.

6.  Verify Installation
    Verify that nemoclaw is correctly installed:
    _source ~/.bashrc
    nemoclaw --help_

After nemoclaw onboard or nemoclaw <name> connect, the gateway is forwarded to your host on port 18789.
