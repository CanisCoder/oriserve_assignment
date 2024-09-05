### Oriserve Problem statement 1
### Step-by-Step Documentation

**1. Create Project Files**

- **Objective**: Set up the initial files required for the application deployment.
- **Steps**:
  - Created the `index.html` file to serve as the main web page of the application.
  - Created the `appspec.yml` file to define the deployment instructions for AWS CodeDeploy.
  - Created the script files (`install_nginx.sh`, `start_nginx.sh`) to automate the installation and startup of NGINX.

**2. Push Files to GitHub**

- **Objective**: Push the created files to GitHub for version control and easy access during deployment.
- **Steps**:
  - Initialized a Git repository in the project folder.
  - Added the files, committed the changes, and pushed them to the GitHub repository.

**3. Clone the Repository in Jenkins Pipeline**

- **Objective**: Clone the GitHub repository in the Jenkins pipeline for automated deployment.
- **Steps**:
  - Configured the Jenkins pipeline to use the `git` command to clone the repository during the pipeline execution.

**4. Setup Jenkins Pipeline**

- **Objective**: Automate the deployment process using Jenkins.
- **Steps**:
  - Installed Jenkins and configured the necessary plugins.
  - Created a new pipeline job in Jenkins and defined the pipeline script to automate the deployment.

**5. Install NGINX**

- **Objective**: Install NGINX on the server to serve the application.
- **Steps**:
  - Used shell commands in the Jenkins pipeline to update the package manager and install NGINX.

**6. Build and Copy Files**

- **Objective**: Build the application and copy the required files to the NGINX directory.
- **Steps**:
  - Used shell commands to copy the `index.html` file to the `/var/www/html/` directory.

**7. Package and Upload to S3**

- **Objective**: Package the application files and upload them to an S3 bucket for deployment.
- **Steps**:
  - Compressed the project files into a zip archive.
  - Uploaded the package to the specified S3 bucket using AWS CLI commands.

**8. Configure AWS CodeDeploy**

- **Objective**: Set up AWS CodeDeploy to manage deployments to EC2 instances.
- **Steps**:
  1. **Create an Application**:
     - Created a new application in AWS CodeDeploy and selected "EC2/On-premises" as the compute platform.
  2. **Create a Deployment Group**:
     - Configured a deployment group with the correct EC2 instances, load balancer, and auto-scaling group settings.
  3. **Configure Load Balancer**:
     - Integrated an Application Load Balancer (ALB) to distribute traffic and perform health checks on the instances.
  4. **Set Up Auto Scaling Group**:
     - Created an Auto Scaling group to manage instance scaling and ensure availability during deployments.

**9. Deploy to AWS CodeDeploy**

- **Objective**: Deploy the packaged application to the target EC2 instances.
- **Steps**:
  - Executed AWS CLI commands in the Jenkins pipeline to create a deployment using AWS CodeDeploy.

**10. Install AWS CodeDeploy Agent**

- **Objective**: Ensure the CodeDeploy agent is running on the EC2 instances for communication with AWS CodeDeploy.
- **Steps**:
  - Installed the CodeDeploy agent on the EC2 instances, configured it, and verified that it was running correctly.

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

