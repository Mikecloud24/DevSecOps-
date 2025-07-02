
# Project overview:

. This project is a ‘DevSecOps’ java base Banking web application with database, maven build, Dockerized and deployed to Azure Kubernetes Service (AKS)- Demo!

# Infrastructure set up:
-	Azure Account with appropriate permissions
-	Azure DevOps Organization account (ADO)
-	Create a service principal (SP) and give it a contributor role in Azure Cloud
-	Provision Service connections for the following services in ADO (aks, acr, docker and SonarQube)
-	AKS-cluster
-	ACR--- Azure Container Registry
-	Setup your project in ADO
-	Clone Repo
-	Set up a Self-hosted agent or an Azure-managed agent   # with self-hosted agent, you can install and configure other tools necessary for the project.

# AWS side:
1.	Agent Server
2.	SonarQube Server



# Login to SonarQube server to obtain the token for ADO service connection. SonarQube server -- Administrator -- users -- token name -- Generate token. 

![image](https://github.com/user-attachments/assets/74134a5e-9160-4486-abbf-5fc658e78f7c) 


# The first three stages manual run were successful, please see the logs. 

![image](https://github.com/user-attachments/assets/5dfcf0fc-3167-4ef4-b4dc-daee632a38e4)


# Self hosted Agent in action (AWS ec2, ubuntu server 24.04 LTS)

- Observing the build process until the Docker build failed.The reason and troubleshooting are below.

- The reason was configuration mismatched. (Maven package) instead (mvn package), corrected and re-run…

![image](https://github.com/user-attachments/assets/bf63ae1f-0fe6-4e0d-9f3d-fa51312e623c)


![image](https://github.com/user-attachments/assets/c3117e6f-7151-451b-b828-b78a0c963bed)


![image](https://github.com/user-attachments/assets/cd460845-f574-4d7c-b66a-bd713bb7de22)

![image](https://github.com/user-attachments/assets/67961499-f50b-4cc9-a570-c5d4f6fe2dbe)


# Successfully deployed to AKS, the blockchain of action…

![image](https://github.com/user-attachments/assets/2d2e5eb1-3bdc-4c38-ae1e-153f71767125)

![image](https://github.com/user-attachments/assets/7ec9eabb-eb02-4b9d-bb03-9a561de7dd5a)


# Image view in acr and Artifacts in ADO

![image](https://github.com/user-attachments/assets/b85f8d1a-30d7-4e3d-8e0e-e693a30c4f8c)


![image](https://github.com/user-attachments/assets/ac576cec-afa7-481f-9b78-4971bbd567d0)


# AKS cluster overview in Azure cloud

![image](https://github.com/user-attachments/assets/576d0cfb-1d4b-413d-b10d-fb8e561d83ec)


# Cluster Services and Ingresses views with the external loadbalancer IP to confirm the app availability 

![image](https://github.com/user-attachments/assets/57e98945-7c64-4f46-a3fe-6aab0af7f003)


# App running…

![image](https://github.com/user-attachments/assets/4399483b-e123-48d1-b585-d2052405df79)


# SonarQube server overview of the bankapp code…

![image](https://github.com/user-attachments/assets/fa564262-b01c-4c8d-b92c-99875efce9c7)


# Conclusion:

The project was successfully executed. Although in nine stages of DevSecOps, it’s a POC demo in a lower environment. 
Production refactored code with best practices in private repo.
Key:
- Centralized pipeline variables with ACR name, image tag, subscriptions etc
- Multi-stage flow ---> the pipeline is refactored into 4 stages:
  1. Build, test and package with Maven
  2. Trivy fs scan and SonarQube Analysis
  3. Docker build and push(single buildAndPush task) and Trivy image scan
  4. Deploy to AKS via KubernetesManifest@1, injecting the new image and pulling secrets from Azure Key Vault
- Automatic failure on HIGH/CRITICAL vulnerabilities in both FS and image scan
- Secrets kept in Azure Key Vault (mysql-root-password, DB creds)
- All Kubernetes manifest (Secrets, PVC, Deployments, Services, HPA, NetworkPolicy) applied with one task
- Pipeline notification hook up with an email on failure or success.
- Environment variables (dev/test/prod)

# Please connect for collaboration. 














