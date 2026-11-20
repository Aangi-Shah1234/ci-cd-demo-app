# AWS EC2 CI/CD Deployment Pipeline

An automated Continuous Integration and Continuous Deployment (CI/CD) pipeline that deploys a web application to an Amazon EC2 Linux instance running Apache HTTP Server using GitHub Actions and AWS services.

## Architecture and Components

1. Amazon EC2
   - Virtual server (Linux) hosting the web application.
   - Configured with security groups to allow HTTP traffic on port 80 and SSH on port 22.

2. Apache HTTP Server (httpd)
   - Installed and enabled on the EC2 instance.
   - Serves web assets directly from `/var/www/html/`.

3. Amazon S3 Bucket
   - Bucket Name: `cicd-source-demo-ashah`
   - Stores deployment packages and source backups (`my-app.zip`).

4. AWS IAM Roles and Policies
   - Configured permissions for EC2, CodeBuild, and CodeDeploy services.
   - Ensures secure access management across AWS resources.

5. GitHub Actions Workflow
   - Defined in `.github/workflows/deploy.yml`.
   - Triggers automatically upon pushing code to the `main` branch.
   - Uses GitHub Secrets (`EC2_HOST`, `EC2_USERNAME`, `EC2_PRIVATE_KEY`) to authenticate securely.

6. Deployment Specifications
   - `buildspec.yml`: Build specification file for AWS CodeBuild.
   - `appspec.yml`: Deployment specification for AWS CodeDeploy.
   - `scripts/restart_server.sh`: Bash script to restart the Apache service via systemctl.

## How the Pipeline Works

1. Code Commit: A developer pushes changes (e.g., updates to `index.html`) to the `main` branch of the GitHub repository.
2. Trigger: GitHub Actions detects the push event and initiates the `deploy` job.
3. File Transfer: The workflow uses SCP to transfer updated application files to the EC2 instance.
4. Deployment: An SSH action copies the files into `/var/www/html/index.html`.
5. Service Restart: The Apache service is restarted (`sudo systemctl restart httpd`) to apply updates.
6. Verification: The updated webpage is immediately accessible via the EC2 instance public IP address.

## Project Structure

```text
ci-cd-demo-app/
|-- .github/
|   `-- workflows/
|       `-- deploy.yml
|-- scripts/
|   `-- restart_server.sh
|-- appspec.yml
|-- buildspec.yml
|-- index.html
|-- my-app.zip
`-- README.md
```

## Key Learnings

- Understanding the core principles of CI/CD: eliminating manual server logins, manual file transfers, and manual service restarts.
- Configuring Linux web servers (Apache httpd) and automating daemon management with systemctl.
- Implementing secure automated deployments using SSH keys and GitHub Secrets.
- Managing artifact storage with Amazon S3 alongside version control on GitHub.
