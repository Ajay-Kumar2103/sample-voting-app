# Sample Voting-App

Download Docker in local machine.

This solution uses Python, Node.js, .NET, with Redis for messaging and Postgres for storage.

Run in this directory to build and run the app:

```shell
docker compose up
```

The `vote` app will be running at [http://localhost:8080](http://localhost:8080), and the `results` will be at [http://localhost:8081](http://localhost:8081).


# Architecture

![Architecture Diagram](https://github.com/Ajay-Kumar2103/sample-voting-app/blob/main/Final_outputs/architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time



# Azure DevOps pipeline


Now we can implement this using Azure DevOps 


![arch](https://github.com/Ajay-Kumar2103/sample-voting-app/blob/main/Final_outputs/img11.jpeg)


In this project I have used Azure Devops to create CI/CD pipeline where the docker image is created from three different services Vote, Worker and Result. The docker image will be then pushed to ACR. We will include a bash script here which pulls the docker image whenever a new image is updated in ACR and this image will be deployed by ArgoCD which I already deployed on a Kuberentes Cluter.

# Continuous Interration

1. Create a new project in Azure DevOps Organisation
2. Now Import the repos from Github to Azure
3. Create a container registery in Azure which will store the docker image that we generate using azure pipeline
4. Now create the azure pipeline for docker (azure pipeline.yaml as above). 
5. Then create a selfhost agent to run all these configarations. Go to project settings --> Agent pools --> Add pool and create new agent by follwing steps mentioned.
6. Now create a new token in Azure Devops project and add token to your agent for verfication purpose. Make sure agent is listening to jobs in project
7. Now run the pipeline, once completed do for remaining micro services.
   

# Continuous Deployment

1. Now create a kubernetes cluster in azure and deploy it and install the ArgoCD in it.
2. After Configuring ArgoCD, create an app with details of project and makesure it is in sync with azure pipeline
3. Now use the script file which pushes the ACR image to ArgoCD to keep it updated with new changes.
4. So we finally completed the CI/CD in this project


   


