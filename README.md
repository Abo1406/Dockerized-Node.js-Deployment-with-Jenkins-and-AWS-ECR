# **Developing with Docker and Deploying to Amazon ECR**

## **Project Overview**
This project demonstrates how to:
- Set up a Dockerized environment including MongoDB, Mongo Express, and a Node.js application.  
- Automate the deployment process using a **Jenkins pipeline**.  
- Dockerize a Node.js application and deploy it to a private Docker registry using **Amazon Elastic Container Registry (ECR)**.

---

## **Features**
- **MongoDB** and **Mongo Express** in Docker containers.  
- A **Node.js** application running in a Docker container.  
- Automated setup with **Jenkins pipeline**.  
- **AWS ECR** integration for hosting Docker images.  

---

## **Prerequisites**
Ensure you have the following installed and configured:
- [Docker](https://docs.docker.com/get-docker/) & **Docker Compose**  
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)  
- **AWS account** with necessary permissions to use ECR  
- **Jenkins** (with permissions to run Docker commands)  
- **Git**  

---

## **Setup and Installation**

### **1. Clone the Repository**
Ensure the directory is clean before cloning:
```bash
rm -rf app
git clone https://github.com/Abo1406/Developing-with-Docker.git app
```

### **2. Start the Jenkins Pipeline**
Configure a Jenkins job to execute the provided Jenkinsfile.  

### **3. Prepare the Node.js Application**
Ensure your application is ready and available in your local machine's project directory.

### **4. Create a Dockerfile**
```dockerfile
FROM node:20-alpine
ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=123
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "server.js"]
```

### **5. Create an AWS ECR Repository**
```bash
aws ecr create-repository --repository-name my-app --region eu-central-1
```

### **6. Authenticate Docker to AWS ECR**
```bash
aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin 423623856295.dkr.ecr.eu-central-1.amazonaws.com
```

### **7. Build and Push Docker Image to AWS ECR**
```bash
docker build -t my-app .
docker tag my-app:latest 423623856295.dkr.ecr.eu-central-1.amazonaws.com/my-app:latest
docker push 423623856295.dkr.ecr.eu-central-1.amazonaws.com/my-app:latest
```

---

## **Deploying the Application with Docker Compose**
Create a `mongo.yml` file:
```yaml
version: '3'
services:
my-app:
image: 423623856295.dkr.ecr.eu-central-1.amazonaws.com/my-app:latest
ports: 
- 3000:3000
  mongodb:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=123
    volumes:
    - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    ports:
      - "5000:8081"
    restart: always
    depends_on:
      - mongodb
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
volumes:
mongo-data:
driver: local
```
mongo-data volume:

Using a named volume (mongo-data) for MongoDB data is a good practice as it allows data to persist even if the container is removed. However, consider specifying access modes or other volume options if needed.
---

## **Access the Services**
- **Node.js App:** [http://localhost:3000](http://localhost:3000)  
- **Mongo Express UI:** [http://localhost:5000](http://localhost:5000)  

---

## **Troubleshooting**
If the pipeline fails:
- Check Jenkins logs for errors.  
- Verify Docker installation.  
- Ensure MongoDB credentials match those in `server.js`.  

---

## **License**
This project is open-source and available under the **MIT License**.
