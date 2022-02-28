# Energi JS
Simple NodeJS app to serve a dummy API. The simple GitLab CI is configured to test and deploy the latest codes on different branches. It has Helm Charts so are capable to be deployed on the Kubernetes, but you need to add the ingress according to your Kubernetes infra (EKS, GKS, minikube, etc). Also, you can use Docker to run it locally or on the fly!

# CI/CD
## Git Flow
The pipeline can work just on the following branches.
- `develop`: for developments, developers constantly merge other branches directly to this branch.
- `stage`: only develop branch can be merged with this branch.
- `prod`: only the stage branch can be merged with this primary branch.

## Run
The code is dockerized so using related commands is accepted.
### Local
Use the simple `docker` commands to run the code.
```
# first of all, need to build the docker image
docker build -t energi-js:local-latest .

# then, run a container using the image just created
docker run -itd -p 3000:3000 --name energi-js-local energi-js:local-latest  
```
### k8s
Make sure you have GitLab runner for Docker, k8s, and SonarQube.
```
# deploy the code to a k8s cluster
helm install k8s k8s/

# get the 
```

## Stages
There are four main stages.
### build
- It builds the latest docker image and pushes that to the docker registry.
- All of the main branches could run the jobs.
### test
- This stage has a job for testing the js app internally.
- Only the `prod` branch could run the jobs.
### review
- This stage review the code quality of the commits and branches.
- Only the `prod` branch could run the jobs.
### deploy
- It can deploy the `develop` and `stage` branches to the swarm cluster, and deploy the latest version of the code in the `prod` branch to the k8s cluster using Helm Charts.
- All of the main branches could run the jobs.