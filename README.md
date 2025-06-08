# 🚀 Website Deployment Automation using CI/CD & GitHub Actions

This project automates the deployment of a website to an AWS EC2 instance using **GitHub Actions** as a CI/CD pipeline. All infrastructure is pre-provisioned, and deployment is triggered automatically on every push to the `main` branch. This project was built and configured by **Aryan Sharma**.

---

## 🛠️ Tech Stack

- **GitHub Actions**
- **AWS EC2 (Ubuntu Server)**
- **Apache2 Web Server**
- **Git**
- **SSH Deployment**
- **Bash Scripting**
- **PHP/HTML Website (Optional)**

---

## 📁 Project Structure

```
.github/workflows/
│   └── deploy.yml          # GitHub Actions CI/CD workflow
│
├── website/
│   ├── index.html
│   └── other-files...
│
├── deploy.sh               # Remote deployment script (optional)
├── README.md
```

---

## ⚙️ How It Works

1. Developer pushes code to GitHub.
2. GitHub Actions triggers `deploy.yml`.
3. Workflow uses SSH to connect to the EC2 server.
4. It clones or copies website files to `/var/www/html/`.
5. Apache serves the updated website.

---

## 🪜 Setup Instructions

### 1. Prepare EC2 Instance

- Launch a **Ubuntu EC2** instance.
- Install Apache:
  ```bash
  sudo apt update
  sudo apt install apache2 -y
  ```
- Enable ports `22 (SSH)` and `80 (HTTP)` in the security group.
- Add your public SSH key to `~/.ssh/authorized_keys`.

### 2. Add Secrets in GitHub Repo

Go to your repo → Settings → Secrets and variables → Actions → New repository secret:

| Secret Name         | Description                          |
|---------------------|--------------------------------------|
| `EC2_HOST`          | Public IP or DNS of your EC2         |
| `EC2_USER`          | e.g., `ubuntu`                       |
| `EC2_KEY`           | Your private key content (base64 or plain) |
| `TARGET_DIR`        | Remote path, e.g., `/var/www/html`   |

### 3. Configure Workflow (`.github/workflows/deploy.yml`)

```yaml
name: 🚀 Deploy Website to EC2

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: ⬇️ Checkout code
        uses: actions/checkout@v3

      - name: 🧾 Set up SSH
        run: |
          echo "${{ secrets.EC2_KEY }}" > key.pem
          chmod 400 key.pem

      - name: 🚀 Deploy to EC2
        run: |
          rsync -avz -e "ssh -i key.pem -o StrictHostKeyChecking=no" ./website/ ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }}:${{ secrets.TARGET_DIR }}
```

---

## ✅ Features

- Push-to-deploy from GitHub to live EC2 server
- Automated with GitHub Actions (no manual SSH required)
- Easy to scale and extend
- Minimal manual configuration needed

---

## 👨‍💻 Author

**Aryan Sharma**  
B.Tech CSE (AI & DS) | Poornima University  
GitHub: [@AryanSharma2206](https://github.com/AryanSharma2206)  
LinkedIn: [linkedin.com/in/aryan-sharma2206](https://www.linkedin.com/in/aryan-sharma2206)  
Location: Jaipur, India
