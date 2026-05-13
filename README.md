# Containerize a RAG API with Docker

**Project Link:** [View Project](http://learn.nextwork.org/projects/ai-devops-docker)

**Author:** Marco Ball  
**Email:** mballalbarran@gmail.com

---

![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_x7y8z9a0)

---

## Introducing Today's Project!

Today I'm going to take an AI-powered RAG API and wrestle it into a docker container so that it behaves itself anywhere we run it. I'm doing this to sharpen up my Docker, DevOps workflows, FastAPI, Ollama and ChromaDB, all while learning how ai applications are actually packaged up and deployed outside the cosy little world of localhost

### Key services and concepts

Services I used were Docker, Docker Hub, FastAPI, Uvicorn, ChromaDB, and Ollama. The project taught me how containerisation works, how Docker images are built from a Dockerfile, how APIs can run inside isolated environments, and how container registries like Docker Hub are used to distribute applications. I also learnt a fair bit about networking between containers and host machines

### Challenges and wins

This project took me 2 or maybe 3 hours overall, mainly because i spent a good while rebuilding the containers and chasing down the ollama connection issue once the api moved into docker. 
the most challenging part for me was making sense of why it stopped behaving once containerised! Argh!
but definitely rewarding getting the api running successfully inside of docker and moving the wholesetup betweeen machines

### Why I did this project

I did this project because i wanted more practice with docker and containerised ai applications (instead of only from theory and documentation). Definitely met my goals and came away with a better understanding of apis, containers, networking and registries 

---

## Setting Up the RAG API

In this step, I'm setting up my RAG API's code, database and dependencies. 
It'll receive questions from users, searches through documents to find relevant information and uses Ollama AI to generate answers, and sends those answers back

### API setup and workspace

So now i'm installing and setting up docker so that I can containerise my RAG API into a portable environment that runs consistently on any machine (shoutout r/canitrundoom).
A virtual environment is an isolated python workspace that keeps this project's packages and dependencies separate from everything else on my system, which saves us from dpeendency hell and conflicts. 

I need docker because my API relies heavily on specific tools, libraries and embeddings data which only I have here in my project folder. Manually rebuilding this setup on every other machine is not anyone's idea of a good time

### Dependencies installed

Packages used here were FastAPi, Uvicorn, ChromaDB and Ollama. 
FastAPI is the framework which builds the API endpoints to expose the app to the outside world rather than sulking from within the terminal. Uvicorn is the web server itself to run the FastAPI App to handle the requests. 
Then there's ChromaDB which is the vector database to store embeddings for the RAG system, whjich gives theAI a fighting chance of retrieving relevant context instead of spurting out confident hallucinations. 
Finally there's Ollama which is the engine running the llm, letting us serve AI responses directly from our own machine without handing the keys over to a cloud provider

![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_c9d0e1f2)

### Local API working

I tested that my API was working by launching the FastAPI application with Uvicorn and then sending a POST request to the /query endpoint using  Invoke-WebRequest. I passed in a test question about jubernetes and the API returned a valid JSON response with an AI-generated answer pulled through the RAG pipeline. 
The server also responded with a 200 status, which is goooood! That confirms the API was running correctly, accepting requests, querying the vector database and generating responses through Ollama without catching fire


![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_v5w6x7y8)

---

## Installing Docker Desktop

### Docker Desktop setup

Docker desktop is the application that lets me build and manage Docker containers on my machine without spending the afternoon on low-level setup manually through the terminal.
I installed it for this project because my RAG API depends on a very particular stack of Python packages and I want the whole thing bundled into a single predictable environment instead of relying on “works on my machine” optimism. 
Containerisation helps my project by packaging the API dependencies and embeddings database together so the application behaves the same regardless of where it's run


### Docker verification

I verified docker was working correctly by running docker --version to confirm the engine was installed properly.
then I launched `hello-world` container using docker run hello-world. Docker successfully pulled the image from Dockerhub, created a container and ran it to returned the “Hello from Docker!” confirmation message in the terminal.
I could also see the image appear inside Docker Desktop afterwards, which confirmed the whole pipeline was alive and behaving itself 


![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_i9j0k1l2)

---

## Creating the Dockerfile

Next I’m packaging my RAG API into a docker container so I can run the exact same application anywhere without rebuilding the environment from scratch every single time. RAG stands for Retrieval-Augmented Generation, which is basically the AI looking things up from a vector database before answering instead of freestyling its way into complete fiction. I’m creating files like a Dockerfile which tells Docker how to build the environment, install the dependencies, copy the API code, and launch the application. 
After that, I’ll build the image and test the container to make sure the API still works once it’s trapped inside docker’s little portable box of networking and dependency sorcery


### How the Dockerfile works

A dockerfile is the set of instructions to build a container image for an application. Not unlike a recipe, every line tells docker what to prepare, install, files to copy over, and what command should run when the container starts etc.
My Dockerfile builds the full RAG API environment so the application runs the same way wherever I deploy it, which is far nicer than playing dependency bingo across different machines.

FROM python:3.11-slim starts things off with a lightweight Python 3.11 image as the base. WORKDIR /app sets the working directory inside the container, basically the container’s home turf. COPY app.py embed.py k8s.txt ./ moves my API code and source files into the image so Docker has everything it needs to work with

The RUN instructions execute commands during the build process. One installs FastAPI, Uvicorn, ChromaDB, and Ollama, while another runs embed.py to generate the vector embeddings ahead of time

### Containerized API test results

Testing the API after containerisation proved that the entire application still worked properly once it was running inside Docker instead of directly on my machine. I started the container with docker run -p 8000:8000 rag-app, sent a POST request to the /query endpoint, and got back a successful JSON response about Kubernetes with a HTTP 200 status. That confirmed the API, vectordb dependencies and Ollama integration were all alive and talking to each other properly inside the container

The difference between running locally and running in Docker is mainly about consistency and isolation. Running locally depends on whatever happens to exist on my machine already, which can get messy fast once package versions start drifting around like shopping trolleys in a car park. Running in Docker gives the API its own sealed environment with the exact libraries and config it expects every time

![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_o1p2q3r4)

---

## Building and Running the Container

### Docker image build complete

Building a Docker image means Docker reads the instructions in my Dockerfile, executes them one by one and packages the finished result into a reusable image containing the API, dependencies, and configuration. It is basically freezing the entire application environment into something portable and predictable
I verified my docker image was built successfully by running docker build -t rag-app . and watching docker complete each stage of the build process without errors. I could see Docker pulling the Python base image, copying my project files, installing FastAPI and the other dependencies, and generating the embeddings from embed.py

After the build finished, I ran docker images | Select-String rag-app and confirmed the rag-app image appeared in the results with the latest tag. I could also see the image listed inside dockerdesktop, which confirmed the API had officially graduated 

![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_p9q0r1s2)

![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_x7y8z9a0)

---

## Pushing to Docker Hub

Finally I'm pushing my docker image to dockerhub so the container itself can be shared, downloaded and run from basically anywhere. 
Dockrhub is an online container registry where devs store and distribute docker images. A bit like github except instead of code repos it's full of portable application environments (and people arguing about image sizes)

I'm doing this because once the image is on dockerhub II'm then able to run the API on another machine with a single comma while muttering “why is Python doing this to me” under my breath. It also mirrors how containerised apps are shared in production workflows

### Docker Hub push complete

I pushed to Dockerhub by first logging in with docker login, which authenticated my Docker CLI with my Docker Hub account through the browser. After that, I tagged my local image using the pinnersinner/rag-app format so Docker knew which online repository the image belonged to. Then I ran docker push pinnersinner/rag-app, which uploaded the image layers to Docker Hub piece by piece

Docker Hub is useful because it gives me a central place to store and distribute container images. Once the image is uploaded, I can pull it onto another machine with a single command

The big advantage of pushing to a registry is portability. My API is now packaged, versioned, and accessible from anywhere Docker is installed

![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_m5n6o7p8)

### Pulling from Docker Hub

Pulling an image from Docker Hub means downloading a pre-built container image from an online registry onto your local machine. When I ran docker pull pinnersinner/rag-app, Docker connected to Dockerhub, checked the repository and downloaded all the image layers needed to recreate my API locally

When Docker finished the pull, I had the exact same container image I originally built and pushed earlier, complete with the API code, dependencies, vector database setup, and configuration

The difference between building locally and pulling from Dockerhub is where the image comes from. Building locally means docker reads the dockerfile and assembles the image step by step on your machine. Pulling from Dockerhub skips all of that and simply downloads a finished image that somebody already built and uploaded

![Image](http://learn.nextwork.org/calm_turquoise_innocent_sea_turtle/uploads/ai-devops-docker_f5g6h7i8)

---

---
