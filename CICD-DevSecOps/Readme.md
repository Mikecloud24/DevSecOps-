# Note: This is the refactored dev/prod code based DevSecOps with best practices. I am going to segment all the services and apply them individually, but they can also be grouped together in one file and apply at once! 

# mysql-credentials secrets and PVC file
- This file creates a Secret to avoid hard-coding passwords, and a PersistentVolumeClaim for durable storage
- apply the file using kubectl apply -f mysql-cred.yaml

# MySQL Deployment
- The file uses Secret for the password, add resource requests/limits, health probes, non-root securityContext, and mount the PVC
- apply it

# MySQL Service
- Apply the service into the default namespace using kubectl apply -f mysql-service.yaml -n default

# Bankapp ConfigMap and Secret
- This file Separate configuration from code. Use a Secret for DB credentials and ConfigMap for the URL.
- appy it

# Bankapp Deployment
- This file implemented the following: non-root user, resource quotas, liveness/readiness HTTP probes, service account with Azure AD Pod Identity annotation, and environment injection from ConfigMap/Secret.
- apply it

# Bankapp Service
- apply it

# Bankapp Horizontal Pod Autoscaler
- This file when applied, Automatically scale the Java app between 2 and 10 replicas based on CPU usage.

# NetworkPolicy
- Lock down traffic so only the Java app can talk to MySQL (Database layer), and only the LoadBalancer can reach the bankapp pods(Frontend)


# The Pipeline key Enhancements:

- Centralized pipeline variables (ACR name, image tag, subscriptions).


- Multi-stage flow 4 stages:

1. Build, test & package with Maven

2. Trivy filesystem scan + SonarQube analysis

3. Docker build & push (single buildAndPush task) + Trivy image scan

4. Deploy to AKS via KubernetesManifest@1, injecting the new images and pulling secrets from Key Vault


- Automatic failure on HIGH/CRITICAL vulnerabilities in both FS and image scans

- Secrets kept in Azure Key Vault (mysql-root-password, DB creds)

- Applies all k8s manifests (Secrets, PVC, Deployments, Services, HPA, NetworkPolicy) with one task


# The Project can further be enhanceed by integrating more features like:

- Adding manual approvals or gates before the Deploy stage for production.

- Hook up pipeline notifications (Teams, Email) on failure or success.

- Parameterize environments (dev/test/prod) with variable groups or runtime parameters.

- Adding environment-specific overlays, or integrate Helm/Flux for GitOps.


# Feel free to tweak it and let me know your thoughts!
