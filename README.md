Task 1: Create a new GitHub repository
You will need a free GitHub.com account for this lab. We recommend using a personal account thatwas
configured in previous labs. You can sign up or login in to an existing account at https://github.com/
Login to github and create a new repository by navigating to https://github.com/new
Use the following settings for the code repository
• Name: “my-app”
• Description: My AWS Application
• Private repo
Once created, connect the the repository to your local machine.
git init
git remote add origin https://github.com/<YOUR_GIT_HUB_ACCOUNT>/my-app.git


------------------------------------------------------------------------


In this project we will deploy a simple docker web page image that redirect to your linkedin account using Elastic Kurernates Cluster. Here are the steps:

Step 1: Create the HTML Page
Create a file named index.html:
--------------------------------------------------

---------------------------------------------------------------------------------
Step 2: Create the Dockerfile

step 3: Create a file named Dockerfile:


---------------------------------------------------------------------------------------------
Step 4: Build and Run the Docker Image
4.1 Build the Docker Image

Run the following command to build the Docker image:

docker build -t linkedin-redirect .

4.2 Run the Docker Container

Run the container locally to test it:

docker run -d -p 8080:80 linkedin-redirect

4.3 Test the Redirection

Open your browser and navigate to:

http://localhost:8080

You should see the HTML page redirecting you to your LinkedIn profile after 5 seconds.
-------------------------------------------------------------------------------------------
Step 5: Push the Image to ECR

Tag the Image

    docker tag linkedin-redirect:latest <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/linkedin-redirect:latest

Authenticate Docker with ECR

    aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com

Push the Image to ECR

    docker push <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/linkedin-redirect:latest

------------------------------------------------------

Step 6: Deploy to ECS

Update your ECS Task Definition to use the new image in the main.tf:

image = "<AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/linkedin-redirect:latest"

-----------------------------------------------------------------------------------------------------------------------------

