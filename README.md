# Simple-Docker

Absolutely, let's dive into each step with more detail:

### 1. Log in to AWS:

**a. Go to the AWS Management Console:**
   - Open your web browser and navigate to https://aws.amazon.com/.
   - Click on "Sign In to the Console" in the top-right corner.

**b. Enter your credentials and log in:**
   - Input your email address and password associated with your AWS account.
   - Click on "Sign In".

**c. Choose the correct region:**
   - AWS services and resources are region-specific, so make sure to select the region where you want to deploy your application.
   - You can choose the region from the dropdown menu in the top-right corner of the AWS Management Console.

### 2. Set up EC2 instance:

**a. Navigate to EC2 service:**
   - In the AWS Management Console, under "Find Services", type "EC2" in the search bar and select "EC2" from the options.

**b. Launch a new EC2 instance:**
   - Click on the "Launch Instance" button to initiate the instance creation wizard.
   - Choose an Amazon Machine Image (AMI), such as Ubuntu Server.
   - Select an instance type based on your requirements (e.g., t2.micro for a free-tier eligible instance).
   - Configure instance details like network settings, subnets, and IAM role (if needed).
   - Add storage as per your application's requirements.
   - Configure security group settings to allow inbound traffic on port 80 (HTTP) and port 22 (SSH) from your IP address or anywhere if it's a test environment.
   - Review your configuration and launch the instance.
   - Choose an existing key pair or create a new one for SSH access.

**c. Download the private key:**
   - After launching the instance, AWS will prompt you to download the private key (.pem file).
   - Store this key in a secure location, as you'll need it to SSH into your EC2 instance.

### 3. SSH into the EC2 instance:

**a. Open terminal or command prompt:**
   - On your local machine, open a terminal (Linux/Mac) or command prompt (Windows).

**b. Use SSH to connect to your EC2 instance:**
   - Navigate to the directory where your private key (.pem file) is stored.
   - Run the following command to connect to your instance:
     ```
     ssh -i your_key.pem ubuntu@your_ec2_public_ip
     ```
   - Replace `your_key.pem` with the filename of your private key and `your_ec2_public_ip` with the public IP address of your EC2 instance.

### 4. Install Docker on EC2 instance:

**a. Update the package index:**
   - Run the following command to update the package index:
     ```
     sudo apt update
     ```

**b. Install necessary packages:**
   - Install necessary packages to allow apt to use a repository over HTTPS:
     ```
     sudo apt install apt-transport-https ca-certificates curl software-properties-common
     ```

**c. Add Docker’s official GPG key:**
   - Run the following command to add Docker’s official GPG key:
     ```
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
     ```

**d. Set up the Docker repository:**
   - Add the Docker repository to your system's software sources:
     ```
     sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
     ```

**e. Update the package index again:**
   - Run `sudo apt update` to update the package index again.

**f. Install Docker:**
   - Install Docker Community Edition (CE) using the following command:
     ```
     sudo apt install docker-ce
     ```

**g. Verify Docker installation:**
   - After installation completes, verify that Docker is installed correctly by running:
     ```
     docker --version
     ```
   - You should see the Docker version installed on your EC2 instance.

### 5. Install Jenkins on EC2 instance:

**a. Follow official Jenkins installation guide:**
   - Visit the Jenkins website for the official installation guide for Ubuntu: https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
   - Follow the instructions provided to install Jenkins on your EC2 instance.

**b. Start and enable Jenkins:**
   - Once Jenkins is installed, start the Jenkins service and enable it to start on boot using the following commands:
     ```
     sudo systemctl start jenkins
     sudo systemctl enable jenkins
     ```

**c. Access Jenkins:**
   - Open a web browser and navigate to `http://your_ec2_public_ip:8080`.
   - Follow the instructions to complete the initial setup of Jenkins.
   - Retrieve the initial admin password from the Jenkins server (usually located at `/var/lib/jenkins/secrets/initialAdminPassword`) and paste it into the Jenkins setup wizard.

