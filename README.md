# Azure App Services HandsOn

<img width="960" alt="Image" src="https://github.com/user-attachments/assets/882aea92-fc25-4e54-9ec1-d7da3c9c67dc" />

<br>

This project automates the deployment process using GitHub Actions for continuous integration and continuous deployment (CI/CD). The Docker image is built and pushed to **Azure Container Registry (ACR)**, from where it is deployed to **Azure App Service**.

 This diagram visually explains the flow from GitHub to Azure App Service using Docker containers stored in Azure Container Registry.

## 📊 Architecture Overview

The architecture follows a streamlined CI/CD pipeline:

1. **GitHub**: Hosts the source code.
2. **GitHub Actions**: Automates the building and pushing of Docker images.
3. **Azure Container Registry (ACR)**: Stores the Docker images.
4. **Azure App Service**: Pulls the images from ACR and deploys them.

### Architecture Flow:
- **Code Push** ➡️ GitHub triggers GitHub Actions.
- **Build & Push Docker Image** ➡️ Docker image is built and pushed to ACR.
- **Deploy** ➡️ Azure App Service pulls the latest image from ACR and deploys it.

### CI/CD Workflow Steps:

| Step                       | Description                                                                 |
| -------------------------- | --------------------------------------------------------------------------- |
| **1. Code Push**            | Code pushed to the `master` branch triggers the CI/CD workflow in GitHub Actions. |
| **2. Build Docker Image**   | GitHub Actions builds a Docker image from the codebase.                     |
| **3. Push to ACR**          | The built image is pushed to **Azure Container Registry** for storage.      |
| **4. Deploy to App Service**| Azure App Service pulls the image from ACR and redeploys the app.           |
| **5. Build Summary**        | GitHub Actions generates a summary for the completed build and deployment.  |

### 🔗 Component Overview

| Component                    | Description                                                                                          |
| ---------------------------- | ---------------------------------------------------------------------------------------------------- |
| **GitHub**                    | The source control platform where code is stored and updated.                                        |
| **GitHub Actions**            | The CI/CD pipeline that automates building, pushing, and deploying the Docker image.                 |
| **Docker**                    | A platform used to build the application into a containerized image.                                 |
| **Azure Container Registry**  | Stores the Docker image for future deployments.                                                      |
| **Azure App Service**         | Hosts the application and pulls the latest Docker image from ACR for deployment.                     |

## ⚙️ Deployment Workflow

1. **Code Push**: When you push code to the `master` branch, the CI/CD workflow is triggered.
2. **GitHub Actions**: Automatically builds the Docker image and pushes it to ACR.
3. **Azure App Service**: Fetches the latest image from ACR and deploys it.

### 🚀 Key Benefits of this CI/CD Setup:

- **Automated Deployment**: No need for manual deployment, every push to `master` results in an automated build and deployment.
- **Scalable**: With Azure App Service, you can easily scale your app without changing the pipeline.
- **Versioned Docker Images**: Each build is tagged with a unique Git commit SHA to maintain versioning.
- **Containerized Workflow**: Leveraging Docker ensures consistency between development and production environments.

## 👨‍💻 How to Use

1. **Clone the repository** and push your code changes to the `master` branch.
2. The GitHub Actions pipeline will automatically build and push the Docker image to ACR.
3. Azure App Service will pull the latest image and redeploy the app.
4. You can view the build summary in the GitHub Actions page for every commit.

## 1️⃣ Create Azure App Service

Follow these steps to create **Azure App Service** from the Azure GUI:

