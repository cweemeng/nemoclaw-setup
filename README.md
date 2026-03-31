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

    a. Install Ollama <br>
    ---------------------------------------------------<br>
       _curl -fsSL https://ollama.com/install.sh | sh_ <br>
    --------------------------------------------------- <br>
    b. Configure Ollama for Network Access: NemoClaw runs in a Docker container and needs to reach Ollama on the host. <br>
       Configure it to listen on all interfaces:<br>
       ---------------------------------------------------<br>
       _sudo mkdir -p /etc/systemd/system/ollama.service.d
       echo -e '[Service]\nEnvironment="OLLAMA_HOST=0.0.0.0:11434"' | sudo tee /etc/systemd/system/ollama.service.d/override.conf
       sudo systemctl daemon-reload
       sudo systemctl restart ollama_
       --------------------------------------------------- <br>
    c. Pull a Model: Download a model suitable for your GPU (e.g., qwen3.5 or llama3):<br>
    ---------------------------------------------------<br>
       _ollama pull qwen3.5:4b_ <br>
    ---------------------------------------------------<br>
3.  **Install Docker**<br>
    https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

4.  **Install NemoClaw**<br>
    Use the official one-line installer to install NemoClaw, which will also prompt you to configure the sandbox. <br>
    --------------------------------------------------- <br>
    _curl -fsSL https://nvidia.com/nemoclaw.sh | sudo bash_
    --------------------------------------------------- <br>

5.  **Onboarding and Sandbox Setup**<br>
    Run the command:<br>
    ---------------------------------------------------<br>
    _nemoclaw onboard_ <br>
    --------------------------------------------------- <br>
    The installer will initiate an interactive onboarding wizard. 
    a. Onboarding: The wizard will walk you through **creating a sandbox**, applying security policies, and configuring inference.
    b. Inference Provider: When asked, select Ollama as your provider.
    c. Model Selection: Choose the model you pulled in Step 2.
    d. Finish: Once completed, the script may ask to update your path or require you to open a new terminal.

6.  **Verify Installation**<br>
    Verify that nemoclaw is correctly installed:<br>
    --------------------------------------------------- <br>
    _source ~/.bashrc <br>
    nemoclaw --help_<br>
    --------------------------------------------------- <br>

After nemoclaw onboard or nemoclaw <name> connect, the gateway is forwarded to your host on port 18789.
