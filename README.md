# 📦 Projeto Docker - Lista de Tarefas

## 🐳 Instalação do Docker

### 🐧 Linux (Ubuntu / Linux Mint)

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg

sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu noble stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Testar:

```bash
sudo docker run hello-world
```

---

### 🪟 Windows

1. Baixar o Docker Desktop: https://www.docker.com/products/docker-desktop/
2. Instalar normalmente
3. Ativar o WSL (caso solicitado)
4. Reiniciar o computador

Testar no terminal:

```bash
docker run hello-world
```

---

### 🍎 macOS

1. Baixar o Docker Desktop: https://www.docker.com/products/docker-desktop/
2. Instalar e abrir o aplicativo
3. Aguardar o Docker iniciar

Testar:

```bash
docker run hello-world
```

---

## 🚀 Execução do Projeto

### 1. Dockerfile do Backend

```Dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

---

### 2. Dockerfile do Frontend

```Dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host"]
```

---

### 3. Criar rede Docker

```bash
docker network create tarefas-network
```

---

### 4. Subir banco PostgreSQL

```bash
docker run -d \
  --name postgres \
  --network tarefas-network \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=tarefasdb \
  -p 5432:5432 \
  postgres
```

---

### 5. Build das imagens

```bash
docker build -t backend ./backend
docker build -t frontend ./frontend
```

---

### 6. Subir backend

```bash
docker run -d \
  --name backend \
  --network tarefas-network \
  -p 3001:3000 \
  backend
```

---

### 7. Subir frontend

```bash
docker run -d \
  --name frontend \
  --network tarefas-network \
  -p 3000:5173 \
  -e VITE_API_URL=http://localhost:3001 \
  frontend
```

---

### 🌐 Acessar aplicação

```
http://localhost:3000
```


---

## 🧪 Testes e Debug

### 🔍 Ver containers ativos

```bash
docker ps
```

---

### 🔍 Ver todos os containers

```bash
docker ps -a
```

---

### 📜 Ver logs

```bash
docker logs backend
docker logs frontend
docker logs postgres
```

---

### ❌ Remover container

```bash
docker rm -f backend
docker rm -f frontend
docker rm -f postgres
```

---

### 🧪 Testar backend

```bash
curl http://localhost:3001/db-status
```