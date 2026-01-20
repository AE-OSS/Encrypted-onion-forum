# 🧅 Encrypted Onion Forum

> **Lightweight, login-protected forum for Tor.**  
> Fast to deploy, minimal dependencies, and ready as a v3 onion service out of the box.

---

## ✨ Features

*   **🔒 Full Encryption**: Usernames are encrypted at rest; passwords are hashed. Never stored in plaintext.
*   **⚡ Zero JavaScript**: Default UI is server-rendered, ensuring maximum security and privacy.
*   **🛡️ Secure & Safe**: Strict CSP, CSRF protection, and sanitized Markdown rendering.
*   **📦 Tiny Footprint**: SQLite storage (single file) with minimal resource usage.
*   **🚀 Instant Deploy**: One-step Docker setup with a built-in Tor hidden service.
*   **👥 Admin Managed**: Login required by default. Admins manage user access.

---

## 🚀 Quick Start

Get your forum up and running in seconds.

### 🐧 Linux / Unix / macOS

```bash
mkdir forum
cd forum
wget https://code.xeayu.com/Xeayu/Encrypted-onion-forum/raw/branch/main/docker-compose.yml
docker compose up -d
```

### 🪟 Windows (PowerShell)

```powershell
mkdir forum
cd forum
wget https://code.xeayu.com/Xeayu/Encrypted-onion-forum/raw/branch/main/docker-compose.yml -OutFile docker-compose.yml
docker compose up -d
```

### 🔗 Get Your Onion Address

Wait about **60 seconds** for Tor to bootstrap, then retrieve your hidden service address:

```bash
docker compose exec tor sh -lc 'cat /var/lib/tor/hidden_service/hostname'
```

*Visit this address in Tor Browser to access your forum.*

---

## 🛠️ Configuration

You can configure the forum by setting environment variables in your `docker-compose.yml` or a `.env` file.

| Variable | Description | Default |
| :--- | :--- | :--- |
| `REQUIRE_LOGIN` | Set to `0` to disable the login requirement. | `1` (Enabled) |
| `ADMIN_USER` | Pre-create an admin username on first boot. | *(None)* |
| `ADMIN_PASS` | Pre-create an admin password on first boot. | *(None)* |
| `SECRET_KEY` | Override the auto-generated 64-hex secret key. | *(Auto-generated)* |
| `FORUM_DB_PATH` | Path to the SQLite database file. | `/data/forum.db` |
| `PORT` | Internal web server port. | `8080` |

---

## 👮 Administration

1.  **First Login**: The first user to visit `/login` and create an account effectively becomes the **Admin**.
2.  **Manage Users**: Once logged in as an admin, navigate to `/admin/users` to add new users or grant admin privileges.
3.  **Upcoming Features**: User deletion and profile editing (username/password) are coming soon.

---

## 💾 Data & Persistence

Your data is persistent and secure.

*   **Forum Data**: The SQLite database and the app's secret key are stored in the `forum_data` volume (mounted to `/data`).
*   **Tor Keys**: The hidden service private key and hostname are stored in the `tor_data` volume.
*   **Secret Key**: A `SECRET_KEY` is auto-generated in `/data/secret_key` on first run if not provided via environment variables.

### How to Reset

To completely **wipe** the forum and start fresh (including a new onion address):

```bash
docker compose down
docker volume rm onion_forum_data 
docker volume rm onion_tor_data
```

---

## 🩺 Health Check

The application exposes a health check endpoint:

```bash
curl http://localhost:8080/healthz
```

---

## 🔧 Troubleshooting

**Tor not ready?**  
Check the logs to see if Tor has finished bootstrapping:
```bash
docker compose logs -f tor
```
*Wait for "Bootstrapped 100%"*

**Forgot Onion Address?**  
Run:
```bash
docker compose exec tor sh -lc 'cat /var/lib/tor/hidden_service/hostname'
```

**Port Conflict?**  
If port `8080` is in use, modify the mapping in `docker-compose.yml` (e.g., `8090:8080`).

---

## 📜 License

See the [LICENSE](LICENSE) file in this repository.
