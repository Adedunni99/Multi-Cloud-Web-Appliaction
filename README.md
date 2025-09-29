# Multi-Cloud App

****I DEPLOYED MY FIRST WEB APP ACROSS THREE DIFFERENT PLATFORM...…AWS, GCP, AND Kubernetes.. 

****Prerequisites:

----Firstly build a docker image; verify it locally and confirm----

 --Install CLIs  

--AWS CLI: aws (configured with aws configure)

--kubectl (for Kubernetes)

--gcloud (for GCP)

--If you don’t have these, install and configure them before continuing.

--Know your values

--AWS region  us-east-1

--AWS account id 


Multi-Cloud Deployment Milestone

  🟠  AWS

--AIM--
Dockerize the app 

<img width="525" height="437" alt="Screenshot from 2025-09-11 14-00-31" src="https://github.com/user-attachments/assets/fdf2569d-19f0-4432-8ee3-889b969b6908" />

---Deploy using AWS Copilot, which handled ECS/Fargate setup and networking automatically.

Step 1.  Install Copilot 

****Why Copilot? It automates VPC, ALB, IAM roles and creates an ECS Fargate service for you.

     curl -Lo copilot https://github.com/aws/copilot-cli/releases/latest/download/copilot-linux
     chmod +x copilot
     sudo mv copilot /usr/local/bin/copilot

  
<img width="797" height="439" alt="Screenshot from 2025-09-15 09-49-13" src="https://github.com/user-attachments/assets/a2e7aebb-8734-4d72-bb52-9fa2398b21eb" />

       Verify Copilot: copilot –version
       
Step 2.  Verify AWS: aws configure

Step 3.  Initialize the co-pilot: copilot init 

        --app myapp \
        --name myapp \
        --type "Load Balanced Web Service" \
        --dockerfile ./Dockerfile \
        --port 8080

      
Step 4.   Deploy to the default environment


<img width="1059" height="510" alt="Screenshot from 2025-09-15 14-28-18" src="https://github.com/user-attachments/assets/0819f3aa-ac89-4802-998e-b57ac87464e1" />  

   

   <img width="1059" height="510" alt="Screenshot from 2025-09-15 14-28-40" src="https://github.com/user-attachments/assets/416991fc-3728-4ebe-9e64-b9bf2c182e64" />
   

   

 <img width="1068" height="528" alt="Screenshot from 2025-09-15 14-29-00" src="https://github.com/user-attachments/assets/8093db34-b36f-408b-8cbe-3bea13ab741f" />

   

   
   <img width="1068" height="528" alt="Screenshot from 2025-09-15 14-29-11" src="https://github.com/user-attachments/assets/b0b010fe-2be4-46a8-8788-e52256832281" />
   


   
                                  http://adedun-Publi-fA3n-161943.us-east-1.elb.amazonaws.com
    




****SO I'VE BEEN SUCCESFULLY ABLE TO:
  1.Push my Docker image to AWS ECR
              
  2.Create an ECS Fargate cluster
              
  3.Create a load balancer
              
  4. Deploy my app





   🔵 Kubernetes---
 
-----Learn how to create deployments and service YAML files and debug pods.

 -----Build a container image and run it on Minikube.
 
 
Step 1. Install Minikube:

    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube

   ----Start cluster

      minikube start

Step 2. Push image to Docker Hub (so Kubernetes can pull it)

      docker tag myapp <your-dockerhub-username>/myapp:latest
      docker login
      docker push <your-dockerhub-username>/myapp:latest


Step 3. Create Kubernetes manifests

---Create a file k8s.yaml


Step 4. Apply manifests

     kubectl apply -f k8s.yaml

Check

     kubectl get pods
	 kubectl get svc
	 
<img width="600" height="211" alt="Screenshot from 2025-09-22 15-35-57" src="https://github.com/user-attachments/assets/d91e1e6a-d2eb-45f0-ab47-c86eb9c33b46" />


Step 5. Access the app

  ****If using minikube:

    minikube service myapp-service

  This opens with your browser;;


<img width="813" height="293" alt="Screenshot from 2025-09-22 15-54-25" src="https://github.com/user-attachments/assets/15e3612f-2483-4060-8214-7ecaa530c174" />


  🟢 GCP---

****Deployed the container on Cloud Run for a fully managed, serverless setup.

Step 1. Enable APIs

    gcloud services enable run.googleapis.com containerregistry.googleapis.com

  <img width="722" height="80" alt="Screenshot from 2025-09-24 17-56-56" src="https://github.com/user-attachments/assets/c8f99c11-6954-4acb-9c70-cfbb2fc755bd" />


STEP 3. Tag & push image

    PROJECT_ID=$(gcloud config get-value project)

    docker tag myapp gcr.io/${PROJECT_ID}/myapp:latest
    docker push gcr.io/${PROJECT_ID}/myapp:latest

STEP 2. Configure Docker to use gcloud credentials

Run:

    gcloud auth configure-docker

********This updates your ~/.docker/config.json so Docker can automatically use your gcloud credentials********
    

<img width="627" height="392" alt="Screenshot from 2025-09-24 21-33-38" src="https://github.com/user-attachments/assets/8e24db24-10b3-4a42-bc82-f17213650d9c" />


 **** AFTER THESE STEPS, DOCKER WILL HAVE PERMISSION AND THE PUSH WILL SUCCEED****



Step 3. Deploy to Cloud Run

    gcloud run deploy myapp \
      --image gcr.io/${PROJECT_ID}/myapp:latest \
      --platform managed \
      --region us-central1 \
      --allow-unauthenticated \
      --port 8080


********CLOUD RUN WILL PRINT A PUBLIC HTTPS URL.

    https://myapp-3949.us-central1.run.app
    

<img width="678" height="295" alt="Screenshot from 2025-09-24 22-08-22" src="https://github.com/user-attachments/assets/bf6c4181-0f59-4634-ba99-ae1569747aaa" />


-----SUMMARY---


---AWS ECS (Copilot) → one command copilot init, then copilot svc deploy.

---Kubernetes → write k8s.yaml, kubectl apply -f k8s.yaml, then expose service.

---GCP Cloud Run → push to GCR, then gcloud run deploy.

