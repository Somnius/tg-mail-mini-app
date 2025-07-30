# tgmail Mini App

A Telegram Mini App providing an embedded email client experience inside Telegram using IMAP/SMTP protocols.  
Built with **FastAPI** (backend), **React** (frontend), and the **Telegram Bot API**, deployed via Docker and Traefik.

---

## Project Overview

This project aims to offer a seamless email client interface directly inside Telegram’s WebApp environment, similar to Telegram’s built-in Wallet app. It supports:

- Telegram WebApp login authentication  
- User IMAP mailbox connection testing  
- Encrypted credential storage (planned)  
- Modular backend and frontend with Docker-compose deployment  
- Secure HTTPS access with automatic certificates via Traefik  
- Easy customization and extensibility  

---

## Features

- FastAPI backend with Telegram Bot webhook integration  
- React frontend integrated with Telegram WebApp SDK for authentication  
- IMAP connection testing endpoint with async Python IMAP client  
- Docker-based deployment for easy VPS setup  
- Reverse proxy with Traefik including security headers and automatic Let's Encrypt certs  

---

## Table of Contents

- [Prerequisites](#prerequisites)  
- [Quick Start — Docker VPS Deployment](#quick-start--docker-vps-deployment)  
- [Step-by-Step Manual Deployment (Native / Home Server)](#step-by-step-manual-deployment-native--home-server)  
- [Telegram Bot Setup](#telegram-bot-setup)  
- [Configuration](#configuration)  
- [Development](#development)  
- [License](#license)  
- [Credits and Attributions](#credits-and-attributions)  
- [Contributing](#contributing)  
- [Contact](#contact)  

---

## Prerequisites

- **VPS or server with public IP** or a home server with static IP and DNS setup  
- **Domain name** configured with appropriate DNS A or CNAME records pointing to your server IP  
- **Docker and docker-compose** installed (for Docker deployment)  
- **Python 3.11+** and **Node.js 18+** (for manual/native deployment)  
- Telegram bot token from [@BotFather](https://t.me/BotFather)  
- Basic familiarity with Linux terminal and networking  

---

## Quick Start — Docker VPS Deployment

This is the recommended deployment method for VPS users.

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/tgmail.git
cd tgmail
```

### 2. Prepare environment variables

Create a .env file in the root project directory:

```.env
TELEGRAM_BOT_TOKEN=1234567890:ABCDEFGHIJKLMNOPQRSTUVWXYZ
POSTGRES_PASSWORD=yourStrongPassword
FERNET_KEY=your_fernet_key_base64_string
TZ=Europe/Athens
DATABASE_URL=postgresql+asyncpg://tgmail:<POSTGRES_PASSWORD>@postgres:5432/tgmail
```

Notes:

    You can generate a Fernet key in Python:
    ```python
    from cryptography.fernet import Fernet
    print(Fernet.generate_key().decode())
    ```
    Replace <POSTGRES_PASSWORD> in DATABASE_URL with your actual password

