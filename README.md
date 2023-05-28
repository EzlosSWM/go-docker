# Go, Postgres & Docker Boilerplate

## Commands 
```Bash
# Enters bash
docker compose run --service-ports web bash

# Builds container 
docker compose build

# Runs 
docker compose run
```

## Dockerfile 
[Air](https://github.com/divrhino/divrhino-trivia/blob/main/.air.toml) enables hot reloading
```Docker 
FROM golang:1.20

WORKDIR /usr/src/app

RUN go install github.com/cosmtrek/air@latest

COPY . .

RUN go mod tidy
```


## Docker Compose 
web service: API (backend)
db service: Database
```Docker 
version: '3.8'

services: 
  web: 
    build: .
    env_file:
      - .env
    ports: 
      - 3000:3000
    volumes: 
      - .:/usr/src/app
    command: air cmd/main.go - b 0.0.0.0
  db: 
    image: postgres:alpine
    environment:
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_NAME=${DB_NAME}
    ports: 
      - 5432:5432
    volumes:
      - postgres-db:/var/lib/postgresql/data

volumes:
  postgres-db:
```