### 6. Set up Jenkins:

**a. Complete Jenkins setup wizard:**
   - Follow the instructions in the Jenkins setup wizard to configure Jenkins.
   - Install recommended plugins or select plugins based on your requirements.
   - Create an admin user and set up your Jenkins instance according to your preferences.

**b. Install necessary plugins:**
   - Once Jenkins is set up, navigate to "Manage Jenkins" > "Manage Plugins".
   - Install necessary plugins such as Docker Pipeline Plugin for Docker integration.

### 7. Configure Jenkins pipeline:

**a. Create a new pipeline project:**
   - Click on "New Item" on the Jenkins dashboard.
   - Enter a name for your pipeline project and select "Pipeline".
   - Click "OK" to create the project.

**b. Configure pipeline to pull source code:**
   - In the project configuration, under the "Pipeline" section, choose "Pipeline script from SCM".
   - Select your version control system (e.g., Git).
   - Enter the repository URL and credentials if required.
   - Specify the branch to build.

**c. Define stages in Jenkinsfile:**
   - Create a `Jenkinsfile` in the root directory of your project.
   - Define stages like build, test, and deploy using Groovy syntax.
   - Include steps to build and push Docker images, deploy containers, etc.

### 8. Write Dockerfile for React app:

**a. Create a Dockerfile:**
   - In the root directory of your React project, create a file named `Dockerfile` (without an extension).

**b. Define the base image:**
   - Choose a base image suitable for running a Node.js application. For example:
     ```
     FROM node:14-alpine
     ```

**c. Copy source code:**
   - Copy your React application source code into the Docker image:
     ```
     WORKDIR /app
     COPY . .
     ```

**d. Install dependencies:**
   - Install dependencies using npm or yarn:
     ```
     RUN npm install
     ```

**e. Expose necessary ports:**
   - Expose the port your React app is running on (usually port 3000):
     ```
     EXPOSE 3000
     ```

**f. Define start command:**
   - Define the command to start

 your React app:
     ```
     CMD ["npm", "start"]
     ```

### 9. Build Docker image:

**a. Add a stage in Jenkins pipeline:**
   - Edit your Jenkins pipeline script to add a stage for building the Docker image.

**b. Use Docker Pipeline Plugin:**
   - Use the Docker Pipeline Plugin in Jenkins to build the Docker image using the `Dockerfile` in your project.

### 10. Push Docker image to a registry:

**a. Add a stage in Jenkins pipeline:**
   - Add a stage to your Jenkins pipeline to push the built Docker image to a Docker registry (e.g., Docker Hub, AWS ECR).

**b. Configure credentials in Jenkins:**
   - Add credentials in Jenkins to authenticate with the Docker registry.

**c. Use Docker Pipeline Plugin:**
   - Use the Docker Pipeline Plugin in Jenkins to push the Docker image to the registry.

### 11. Deploy Docker container:

**a. Add a stage in Jenkins pipeline:**
   - Add a stage in your Jenkins pipeline to deploy the Docker container on your EC2 instance.

**b. SSH into the EC2 instance:**
   - Use Jenkins Pipeline SSH steps to SSH into your EC2 instance from the Jenkins server.

**c. Pull Docker image and run container:**
   - Inside the SSH stage, pull the Docker image from the registry and run the Docker container on the EC2 instance.

### 12. Test the deployment:

**a. Access your EC2 instance:**
   - Open a web browser and navigate to the public IP address of your EC2 instance.

**b. Verify the React app:**
   - If everything is set up correctly, you should see your React app running on the EC2 instance.

### 13. Monitor and maintain:

**a. Set up monitoring and logging:**
   - Use AWS CloudWatch or other monitoring tools to monitor the performance and health of your EC2 instance and Docker containers.

**b. Regular maintenance:**
   - Regularly update and maintain your application, Docker images, and infrastructure to ensure security and performance.

Following these detailed steps should help you deploy your small React app through Jenkins in Docker on AWS successfully. Let me know if you need further clarification on any part!
