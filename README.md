# End-to-End Automation with Azure DevOps CI/CD Pipelines +GitOps


### Sample Voting-App

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


This project is about making it easier to build, test, and launch applications using Azure DevOps. Instead of doing everything manually, it sets up an automated process called a CI/CD pipeline. This pipeline takes care of building your code, testing it to make sure everything works, and then deploying it to the environment you choose, like development, testing, or production.

## There are 3 phases.


### Phase 1 :Continuous Integration on Azure Devops Platform

Step 1 : Setup Azure Repos

Step 2 : Set Azure Container Registeries

Step 3: Create Azure Pipelines

Step 4: Setup agent pool

Step 5: Setup Pipelines for Voting and worker microservices


### Phase 2 : Continuous Deployment using gitops

Step 1: Kubernetes Cluster Creation using Azure Kubernetes Service (AKS)

Step 2: Setup Azure CLI

Step 3: Install and configure Argo-CD

Step 4: Deploy Application on argocd


### Phase 3: Automating the whole cicd part

Step 1: Creating a shell script

Step 2 : Added a new stage in pipeline

# Step 1 : Setup Azure Repos

1.	Go to your Azure portal and search for devops organization and create a new organization.

2. click on Azure Repos and then click on Import option.

3. Paste the github project repo link
```shell
git@github.com:Ajay-Kumar2103/sample-voting-app.git
```
4. Provide the github project repo URL and then click on Import

   ![image](https://github.com/user-attachments/assets/c28d26fd-d07c-45f9-b0e1-d7d24c392659)


   ![image](https://github.com/user-attachments/assets/d5119b2d-6b46-418c-a1a8-86f5565709ef)


   ![image](https://github.com/user-attachments/assets/62dcbd61-61a5-4710-aea9-9db4ed01da8c)


 5. It will fetch all the Files and branches from github.

 6. Now you have to set main branch as default branch.
  
 7. Select branches from left menu and then click on three dots of main branch where you find a option to set main branch as a default branch.

 8. We set main branch as default one beacause the latest code will always have to be in main branch.



# Step 2 : Set Azure Container Registeries

1. Create a Container Registries on Azure account. Once it is created then it will be ready to use for storing docker images.



# Step 3: Create Azure Pipelines

Azure Pipelines helps you automatically build, test, and deploy your code just like jenkins It’s like a robot that takes your code, checks if it works correctly, and sends it to where it needs to be (like a website or app store) without you doing it manually every time.


1.	Click on Pipelines Option present in the left menu

 
![image](https://github.com/user-attachments/assets/a8d83ffe-81be-463b-be04-15f5935c8a28)

2. In our case we fetch the code from github and store it in Azure Repos so we have to select Azure repos from options given below.


![image](https://github.com/user-attachments/assets/b0c6334a-8b93-4d5d-ad20-287fbbeb4e2b)

3. These are the pre templates of your pipeline code in our case we are using docker for building and pushing the image to ACR

![image](https://github.com/user-attachments/assets/a5271b0a-9087-4c5d-821d-4c2d5804b128)


4. Select the subscription and container regitery that we created earlier then click validate and configure.

5. It will provide the template for azure pipeline. For reference use "azure-pipelines-1.yaml" in repos file.

6. In yaml file we can find "trigger" path we can choose when a pipeline can be triggerd based on changes we made. For example if we update result/Dockerfile, the pipeline will run. If you change something outside this folder, it won't trigger.

```shell
trigger:
  paths:
    include:
      - result/*
```

7. We can validate and run the pipeline.

![image](https://github.com/user-attachments/assets/a54130ac-00f4-4ba8-92df-b2c1f34bc10d)


We got the above error because the agentpool has not been created. So we need to create agentpool now.



# Step 4: Setup agent pool

An agent pool in Azure DevOps is like a group of workers (computers or virtual machines) that perform tasks in our pipeline, such as building, testing, or deploying our code.

## How It Works:
•	When we run a pipeline, it looks for an available “worker” in the agent pool to carry out the tasks we have defined.

•	Each worker is called an agent, and it runs one job at a time.

## Why It’s Useful:

1.	Resource Management: It allows we to manage and organizeyour agents into groups, so we can use them for specific pipelines or projects.

### Types of Agent Pools:

### 1.	Microsoft-Hosted Agent Pool:
   
•	Provided by Azure DevOps.

•	Includes pre-configured virtual machines (e.g. Windows, Linux, macOS).

•	You don’t need to maintain or set up anything just use it.

### 2. Self-Hosted Agent Pool:

•	Machines that you set up and maintain (e.g. your own servers or VMs).

•	Useful if you need:

•	More customization (e.g., special software or configurations).

•	To avoid limitations in Microsoft-hosted agents.

In our Case we are going to use Self-Hosted Agent Pool

First we need to create Virtual Machine in which our agent is being hosted.

We can either create a virtual machine on cloud or you can use your system as agentpool.


1. Click on Organization settings --> Agent pools --> Add Pool ---> Self hosted ---> Name the agent ---> create

2. Select new agent, follow below instructions to setup agent


![image](https://github.com/user-attachments/assets/66f6be38-9486-4c6f-bd48-6091fba825cf)


3. After running  "config.sh" script we need to create a access token in azure portal and store it on agent for successful authentication purposes.

4. Click on agents tab --> personal access tokens --> create access token by giving required permissions.

5. Copy the token and use it as shown below.

![image](https://github.com/user-attachments/assets/c4973df1-4c16-4a1d-a861-4b6e8eae15be)


![image](https://github.com/user-attachments/assets/6f027b1f-f192-44a1-89dd-6344dfe79c57)


6. Make sure agent is online is ready to use.

![image](https://github.com/user-attachments/assets/8e88cf71-1df9-4514-9140-fb37288713db)


7. If you are Going to pipeline section you will see your pipeline will autmatically triggered by the agent.


![image](https://github.com/user-attachments/assets/227c7854-dd61-4be8-a35f-c506800ed371)


8. Click on the stage to view logs.

![image](https://github.com/user-attachments/assets/e5c6e8ed-704c-4ef7-8341-2b291eb2a520)


Now everything is setup you all need to make two more pipelines for voting and worker microservices.



# Step 5: Setup Pipelines for Voting and worker microservices

We can use the same yaml file for the "Worker" and "vote" microservices. Under trigger section mention both names, so that pipeline can be triggered for them.

As of now you have successfully created 3 pipelines and all your pipelines successfully build and push docker images to Azure Container Registeries.


![image](https://github.com/user-attachments/assets/6b6e51dc-1c93-45a3-96e7-bbe954c694ec)



![image](https://github.com/user-attachments/assets/6dbd9777-8f21-410a-a956-276c20e982a9)



### Here we completed our Phase 1 that is CI part on Azure Devops Platform Let’s move to CD part.



# Phase 2 : Continuous Deployment


## Step 1: Kubernetes Cluster Creation using Azure Kubernetes Service (AKS)


1. Go to azure portal and create a kubernetes cluster for the ArgoCD deployment.

2. Instll azure cli and configure it.

3. create the argocd namespace.
```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```
4. Above command will downloads the install.yaml file from the Argo CD GitHub repository.
   
•	The file contains a predefined set of instructions to install Argo CD into your Kubernetes cluster.

•	Kubernetes will create all the necessary resources (like Deployments, Services, Pods, etc.) required to run Argo CD in the argocd namespace.


5. Run below command to verify everything is installed or not.

```shell
Kubectl get pods -n argocd
```

![image](https://github.com/user-attachments/assets/4d626b57-d059-4d65-ba0b-e23577f12033)

6. To access the UI of Argocd we need credentials that is stored as secret.

```shell
kubectl get secrets -n argocd
```
7. Open the yaml file for admin secret
```shell
kubectl edit secrets argocd-initial-admin-secret -n argocd
```

![image](https://github.com/user-attachments/assets/c451fb74-a33f-462e-ad13-950db320c756)


8. Copy the password

9. Initially it is base64 encoded and we need to decode the password

```shell
#run the following command to encode the secret
echo <encoded_password> |base64 --decode
```
10. Copy the decoded password and paste it in notepad
```shell
kubectl get svc -n argocd
# This will display a list of all services in the argocd namespace
```
![image](https://github.com/user-attachments/assets/0f28742b-244a-4425-880c-670800838510)

11. Checkout service for the argocd-server

![image](https://github.com/user-attachments/assets/e0e7f613-9310-40e5-bf17-2c0c41cce236)

12. As we see the argocd server expose as ClusterIP we need to change this to NodePort

If you want to open Argo CD’s UI in your web browser or interact with its API from outside the cluster, ClusterIP won’t work. Switching to NodePort exposes it so you can reach it via <Node IP>:<NodePort>

For example:
•	ClusterIP: Accessible only inside Kubernetes (e.g., http://argocd-server:8080).

•	NodePort: Accessible outside (e.g., http://<NodeIP>:32000).

Think of ClusterIP as a private door that can only be opened from inside your house (the cluster). Changing it to NodePort adds a public door that anyone outside your house (the cluster) can use, so you can easily access Argo CD from your browser or tools.


13. Run the Following command to edit the argocd-server manifest
```shell
kubectl edit svc argocd-server -n argocd
```

14. Look for type and change It to NodePort.

![image](https://github.com/user-attachments/assets/6b0a0a82-2479-4535-87f7-1ce34096177b)

```shell
kubectl get svc -n argocd
```
![image](https://github.com/user-attachments/assets/3d703fb9-c736-4f2f-b27e-f4ddda08626c)
you need to open the port no. 30645

15. Go to your azure portal type vmss
![image](https://github.com/user-attachments/assets/c010b92b-b996-47e6-8e69-05eb92c9b767)


16. click on agent pool>networking
![image](https://github.com/user-attachments/assets/0e9e9a70-4a6f-4d07-950a-4f7b43a9e839)


17. Create a New port rule and open the port no. 30645 that is our argocd port.
![image](https://github.com/user-attachments/assets/44b44e24-b656-4f60-b255-a1deffd85687)


18. Go back to your terminal
```shell
# this command will provide the externalip of node
kubectl get nodes -o wide
```

![image](https://github.com/user-attachments/assets/2a665489-aace-4e97-91f9-a0d47c88290b)

19. copy the external ip and go to your browser
    type http://<public_ip>:<port_no.>


20. Now go back to broswer and proceed to advanced.
    ![image](https://github.com/user-attachments/assets/4a377d16-2807-4e32-b54d-2075caaf25a3)


21. username == admin password is what you decoded earliar in the blog



# Step 4: Deploy Application on argocd

## 1. Connect With Azure Repos

1. First you need to conned the Azure repo which contain the source code.

2. click on setting>repositories.
![image](https://github.com/user-attachments/assets/4630420d-b187-4b43-8ba1-c6e4d831353c)

3. Change the connection method to HTTPS
   ![image](https://github.com/user-attachments/assets/a737a2c9-ade3-45d6-b62f-b1dc0df3cd09)


4. For repository url go to azure repos and then click on clone
![image](https://github.com/user-attachments/assets/c6021c7d-196c-45d1-89c4-cbe4a8b6de3c)


5.Paste in the URL section

Replace "Organisation name" with a newly genarated token on the azure portal.


6. As it is a private repo so we need to token that we had generated earliar for connection paste your token here.
![image](https://github.com/user-attachments/assets/ad4fdef8-0e29-48d9-bec4-fbf599560ff4)

![image](https://github.com/user-attachments/assets/9c97092b-db92-4def-a9f8-a669ab116221)

Click on connect repo

![image](https://github.com/user-attachments/assets/75576866-da83-4564-99a2-2d1cac6a686e)

Connection will be successful now.


7. We need to change the Application refresh timout to 10 sec by default it is 3 min. So we need to edit the config file of argocd and add the section of application refresh timeout.
```shell
kubectl edit configmap argocd-cm -n argocd
```

![image](https://github.com/user-attachments/assets/f1bc5bbd-ff67-4a37-9516-3efbf22a1a3c)

```shell
#Restart the argocd server
kubectl rollout restart deployment argocd-server -n argocd
```

![image](https://github.com/user-attachments/assets/7eb60096-cecd-4fa4-8e91-589e5d113a43)




# 2. Create a new Application

1.	Click on New app section on argocd
![image](https://github.com/user-attachments/assets/070f79f7-11f3-4116-92e7-2f77efd8a899)

2. Keep all the details as it shown in the below images
![image](https://github.com/user-attachments/assets/f8821df1-acd0-4631-964d-8fcb13e02ad2)
![image](https://github.com/user-attachments/assets/a01b7d46-5ff8-4ea7-bda4-4ed0680d173e)
Repository URL along with the token

3. In the path section provide the manifest location. All the manifest is stored in k8s-specification folder.
![image](https://github.com/user-attachments/assets/c440b911-906a-460a-8111-7d9e5bfcd0bc)

![image](https://github.com/user-attachments/assets/44930705-9545-4929-89bc-f2b3df95bd0d)
![image](https://github.com/user-attachments/assets/2c583ef1-8288-4eb4-8cef-9af70256d63e)


4. Your Application is now deployed on Azure Kubernetes Cluster via argo-cd.
![image](https://github.com/user-attachments/assets/fdb153c9-a1ef-4534-8032-7080f7127362)

5. Go to your terminal check for the pods and services and open the no. in which your services is running.
```shell
kubectl get pods
kubectl get svc
```
![image](https://github.com/user-attachments/assets/bbac0fcf-af53-4897-ad3e-3e4bcb3daa70)


6. go to yourazure portal and search for vmss>nodepool and open the 31000,31001 port that is for vote app
![image](https://github.com/user-attachments/assets/fa91e0b8-f541-4700-95de-7cabdb7d744d)


7. collect the node external ip and again fo to your web broswer type http://<node_ip>:<port_no.>
```shell
kubectl get nodes -o wide
```
![image](https://github.com/user-attachments/assets/3b44b043-645a-4da3-ac53-8a3a2df71abb)
copy any of the node ip

![image](https://github.com/user-attachments/assets/355700bf-d76d-4394-901e-09946ed1c25e)
### Voting Microservice

![image](https://github.com/user-attachments/assets/8e79a14a-2d87-4442-87c8-3f77407c87f7)
### Result Microservice


Here we done Continuous Deployment Part of our Application using GitOps Approach.




# Phase 3: Automating the whole cicd part
























































   


