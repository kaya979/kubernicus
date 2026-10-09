# 🦎 openSUSE Slowroll Setup Guide

This guide outlines how to establish the `kubernicus` local environment on an **openSUSE Slowroll** workstation using Docker and Minikube.

---

## 🐋 Step 1: Install & Configure Docker
Docker is natively available in the official openSUSE repositories.

### 1. Install the packages
Refresh your `zypper` repositories and install Docker along with the Compose plugin:
```bash
sudo zypper refresh
sudo zypper install docker docker-compose
```

### 2. Manage the Systemd Daemon
Enable Docker to start automatically at system boot, then spin up the engine:
```bash
sudo systemctl enable docker
sudo systemctl start docker
```

### 3. Run Without Sudo (Rootless Execution)
To prevent needing `sudo` for every command, add your local account to the system's `docker` group:
```bash
sudo usermod -aG docker $USER
```
*⚠️ **Crucial Step:** You must log out of your desktop session and log back in (or restart your laptop) for this group change to take effect.*

### 4. Verify Docker
Test your local engine by pulling down the standard test image:
```bash
docker run hello-world
```

---

## 🪐 Step 2: Install Kubernetes Tools

For a laptop sandbox, we install **kubectl** (the command-line pilot) and **Minikube** (which creates a local Kubernetes node inside a secure Docker container).

### 1. Download and Install Minikube
Pull the official Linux AMD64 binary from Google's servers and place it in your local system path:
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
rm minikube-linux-amd64
```

### 2. Download and Install kubectl
Grab the stable Kubernetes CLI binary and install it to control your cluster:
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://k8s.io)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
rm kubectl
```

---

## 🚀 Step 3: Launching your Universe

With Docker acting as our infrastructure engine, we can instruct Minikube to spin up a Kubernetes control plane planet:

```bash
# Start Minikube using the native Docker engine driver
minikube start --driver=docker

# Verify that kubectl can map your cluster coordinates
kubectl get nodes
```

If successful, you will see `minikube` listed with a `Ready` status! Your `kubernicus` workspace is now fully initialized.
