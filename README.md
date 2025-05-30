## 🎯 Objetivos do Projeto

- ✅ Aplicar os conceitos de **containerização**, **CI/CD** e **orquestração de containers** usando Docker, Azure Container Registry (ACR) e Azure Kubernetes Service (AKS).
- ✅ Documentar o processo técnico de forma clara e estruturada.
- ✅ Utilizar o GitHub como ferramenta de versionamento e compartilhamento da documentação do projeto.

---

## 🖥️ Passos Realizados

A seguir, os passos executados para a realização deste projeto:

---

### 🔹 Construção da Imagem Docker

- Criação do `Dockerfile` com base na imagem `nginx:alpine`.
- Cópia do arquivo `index.html` (html simples para ilustrar o serviço) para o container Nginx.
- Comando utilizado para build:
  ```
  docker build -t techblog/land:latest .
  ```
- Teste local da imagem:
  ```
  docker run -d -p 80:80 techblog/land:latest
  ```

---

### 🔹 Autenticação no Azure e Push para o ACR

- Login no Azure:
  ```
  az login
  ```
- Login no Azure Container Registry:
  ```
  az acr login --name rgstryphforcloud
  ```
- Tag da imagem local para o ACR:
  ```
  docker tag techblog/land:latest rgstryphforcloud.azurecr.io/techblog-land:latest
  ```
- Push da imagem para o ACR:
  ```
  docker push rgstryphforcloud.azurecr.io/techblog-land:latest
  ```

---

### 🔹 Configuração do Cluster Kubernetes (AKS)

- Utilização do cluster criado para o lab:
  - Nome: `aks-dev-westus3-0ph1-dio-labph0001`
  - Resource Group: `AKS01`
- Recuperação das credenciais para o kubectl:
  ```
  az aks get-credentials --resource-group AKS01 --name aks-dev-westus3-0ph1-dio-labph0001
  ```

---

### 🔹Criação dos Manifests Kubernetes

- Arquivo `deployment.yaml`:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: landing-page
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: landing-page
    template:
      metadata:
        labels:
          app: landing-page
      spec:
        containers:
        - name: landing-page
          image: rgstryphforcloud.azurecr.io/techblog-land:latest
          ports:
          - containerPort: 80
  ```
- Arquivo `service.yaml`:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: landing-page
  spec:
    type: LoadBalancer
    ports:
    - port: 80
      targetPort: 80
      protocol: TCP
    selector:
      app: landing-page
  ```

---

### 🔹Aplicação dos Manifests no Cluster

- Aplicação do Deployment:
  ```
  kubectl apply -f deployment.yaml
  ```
- Aplicação do Service:
  ```
  kubectl apply -f service.yaml
  ```

---

### 🔹 Verificação do Service Exposto

- Checagem do serviço:
  ```
  kubectl get svc landing-page
  ```
  

---

Imagens úteis:
![image](https://github.com/user-attachments/assets/753ba795-657c-415f-a32c-c47288ee1686)

![image](https://github.com/user-attachments/assets/9e7b3220-2fc3-4e43-973c-94debd254a8b)


## 🧾 Notas Finais

Este README é um modelo padrão reutilizável para projetos desenvolvidos em bootcamps da DIO.  
Adapto conforme o desafio proposto, mantendo a estrutura clara e objetiva.

---
