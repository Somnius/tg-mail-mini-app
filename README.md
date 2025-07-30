# tgmail Mini App

A Telegram Mini App providing an embedded email client experience inside Telegram using IMAP/SMTP protocols.  
Built with **FastAPI** (backend), **React** (frontend), and the **Telegram Bot API**, deployed via Docker and Traefik.

<p align="center">
  <img src="tg-email_logo.png" alt="tgmail Logo" width="200" />
</p>

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

### 3. Configure DNS

Set DNS A records for your domain, e.g., `tgmail.example.com` to point to your server IP.

### 4. Setup Traefik (already included)

- Your Traefik container handles HTTP->HTTPS redirects and automatic Let’s Encrypt certificates.
- Make sure ports 80 and 443 are open on your server firewall.

### 5. Build and start backend and frontend stacks

```bash
cd compose
docker-compose -f backend.yaml build
docker-compose -f backend.yaml up -d

docker-compose -f frontend.yaml build
docker-compose -f frontend.yaml up -d
```

### 6. Initialize database schema (once)

```bash
docker exec -it <backend_container_id> python /app/app/db_init.py
```
don't forget to swap `<backend_container_id>` with yours

### 7. Set Telegram bot webhook

Replace `<BOT_TOKEN>` and `<DOMAIN>` accordingly:
```bash
curl -F "url=https://tgmail.example.com/webhook/<BOT_TOKEN>" https://api.telegram.org/bot<BOT_TOKEN>/setWebhook
```

### 8. Setup bot domain in BotFather

Send `/setdomain` command in `BotFather` for your bot, enter your domain `tgmail.example.com`.

### 9. Launch the Mini App

- Open Telegram mobile app
- Chat with your bot
- Tap on the custom "Open tgmail" WebApp button (set via inline keyboard)
- The Mini App will load inside Telegram with Telegram login enabled

### 10. Test IMAP connection

- Enter your IMAP server, email, and password in the form
- Submit to test connection to your mailbox

## Step-by-Step Manual Deployment (Native / Home Server)

If you prefer not to use Docker or have your own server with a static IP, you can deploy the app manually.

### Requirements

- Python 3.11+ installed with `pip`  
- Node.js 18+ installed with `npm` or `yarn`  
- PostgreSQL server running and accessible  
- A reverse proxy (e.g., Nginx or Caddy) for HTTPS termination  
- Domain name configured to point to your server  

---

### Backend

1. **Create and activate a Python virtual environment:**

    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```

2. **Install dependencies:**

    ```bash
    pip install fastapi uvicorn python-telegram-bot[async] asyncpg sqlalchemy[asyncio] cryptography aioimaplib pydantic
    ```

3. **Configure environment variables** (export or use a `.env` file):

    ```bash
    export TELEGRAM_BOT_TOKEN="your_bot_token"
    export POSTGRES_PASSWORD="your_password"
    export FERNET_KEY="your_fernet_key"
    export DATABASE_URL="postgresql+asyncpg://tgmail:your_password@localhost:5432/tgmail"
    ```

4. **Initialize the database schema:**

    ```bash
    python app/db_init.py
    ```

5. **Run the backend server:**

    ```bash
    uvicorn app.main:app --host 0.0.0.0 --port 8000
    ```

---

### Frontend

1. **Navigate to the frontend folder:**

    ```bash
    cd frontend
    ```

2. **Install dependencies:**

    ```bash
    npm install
    ```

3. **Build the React app:**

    ```bash
    npm run build
    ```

4. **Serve static files** via Nginx, Caddy, or any web server on your domain.

---

### Reverse Proxy Setup

Example **Nginx** server block (simplified):

```nginx
server {
    listen 80;
    server_name tgmail.example.com;

    location / {
        proxy_pass http://localhost:8000;  # Backend API
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $remote_addr;
    }

    location /static/ {
        root /path/to/frontend/build;
    }

    # Redirect HTTP to HTTPS or set SSL configuration here
}
```

### Telegram Setup

- Follow the same BotFather instructions and webhook setup as for Docker setup, updating URLs accordingly.

---

### Telegram Bot Setup

- Create your Telegram bot with [@BotFather](https://t.me/BotFather)  
- Save the bot token securely  
- Use `/setdomain` to set your domain (where frontend is hosted)  
- Use `/setwebhook` to set your backend webhook URL  
- Add an inline keyboard button with a WebApp URL for launching the Mini App  

Example keyboard JSON:

```json
{
  "inline_keyboard": [
    [
      {
        "text": "Open tgmail",
        "web_app": { "url": "https://tgmail.example.com" }
      }
    ]
  ]
}
```

Send this JSON via the `sendMessage` API or from your bot code.

---

### Configuration

- `.env` or environment variables control sensitive settings (bot token, database credentials, Fernet key)  
- Database URL format: `postgresql+asyncpg://user:password@host:port/dbname`  
- Fernet key must be securely generated and kept secret for encrypting mailbox passwords  
- Timezone can be set with `TZ` variable (e.g., `Europe/Athens`)  

---

### Development

- Backend code lives under `/backend/app`  
- Frontend React code lives under `/frontend/src`  
- Use Docker Compose files under `/compose` for local or VPS deployments  
- Use `uvicorn` to run backend in development mode, `npm run dev` for frontend  

---

## License

This project is licensed under the MIT License.

---

## Credits and Attributions

- Developed by Eleftherios Iliadis  
- Uses open-source libraries and tools including:  
  - [FastAPI](https://fastapi.tiangolo.com/)  
  - [Python-Telegram-Bot](https://github.com/python-telegram-bot/python-telegram-bot)  
  - [React](https://reactjs.org/)  
  - [Traefik](https://traefik.io/)  
  - [PostgreSQL](https://www.postgresql.org/)  
  - [aioimaplib](https://github.com/fox-it/aioimaplib)  
  - and others — see `requirements.txt` and `package.json`

---

## Contributing

Contributions, bug reports, and feature requests are welcome!  
Please open an issue or a pull request on GitHub.

---

## Contact

Eleftherios Iliadis — lefteros@outlook.com / Lefteris @SomniusX
