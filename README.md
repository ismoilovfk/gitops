# gitops
## What is GitOps for me ?
GitOps is a form of Infrastructure as Code (IaC) where you can manage all services from Git repositories. You have only one entry point for changes, which is critical in a production environment (protected branches and a complete history of changes).
### GitOps Repository Structure
```sh
gitops/
|
|---master/
|   |   
|   |---project1/
|   |   |--- Chart.yaml
|   |   |
|   |   |--- frontend.master.yaml
|   |   |    
|   |   |--- backend.master.yaml
|   |   |   
|   |   |--- admin.master.yaml
|   |   |
|   |   |---templates/
|   |       |
|   |       |--- deploy.yaml
|   |       |
|   |       |--- service.yaml
|   |       |
|   |        |--- ingress.yaml
|   |---project2/
|       |--- Chart.yaml
|       |
|       |--- frontend.master.yaml
|       |    
|       |--- backend.master.yaml
|       |   
|       |--- admin.master.yaml
|       |
|       |---templates/
|           |
|           |--- deploy.yaml
|           |
|           |--- service.yaml
|           |
|           |--- ingress.yaml
|
|---dev/
    |   
    |---project1/
    |   |--- Chart.yaml
    |   |
    |   |--- frontend.dev.yaml
    |   |    
    |   |--- backend.dev.yaml
    |   |   
    |   |--- admin.dev.yaml
    |   |
    |   |---templates/
    |       |
    |       |--- deploy.yaml
    |       |
    |       |--- service.yaml
    |       |
    |        |--- ingress.yaml
    |---project2/
        |--- Chart.yaml
        |
        |--- frontend.dev.yaml
        |    
        |--- backend.dev.yaml
        |   
        |--- admin.dev.yaml
        |
        |---templates/
            |
            |--- deploy.yaml
            |
            |--- service.yaml
            |
            |--- ingress.yaml
```
## Prerequisites
* Ensure you have a repository with the public SSH key uploaded. The corresponding private key should be securely stored on your local machine.
* An ArgoCD cluster with permissions to add repositories and create applications
* Install the ArgoCd CLI and optionally kubectl for managing applications through the CLI. Alternatively, applications can also be managed via the Argo CD UI.

## Steps to deploy...

1.**Set Up GitOps Repository Structure:**
* Create a Git repository (gitops) where all configuration and deployment manifests will be stored.
* Organize your repository with separate branches (master, stage, dev...) to manage different environments.
* Within each environment branch (master and dev), organize projects (project1, project2, etc.) representing different applications or services.
* Each project directory should contain:
    *   A Chart.yaml file for Helm charts metadata.
    * Application manifests (frontend.master.yaml, backend.master.yaml, etc., for master branch; frontend.dev.yaml, backend.dev.yaml, etc., for dev branch).
    * A templates/ directory containing Kubernetes manifests (deploy.yaml, service.yaml, ingress.yaml, etc.) for deployment configurations.