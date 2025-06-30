# docker-app-packaging
## 🧱 Empacotamento de Aplicações com Docker
### 🎯 Objetivo
Containerizar uma aplicação Node.js simples com Docker, aplicar boas práticas de build, gerar a imagem e publicá-la no Docker Hub.

📁 Estrutura do projeto
```
lab-02-docker-app/
├── Dockerfile
├── .dockerignore
├── package.json
├── package-lock.json
├── index.js
├── README.md
```
### 💻 1. Código da aplicação
✅ index.js
```
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (_, res) => {
  res.send('🚀 Hello from Dockerized Node.js App!');
});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});
```
✅ package.json
```
{
  "name": "docker-node-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
✅ package-lock.json
 - ⚠️ Gerado automaticamente ao rodar npm install — inclua no repositório.

### 🐳 2. Docker
✅ Dockerfile
```
# Etapa 1: build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .

# Etapa 2: runtime
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app .

EXPOSE 3000
CMD ["npm", "start"]
```
✅ .dockerignore
```
node_modules
npm-debug.log
Dockerfile
.dockerignore
.git
```
### ▶️ 3. Comandos para execução
🧪 Build local da imagem:
```
docker build -t tse/docker-node-app:latest .
```
▶️ Rodar localmente
```
docker run -p 3000:3000 tse/docker-node-app:latest
```
Acesse via browser:
```
http://localhost:3000
```
### 🚀 4. Publicar no Docker Hub
🔐 Faça login (uma vez)
```
docker login
```
🔼 Push da imagem
```
docker push xpto/docker-node-app:latest
```
