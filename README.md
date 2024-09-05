### Problem Statement 1
### Step-by-Step Documentation

**1. Setup Jenkins Pipeline**

- **Objective**: Set up a Jenkins pipeline to automate the deployment process.
- **Steps**:
  1. Installed Jenkins and configured necessary plugins for the pipeline.
  2. Created a new pipeline job in Jenkins.
  3. Configured the pipeline script to automate tasks, including cloning the repository, installing NGINX, building the application, packaging files, uploading to S3, and deploying via AWS CodeDeploy.

**2. Clone Repository**

- **Objective**: Clone the GitHub repository containing the project files.
- **Steps**:
  - Configured the Jenkins pipeline to clone the repository using the `git` step with the specified branch and URL.

**3. Install NGINX**

- **Objective**: Install NGINX on the server to serve the application.
- **Steps**:
  - Updated the package manager and installed NGINX using shell commands.

**4. Build and Copy Files**

- **Objective**: Build the application and copy the required files to the NGINX directory.
- **Steps**:
  - Copied the `index.html` file to the `/var/www/html/` directory using shell commands.

**5. Package and Upload to S3**

- **Objective**: Package the application files and upload them to an S3 bucket for deployment.
- **Steps**:
  - Zipped the project files and used AWS CLI commands to upload the package to the specified S3 bucket.

**6. Configure AWS CodeDeploy**

- **Objective**: Set up AWS CodeDeploy for automated deployments to EC2 instances.
- **Steps**:
  1. **Create an Application**:
     - Navigate to AWS CodeDeploy in the AWS Management Console.
     - Created a new application, selecting "EC2/On-premises" as the compute platform.
  2. **Create a Deployment Group**:
     - Configured a new deployment group for the application.
     - Specified the deployment type as "In-place" and selected the required EC2 instances using their tags.
     - Associated the Auto Scaling group to allow instances to automatically join the deployment group during scaling events.
  3. **Configure Load Balancer**:
     - Integrated an Application Load Balancer (ALB) to route traffic to the healthy instances during deployment.
     - Configured health checks to ensure smooth traffic switching between instances.
  4. **Set Up Auto Scaling Group**:
     - Created an Auto Scaling group with the desired capacity and scaling policies.
     - Linked the Auto Scaling group to the deployment group, ensuring instances are automatically added to CodeDeploy during scaling.

**7. Deploy to AWS CodeDeploy**

- **Objective**: Deploy the application to the target EC2 instances using AWS CodeDeploy.
- **Steps**:
  - Created a deployment using AWS CodeDeploy with the application name, deployment group, and S3 package details.

**8. Install AWS CodeDeploy Agent**

- **Objective**: Ensure the CodeDeploy agent is installed and running on the EC2 instance.
- **Steps**:
  - Installed the CodeDeploy agent using the necessary commands, started the service, and verified its status.

### Major Challenges Faced and Resolutions

1. **Missing Git Credentials in Jenkins**
   - **Problem**: The pipeline failed at the repository cloning stage due to missing Git credentials.
   - **Resolution**: Configured the correct Git credentials in Jenkins by setting up a credential binding within the Jenkins job.

2. **Permissions Error During NGINX Installation**
   - **Problem**: Encountered permission errors when attempting to install NGINX.
   - **Resolution**: Resolved the issue by adding `sudo` to the installation commands and ensuring the Jenkins user had necessary permissions.

3. **Failed Upload to S3 Due to Missing AWS Credentials**
   - **Problem**: The pipeline was unable to upload the packaged files to S3 because of missing AWS credentials.
   - **Resolution**: Configured AWS credentials in Jenkins using the `withAWS` block and ensured the IAM role had proper permissions.

4. **CodeDeploy Agent Not Receiving Lifecycle Events**
   - **Problem**: The deployment failed with an error indicating that the CodeDeploy agent was not able to receive lifecycle events.
   - **Resolution**: Installed the AWS CodeDeploy agent on the EC2 instance, started the service, and ensured the agent was configured correctly to communicate with AWS CodeDeploy.

5. **Load Balancer and Auto Scaling Group Misconfigurations**
   - **Problem**: The deployment group was not correctly routing traffic due to load balancer misconfigurations and the Auto Scaling group not functioning as expected.
   - **Resolution**: Reviewed and adjusted the load balancer target group settings and ensured the health checks were configured correctly. Verified the Auto Scaling group settings to ensure new instances were automatically included in deployments.

