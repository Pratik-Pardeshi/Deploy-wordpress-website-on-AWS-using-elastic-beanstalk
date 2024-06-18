# Deploy WordPress Website on AWS using Elastic Beanstalk

## Introduction
This guide walks you through the steps to deploy a WordPress website on AWS using Elastic Beanstalk. Elastic Beanstalk is a platform-as-a-service (PaaS) offered by AWS that simplifies the deployment and management of applications. This tutorial assumes you have a basic understanding of AWS services and some experience with Linux.

## Prerequisites
- Basic knowledge of AWS Elastic Beanstalk and RDS.
- AWS account with appropriate permissions.
- AWS CLI installed and configured.
- SSH key pair for EC2 instances.
- Basic knowledge of Linux command-line operations.

## Setup Instructions

### 1. Launch RDS Database

1. **Search for RDS on AWS Console**:
   - Navigate to the RDS service.

2. **Create Database**:
   - Choose `Standard Create`.
   - Select `MySQL` as the engine.
   - Set your master username and password. You can also auto-generate the password.
   - Provide a name for your database under `Additional Configuration`.
   - Keep the other settings as default and click `Create Database`.
   - Wait for the database to be created (3-4 minutes).

### 2. Launch Elastic Beanstalk Environment

1. **Search for Elastic Beanstalk on AWS Console**:
   - Navigate to the Elastic Beanstalk service.

2. **Create Application**:
   - Choose `Create a new application`.
   - Select `Web server environment`.
   - Name your application.

3. **Select Platform**:
   - Choose `PHP` from the managed platforms.

4. **Configure Service Role**:
   - Create a new service role if not already created.
   - Select `aws-elasticbeanstalk-service-role`.

5. **Configure EC2 Key Pair and Instance Profile**:
   - Choose your EC2 key pair.
   - Select an instance profile (e.g., `ec2accessdemoapp`).

6. **Network Configuration**:
   - Use the default VPC.
   - Select appropriate subnets for the instance and database.

7. **Security Group**:
   - Choose the default security group.

8. **Health Reporting**:
   - Select `Basic`.
   - Untick `Managed Updates`.

9. **Database Connection**:
   - Add the following environment properties:
     - `RDS_HOSTNAME`: The hostname of the DB instance (found in the RDS console under `Connectivity & security`).
     - `RDS_PORT`: The port of the DB instance.
     - `RDS_DB_NAME`: The database name (`ebdb`).
     - `RDS_USERNAME`: The master username.
     - `RDS_PASSWORD`: The master password.

10. **Review and Launch**:
    - Review your configuration.
    - Click `Submit` and wait for the environment to be created (around 5 minutes).

### 3. Download and Prepare WordPress

1. **Clone WordPress Repository**:
   - On your local machine, open a terminal and run the following commands:
     ```bash
     mkdir wordpress-deployment
     cd wordpress-deployment
     git clone https://github.com/Pratik-Pardeshi/Deploy-wordpress-website-on-AWS-using-elastic-beanstalk.git
     cd deploy-wordpress
     zip -r wordpress.zip *
     ```

### 4. Upload and Deploy WordPress

1. **Upload WordPress to Elastic Beanstalk**:
   - Go to the Elastic Beanstalk console.
   - Select your environment.
   - Click `Upload and Deploy`.
   - Upload the `wordpress.zip` file.
   - Click `Deploy`.

2. **Fix 404 Nginx Error**:
   - Go to the EC2 Dashboard.
   - Find the instance created by Elastic Beanstalk.
   - SSH into the instance:
     ```bash
     ssh -i <your-key-pair-name>.pem ec2-user@<public-ip>
     ```

3. **Move WordPress Files**:
   - Run the following commands on the EC2 instance:
     ```bash
     sudo su
     cd /var/www/html/
     mv wordpress/* .
     rm -rf wordpress
     ```

4. **Verify Deployment**:
   - Open the URL provided by Elastic Beanstalk.
   - You should see the WordPress setup page.

## Troubleshooting

- If you encounter a 404 error, ensure that the WordPress files are correctly moved to `/var/www/html/`.
- Verify that all environment variables for the RDS database are correctly set.
- Check the Elastic Beanstalk logs for any errors and resolve them as necessary.

## Conclusion

Congratulations! You have successfully deployed a WordPress website on AWS using Elastic Beanstalk. For any further assistance, refer to the [AWS Elastic Beanstalk Documentation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html).
