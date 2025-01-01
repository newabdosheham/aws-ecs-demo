In this project we will deploy a simple docker web page image that redirect to your linkedin account using Elastic Kurernates Cluster. Here are the steps:

Step 1: Create the HTML Page
Create a file named index.html:
--------------------------------------------------
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Redirecting to LinkedIn</title>
    <meta http-equiv="refresh" content="5;url=https://www.linkedin.com/in/your-linkedin-profile/" />
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            padding: 50px;
        }
        h1 {
            color: #0077B5;
        }
    </style>
</head>
<body>
    <h1>Redirecting to My LinkedIn Profile</h1>
    <p>If you are not redirected automatically, click <a href="https://www.linkedin.com/in/your-linkedin-profile/">here</a>.</p>
</body>
</html>

---------------------------------------------------------------------------------
Step 2: Create the Dockerfile

Create a file named Dockerfile:

# Use an official Nginx image as the base image
FROM nginx:alpine

# Copy the HTML file to the default Nginx HTML directory
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]

---------------------------------------------------------------------------------------------
Step 3: Build and Run the Docker Image
3.1 Build the Docker Image

Run the following command to build the Docker image:

docker build -t linkedin-redirect .

3.2 Run the Docker Container

Run the container locally to test it:

docker run -d -p 8080:80 linkedin-redirect

3.3 Test the Redirection

Open your browser and navigate to:

http://localhost:8080

You should see the HTML page redirecting you to your LinkedIn profile after 5 seconds.
-------------------------------------------------------------------------------------------
Step 4: Push the Image to ECR

    Tag the Image

docker tag linkedin-redirect:latest <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/linkedin-redirect:latest

Authenticate Docker with ECR

aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com

Push the Image to ECR

    docker push <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/linkedin-redirect:latest

------------------------------------------------------

Step 5: Deploy to ECS

Update your ECS Task Definition to use the new image in the main.tf:

image = "<AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/linkedin-redirect:latest"

-----------------------------------------------------------------------------------------------------------------------------

