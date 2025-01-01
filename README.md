In this project we will deploy a simple docker web page image that redirect to your linkedin account using Elastic Kurernates Cluster. Here are the steps:

Step 1: Create the HTML Page
Create a file named index.html:
--------------------------------------------------

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

