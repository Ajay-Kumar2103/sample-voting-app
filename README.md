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
