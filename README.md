# Developing-with-Docker

## Project Overview
This project sets up a Dockerized environment including MongoDB, Mongo Express, and a Node.js application. The setup is automated using a Jenkins pipeline to streamline the deployment process.

## Features
- Deploys MongoDB in a Docker container
- Deploys Mongo Express for database management
- Runs a Node.js application
- Automated setup using Jenkins pipeline

## Prerequisites
Ensure you have the following installed on your system:
- **Jenkins** (with necessary permissions to run Docker commands)
- **Docker & Docker Compose**
- **Git**

## Setup and Installation

### 1. Clone the Repository
```sh
rm -rf app  # Ensure the directory is clean before cloning
git clone https://github.com/Abo1406/Developing-with-Docker.git app
```

### 2. Start the Jenkins Pipeline
- Configure a Jenkins job to execute the provided `Jenkinsfile`.
- This pipeline will:
  - Install Docker (if not installed)
  - Create a Docker network
  - Start MongoDB and Mongo Express containers
  - Install Node.js dependencies
  - Start the Node.js application

### 3. Verify Running Containers
Run the following command to check running containers:
```sh
docker ps
```
You should see the MongoDB, Mongo Express, and Node.js app running.

### 4. Access the Services
- **Mongo Express UI:** [http://localhost:5000](http://localhost:5000)
- **Node.js App:** [http://localhost:3000](http://localhost:3000)

## Additional Notes
- Ensure firewall settings allow required ports (3000, 5000, 27017).
- Modify `server.js` to use the correct MongoDB credentials.

## Troubleshooting
If the pipeline fails:
- Check Jenkins logs for errors.
- Verify that Docker is installed and running.
- Ensure MongoDB credentials match those in `server.js`.

## License
This project is open-source and available under the MIT License.


