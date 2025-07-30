# tgmail Mini App

A Telegram Mini App providing email client features inside Telegram using IMAP/SMTP —  
built with FastAPI, React, and Telegram Bot API.

---

## Features

- Telegram WebApp login authentication  
- IMAP connection testing with user credentials  
- Secure backend with encrypted credential storage (planned)  
- Modular, Docker-compose based deployment  
- Reverse proxy and HTTPS via Traefik  
- Open-source and extensible  

---

## Getting Started

### Requirements

- Docker & docker-compose  
- VPS with public IP and domain configured (e.g. `tgmail.srv-box.com`)  
- Telegram bot token from [BotFather](https://t.me/BotFather)  

### Deployment

1. Clone the repo  
2. Configure `.env` with your secrets and tokens  
3. Deploy backend and frontend stacks with `docker-compose`  
4. Setup DNS and Traefik to route domains  
5. Register bot domain with BotFather (`/setdomain`)  
6. Launch the Mini App from Telegram  

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Credits and Attributions

- Developed by Eleftherios Iliadis  
- Built with open-source libraries and tools:
  - [FastAPI](https://fastapi.tiangolo.com/)  
  - [Python-Telegram-Bot](https://github.com/python-telegram-bot/python-telegram-bot)  
  - [React](https://reactjs.org/)  
  - [Traefik](https://traefik.io/)  
  - [PostgreSQL](https://www.postgresql.org/)  
  - [aioimaplib](https://github.com/fox-it/aioimaplib)  
  - Others, see `requirements.txt` and `package.json`  

---

## Contributing

Contributions and issues are welcome! Please open an issue or pull request on GitHub.

---

## Contact

Eleftherios Iliadis — [your email or website]  
