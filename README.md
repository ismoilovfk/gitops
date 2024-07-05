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