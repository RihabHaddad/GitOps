# GitOps Repository

This repository serves as the single source of truth for the desired state of our Kubernetes cluster. It follows the GitOps principles, where all infrastructure and application deployments are managed declaratively through Git. ArgoCD continuously monitors this repository and automatically reconciles the cluster state with the configurations defined here.

## Contents

* **config/:** (Optional) This directory might contain application-specific configuration files. Currently empty.
* **k8s/:** This directory contains the Kubernetes manifests that define the desired state of our application and related services.
    * `deployment.yaml`: Defines the deployment of the main application. The `image` tag in this file is automatically updated by the Jenkins pipeline.
    * `mongo-deployment.yaml`: Defines the deployment for the MongoDB service.
    * `rbac.yaml`: Contains Role-Based Access Control (RBAC) configurations within the cluster, likely related to Vault or other service permissions.
    * `secret-provider-class.yaml`: Configuration for a Secret Store CSI driver (like HashiCorp Vault Secrets Operator) to fetch secrets from an external provider.
    * `service-accounts.yaml`: Defines Kubernetes Service Accounts used by different deployments (e.g., for Vault access).
* **monitoring/:** Contains configuration files related to monitoring the application and cluster using Prometheus and Grafana.
    * `Grafana-deploy.yaml`: Kubernetes deployment for the Grafana visualization tool.
    * `Grafana-service.yaml`: Kubernetes service to expose the Grafana UI.
    * `Prometheus-config.yml`: Configuration for the Prometheus metrics collection server.
    * `Prometheus-deploy.yml`: Kubernetes deployment for the Prometheus server.
    * `Prometheus-rbac.yaml`: RBAC configurations specific to Prometheus, granting it necessary permissions within the cluster.
    * `Prometheus-service.yml`: Kubernetes service to expose the Prometheus UI.
    * `servicemonitor.yaml`: Defines how Prometheus discovers and scrapes metrics from our application pods.

## Purpose

Any changes made to the Kubernetes manifests in this repository will be automatically applied to the Kubernetes cluster by ArgoCD. This GitOps approach provides:

* **Declarative Configuration:** The desired state of the cluster is defined in Git.
* **Version Control:** All changes to the infrastructure and deployments are tracked in Git.
* **Automation:** ArgoCD automates the synchronization of the cluster with the Git repository.
* **Auditability:** The Git history provides a complete audit log of all changes.
* **Rollbacks:** Reverting to a previous state is as simple as reverting a Git commit.

## Key Technologies Integrated

* **Git:** As the source of truth for the desired cluster state.
* **Kubernetes:** The container orchestration platform where the application and supporting services (like MongoDB) are deployed.
* **ArgoCD:** A GitOps continuous delivery tool for Kubernetes that automates application deployment and lifecycle management.
* **Prometheus:** Configured here to collect and store metrics from the application and cluster components.
* **Grafana:** Deployed here to provide visualization of the metrics collected by Prometheus through dashboards.
* **HashiCorp Vault:** (Integration implied by `secret-provider-class.yaml` and `service-accounts.yaml`) Secrets are likely managed externally using Vault and accessed within the cluster via a Secret Store CSI driver.

## Getting Started (Operators/DevOps)

1.  **Clone this repository:**
    ```bash
    git clone https://github.com/RihabHaddad/GitOps
    cd GitOps
    ```

2.  **Understand the Kubernetes manifests:** Familiarize yourself with the configurations in the `k8s/` and `monitoring/` directories. Pay close attention to the deployment specifications, service definitions, and RBAC configurations.

3.  **Monitor ArgoCD:** Observe ArgoCD's interface to see the synchronization status of the applications and infrastructure managed by this repository. Ensure that the `nodejs-app` application (defined in `k8s/deployment.yaml`) is healthy and synced.

4.  **Make changes carefully:** Any changes committed to this repository will be applied to the cluster. Ensure you thoroughly understand the impact of your modifications before committing.

