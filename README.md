# vjezba

Dockerfiles: 

ENV fprocess changed to python3 

FROM python:latest changed to static tag python3.8-alpine 

Added apk update; apk upgrade; apk add curl for (alpine) 

---

Created an ingress config file with paths for both servces (services.com /1 /2) (necessary to add an entry to /etc/hosts) 

---
Created a Jenkinsfile with the following stages: 
- Pull service from git on every commit 
- Izgradnja containera koristeći dockerfileove 
- Unit test 
- Integration test 
- Publish to Docker Hub 
- K8s - deploy to cluster for staging 
- Smoke testing 
- End to end tests 
- K8s – deploy to cluster for production 


Self-assessment: 
    Learned/familiarized myself with: 
    
    - Infrastructure as code 
    
    - DevOps principles and tools 
        
    - Docker 
        - Containerization 
        - Building/running images 
        - Dockerfile 
        - Reducing time to deploy and memory usage by correctly writing Dockerfiles 
        - Registries 

    - K8s 
        - Theory (master/slave pods, nodes, services, ingress, clusters) 
        - Volumes 
        - Minikube 
        - Kubectl 
        - Helm3 
        - Yaml config files 

    - Jenkins CI/CD 
        - Basic jobs 
        - Pipeline 
        - Jenkinsfile 
        - Connecting with git and Docker Hub 

Successful: 
    - Improved docker images 
    - Built pods and services, encapsulated using ingress 
    - Deployed images as pods to cluster 
    - Created CI/CD pipeline using Jenkins and Jenkinsfile 

Unsuccessful: 
    - Connecting the services, smoke testing 
    - Automated image versioning within Jenkinsfile
    - Different pipelines for dependent/independent services
