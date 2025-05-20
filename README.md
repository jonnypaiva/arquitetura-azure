# 🏗️ Componentes de Arquitetura do Azure + Como Criar uma Arquitetura Básica

## 📘 Introdução

Projetar uma arquitetura na nuvem exige o entendimento dos principais componentes oferecidos pelo Azure. Esses elementos são usados para construir desde uma simples aplicação web até sistemas distribuídos de grande escala.

---

## 🧩 Principais Componentes da Arquitetura do Azure

### 1. 🔷 **Resource Group**
Unidade lógica que agrupa recursos relacionados para gerenciamento conjunto (como VMs, bancos de dados, redes, etc.).

### 2. 🧱 **Virtual Network (VNet)**
Rede privada no Azure que permite comunicação entre recursos, com sub-redes, segurança e conectividade híbrida.

### 3. 💻 **Virtual Machines (VMs)**
Servidores configuráveis na nuvem. Úteis para aplicações legadas ou que exigem controle total do ambiente.

### 4. 🌐 **Load Balancer**
Distribui o tráfego de rede entre várias instâncias para garantir alta disponibilidade.

### 5. 🗃️ **Azure Storage**
Armazenamento de blobs, arquivos, discos e filas. Ideal para dados estruturados e não estruturados.

### 6. 📡 **Application Gateway / Azure Front Door**
Controla o tráfego HTTP com roteamento inteligente, balanceamento global e segurança via WAF.

### 7. 🧠 **Azure App Service**
Hospedagem de aplicações web e APIs sem se preocupar com infraestrutura.

### 8. 🛡️ **Network Security Group (NSG)**
Firewall que controla o tráfego de entrada e saída de redes e sub-redes.

### 9. 📊 **Azure Monitor + Log Analytics**
Ferramentas para coleta, análise e visualização de métricas e logs.

---

## 🏗️ Criando uma Arquitetura Básica no Azure

Vamos montar uma arquitetura simples para uma aplicação web, seguindo boas práticas.

### 📌 Cenário: Aplicação Web com banco de dados, alta disponibilidade e segurança.

---

### 🔹 Etapas:

#### 1. **Criar um Resource Group**
```bash
az group create --name rg-webapp --location eastus

#### 2. Criar uma VNet com Sub-rede
bash
Copy
Edit
az network vnet create \
  --name vnet-webapp \
  --resource-group rg-webapp \
  --subnet-name subnet-app

#### 3. Criar uma VM (se necessário)
bash
Copy
Edit
az vm create \
  --resource-group rg-webapp \
  --name vm-web \
  --vnet-name vnet-webapp \
  --subnet subnet-app \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys

####4. Criar um App Service (alternativa sem VM)
bash
Copy
Edit
az appservice plan create \
  --name appplan \
  --resource-group rg-webapp \
  --sku B1 \
  --is-linux

az webapp create \
  --resource-group rg-webapp \
  --plan appplan \
  --name nome-da-sua-app \
  --runtime "NODE|18-lts"

####5. Criar um Banco de Dados (ex: Azure SQL)
bash
Copy
Edit
az sql server create \
  --name sqlserverwebapp \
  --resource-group rg-webapp \
  --location eastus \
  --admin-user azureuser \
  --admin-password SenhaSegura123!

az sql db create \
  --resource-group rg-webapp \
  --server sqlserverwebapp \
  --name dbwebapp \
  --service-objective S0

6. Adicionar Regras de NSG (opcional)
Controle o acesso por IP e portas específicas.

🗺️ Diagrama de Arquitetura Básica
mermaid
Copy
Edit
graph TD
    A[Usuário] --> B[App Web no App Service]
    B --> C[Azure SQL Database]
    B --> D[Azure Storage]
    B --> E[Azure Monitor]
    B --> F[Virtual Network + Subnet]
    F --> G[Network Security Group]
