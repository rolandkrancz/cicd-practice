# FastAPI Azure CI/CD Demo

A simple **FastAPI** web application, containerized with **Docker**, and deployed to **Azure Web App for Containers** via **GitHub Actions CI/CD**.

---

## 🏗️ Project Structure

```
CICD-PRACTICE/
│
├── app/
│   ├── main.py           # FastAPI application
│   └── __init__.py
│
├── tests/
│   └── test_main.py      # Unit tests
│
├── requirements.txt      # Python dependencies
├── Dockerfile            # Docker container definition
└── .github/
    └── workflows/
        └── deploy.yml    # GitHub Actions workflow
```

---

## 🚀 Features

* FastAPI web server
* `/` endpoint: Returns a hello message
* `/health` endpoint: Returns status `"healthy"`
* Unit testing with **pytest**
* Containerized with **Docker**
* CI/CD pipeline:

  * Build & test
  * Push Docker image to **Azure Container Registry (ACR)**
  * Deploy automatically to **Azure Web App for Containers**

---

## ⚡ Requirements

* Python 3.11+
* Docker
* GitHub repository
* Azure subscription
* Azure CLI
* GitHub Secrets (see below)

---

## 🔑 GitHub Secrets

You need the following secrets in your repository:

| Secret Name                             | Description                                      |
| --------------------------------------- | ------------------------------------------------ |
| `AZURE_CLIENT_ID`                       | Service principal appId                          |
| `AZURE_CLIENT_SECRET`                   | Service principal password                       |
| `AZURE_TENANT_ID`                       | Azure tenant ID                                  |
| `AZURE_SUBSCRIPTION_ID`                 | Azure subscription ID                            |
| `AZURE_CONTAINER_REGISTRY`              | Azure Container Registry name                    |
| `AZURE_CONTAINER_REGISTRY_LOGIN_SERVER` | ACR login server (e.g., `myregistry.azurecr.io`) |
| `AZURE_WEBAPP_NAME`                     | Azure Web App name                               |

---

## 🐳 Docker

### Build locally

```bash
docker build -t fastapi-demo .
docker run -p 8000:8000 fastapi-demo
```

Visit `http://localhost:8000` to test.

---

## ✅ Run Tests

```bash
pip install -r requirements.txt
pip install pytest
pytest
```

---

## ☁️ Azure Setup

### 1. Create Resource Group

```bash
az group create --name <resource-group-name> --location <location>
```

### 2. Create Azure Container Registry

```bash
az acr create --resource-group <resource-group-name> --name fastapidemoregistry --sku Basic
```

### 3. Create Web App for Containers

```bash
az appservice plan create --name <plan-name> --resource-group <resource-group-name> --sku B1 --is-linux

az webapp create \
  --resource-group <resource-group-name> \
  --plan <plan-name> \
  --name <app-name> \
  --deployment-container-image-name mcr.microsoft.com/azuredocs/aci-helloworld
```

> The GitHub Actions workflow will update the container automatically.

---

## ⚙️ GitHub Actions CI/CD

* Triggered on push to `main` branch
* Steps:

  1. Checkout code
  2. Set up Python
  3. Install dependencies
  4. Run unit tests
  5. Login to Azure
  6. Build and push Docker image to ACR
  7. Deploy container to Azure Web App

---

## 🌐 Access App

After deployment, your app will be available at:

```
https://<AZURE_WEBAPP_NAME>.azurewebsites.net
```

Example endpoints:

* `GET /` → `{"message": "Hello from root!"}`
* `GET /health` → `{"status": "healthy"}`

---

## 💡 Notes

* Make sure the **PORT environment variable is set to 8000** in Web App Configuration.
* Use a **service principal** with least privileges for security.
* You can extend this setup with:

  * Staging and production environments
  * Blue/green deployment
  * Automated security scans (Trivy, CodeQL)

---

## 📝 References

* [FastAPI Documentation](https://fastapi.tiangolo.com/)
* [Azure Container Registry](https://learn.microsoft.com/en-us/azure/container-registry/)
* [Azure Web App for Containers](https://learn.microsoft.com/en-us/azure/app-service/containers/)
* [GitHub Actions for Azure](https://github.com/Azure/actions)
