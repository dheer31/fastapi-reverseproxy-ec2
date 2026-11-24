# FastAPI + MySQL + Docker Compose

A minimal, production-style FastAPI project with a MySQL backend, fully containerized
with Docker and Docker Compose. Database tables are created automatically on
application startup if they don't already exist (`Base.metadata.create_all`), with
retry logic so the app waits gracefully for MySQL to become ready.

## Project structure

```
.
├── app/
│   ├── main.py            # FastAPI app, startup DB init, health routes
│   ├── database.py        # SQLAlchemy engine/session config (env-driven)
│   ├── models.py          # ORM models (Item)
│   ├── schemas.py         # Pydantic request/response schemas
│   ├── crud.py             # DB access functions
│   └── routers/
│       └── items.py       # /items CRUD endpoints
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .env.example
└── .env                    # already filled with default local dev values
```
<img width="1024" height="700" alt="image" src="https://github.com/user-attachments/assets/561e1609-27d0-40d6-8eba-44307c5a4aab" />


upstream backend {
    server 15.207.100.65:8000;
}

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://backend;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

sudo rm /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