1. **Log in to Azure Portal**:
   - Go to [Azure Portal](https://portal.azure.com) and log in with your credentials.

2. **Create an App Service**:
   - From the **Azure Portal**, click **Create a resource** on the left sidebar.
   - Search for **App Service** in the **Search Box** and select it.
   - Click **Create** to start the setup.

3. **Fill in the App Service details**:
   - **Subscription**: Select your Azure subscription.
   - **Resource Group**: Choose an existing resource group or create a new one.
   - **Name**: Enter a unique name for your app (e.g., `my-vite-app`).
   - **Publish**: Choose **Code** (since we are deploying code directly from GitHub).
   - **Runtime stack**: Select **Node 18+** (ensure compatibility with Vite).
   - **Region**: Select **Southeast Asia** from the dropdown to deploy in the desired region.

4. **Configure App Service Plan**:
   - Choose **Pricing Plan**: Select an existing plan or create a new one based on your requirements (e.g., B1 - Basic, S1 - Standard).
   - Set the **Operating System** to **Linux**.

5. **Review and Create**:
   - Review your configurations and click **Create**.
   - Wait a few minutes for the deployment to complete.


<br>

![Image](https://github.com/user-attachments/assets/9d3d59f9-04ff-471b-b868-70cfc3713c13)

![Image](https://github.com/user-attachments/assets/74797f0c-a843-4fb4-84cb-272335a14747)

![Image](https://github.com/user-attachments/assets/b2364a16-c4a3-4c0a-a928-cfdbfaa863b4)

---

### 2️⃣ Set Up Azure Container Registry (ACR)

Follow these steps to create **Azure Container Registry** from the Azure GUI:

1. **Go to Azure Portal**:
   - From the **Azure Portal**, click **Create a resource**.

2. **Search for Container Registry**:
   - In the **Search Box**, type **Container Registry** and select it.
   - Click **Create** to begin.

3. **Fill in the Container Registry details**:
   - **Subscription**: Select your Azure subscription.
   - **Resource Group**: Use the same resource group as the App Service.
   - **Registry Name**: Enter a name for your registry (e.g., `myacrregistry`).
   - **SKU**: Choose **Basic** for basic needs or **Standard** for more advanced features.
   - **Region**: Select **Southeast Asia** to deploy in the same region as the App Service.

4. **Review and Create**:
   - Review your configurations and click **Create**.
   - Wait for the registry to be provisioned.


<br>

![Image](https://github.com/user-attachments/assets/5ded0cd6-9bd9-4c7b-98d1-8e57bfa07eb0)

![Image](https://github.com/user-attachments/assets/b62b20f7-ec97-4fc7-99a0-23ad917a7667)

![Image](https://github.com/user-attachments/assets/cd4214ae-83ea-4e46-b1d1-d0e0d730b4ce)

![Image](https://github.com/user-attachments/assets/e168f329-85e9-44b1-b4be-29cc4fb4812b)

![Image](https://github.com/user-attachments/assets/3f1650eb-68b7-42fc-82d1-8f51497c72f1)

## 🛠️ GitHub Actions Setup And Info

Yaml file can be found in the ".github" directory in the source code. This workflow automates the process of building and pushing the Docker image to **Azure Container Registry (ACR)**, followed by deploying the image to **Azure App Service**. Below is a breakdown of the steps involved in the **App Build and Push to Azure Container Registry** GitHub Action:

#### Steps Breakdown

1. **Checkout**:
   - The first step uses the `actions/checkout@v4` action to clone the GitHub repository, ensuring that the latest code is pulled into the workflow.

2. **Login to Azure Container Registry (ACR)**:
   - The `azure/docker-login@v2` action is used to log in to Azure Container Registry with credentials stored securely in GitHub Secrets. The secrets include:
     - `AZCR_REGISTRY_SERVER`: The Azure Container Registry server URL.
     - `AZCR_USERNAME`: The username for the Azure Container Registry login.
     - `AZCR_PASSWORD`: The password or token for authenticating with the registry.

3. **Build and Push Docker Image**:
   - The `docker build` command is used to build the Docker image from the Dockerfile located in the repository. The image is tagged using the commit hash (`github.sha`) to ensure unique identification of the image.
   - The `docker push` command pushes the built image to the Azure Container Registry using the tagged image name: `${{secrets.AZCR_REGISTRY_SERVER}}/${{secrets.AZCR_REPO_NAME}}:${{ github.sha }}`.

4. **Deploy to Azure App Service**:
   - The `azure/webapps-deploy@v2` action is used to deploy the Docker image from the Azure Container Registry to **Azure App Service**. The `publish-profile` is stored as a GitHub secret (`AZURE_APP_SERVICES_PUBLISH_PROFILE`), and it provides the credentials needed for deployment.
   - The action deploys the Docker image tagged with the commit hash to the Azure App Service defined by the secret `AZAS_APP_NAME`.

<br>

![Image](https://github.com/user-attachments/assets/ceed883d-839f-4043-a9c5-fcb6afdfc9bb)

### Configure Azure Publish Profile in GitHub Secrets

1. **Download the Azure publish profile** from **Azure App Service** → **Deployment Center**.
   - In your Azure App Service, go to the **Deployment Center** tab.
   - Under **Publish Profile**, click **Download Publish Profile**.
   
2. **Go to GitHub** → **Settings** → **Secrets** → **New Repository Secret**.
   - Name the secret `AZURE_WEBAPP_PUBLISH_PROFILE` and paste the contents of the publish profile.

---

## Proof Of Work

![Image](https://github.com/user-attachments/assets/e9366e01-4388-42fe-8f8a-ff38483f4036)
