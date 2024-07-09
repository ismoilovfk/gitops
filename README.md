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
* ArgoCd CLI and kubectl

## Steps to deploy...

1.**Set Up GitOps Repository Structure:**
* Create a Git repository (gitops) where all configuration and deployment manifests will be stored.
* Organize your repository with separate branches (master, stage, dev...) to manage different environments.
* Within each environment branch (master and dev), organize projects (project1, project2, etc.) representing different applications or services.
* Each project directory should contain:
    *   A Chart.yaml file for Helm charts metadata.
    * Application manifests (frontend.master.yaml, backend.master.yaml, etc., for master branch; frontend.dev.yaml, backend.dev.yaml, etc., for dev branch).
    * A templates/ directory containing Kubernetes manifests (deploy.yaml, service.yaml, ingress.yaml, etc.) for deployment configurations.
2. ** Connect our repo to argocd and create proj in argocd **
```sh
argocd login my-argocd.com
argocd repo add git@github.com:ismoilovfk/gitops.git  --ssh-private-key-path .ssh/id_rsa
argocd proj create production-proj --allow-namespaced-resource proj1-prod
```
3. ** Create ns and applications in argocd one by one **
```sh
kubectl create ns proj1-prod
kubectl apply -f single.application.yaml
```
## Argcocd application for GitOps...
```sh
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: backend.master  -- Application Name in ArgoCD
  namespace: argocd     -- ArgoCD application namespace. Usually, all applications are located in the argocd namespace, but you locate them anywhere.
spec:
  destination:
    namespace: proj1-prod                  -- Namespace where Service shoud locate
    server: https://kubernetes.default.svc -- K8s address in argocd
  project: production-proj                 -- Project name in ArgoCD. This will not affect the application directly.
  source:
    helm:
      valueFiles:
        - backend.master.yaml              -- Values file. I would recommend adding the stage in the filename. This adds an extra level of protection, ensuring you don't accidentally change something while thinking you're in a different branch. Including the branch and stage in the filename provides clarity on where and what stage you are making changes.
      path: project1                      -- Path to the project directory. This specifies the location of the project's source code and configuration files within the repository.
    repoURL: git@github.com:ismoilovfk/gitops.git  -- SSH link to your Git repository
    targetRevision: master                         -- The branch to track. This specifies which branch in the repository ArgoCD should monitor for updates and changes.
  syncPolicy:
    automated:
      prune: true       -- Automatically remove resources that are no longer defined in the Git repository.
      selfHeal: true    -- Automatically correct drift by synchronizing the live state with the desired state defined in the Git repository.

```

## Automation of Creating ArgoCD Applications for GitOps

When configuring GitOps with ArgoCD, managing dozens of projects and hundreds of applications manually can become cumbersome. To streamline this process, using a Helm chart can simplify the creation of hundreds of applications effortlessly.


## Steps to deploy...

```sh
1. Configure the Values file with the necessary application details:
   stage: master
   server: https://kubernetes.default.svc
   project: production-proj
   namespace: proj1-prod
   path: project1
   repo: git@github.com:ismoilovfk/gitops.git
   apps:
     - admin
     - backend
     - frontend

2. Automate the creation of multiple applications to avoid manual creation of hundreds of applications.


```
