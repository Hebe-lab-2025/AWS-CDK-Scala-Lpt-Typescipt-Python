
Understanding AWS Compute Services — From Zero to Hero
Create a Repository and Push the Docker Image

Amazon Elastic Container Service (ECS) is a fully managed container orchestration service by AWS. 
It simplifies the deployment and management of containerized applications, 
allowing us to run Docker containers at scale. 

ECS integrates seamlessly with other AWS services, 
providing a flexible and efficient solution for deploying and managing containerized workloads.



ECS serves a unique purpose in the AWS ecosystem, 
complementing both EC2 instances and AWS Lambda. 

EC2 provides virtual servers that need manual provisioning and management, 
whereas ECS offers container orchestration, 
allowing for efficient deployment and scaling of containerized applications. 

On the other hand, AWS Lambda excels at event-driven, 
short-lived tasks but may not be suitable for long-running or stateful applications. 
ECS provides a middle ground, offering flexibility and control over the execution environment 
while still abstracting away much of the infrastructure management. 

Therefore, it’s valuable for a wide range of applications.

ECS can use an Amazon Elastic Container Registry (ECR) for container management. 

This integration ensures reliable and efficient deployment of containers on ECS, 
facilitating version control and ensuring consistency in application delivery.

Amazon Elastic Container Registry (ECR) is an AWS managed Docker container registry service. 
ECR provides a secure and scalable repository to store, manage, and deploy Docker images.



Create a repository
In this task, we’ll create a private repository in Amazon ECR. 
After completing this task, the provisioned infrastructure should look like the one shown in the figure below:


Provisioned infrastructure after creating an ECR repository

Provisioned infrastructure after creating an ECR repository

To create a private repository, follow the steps below:

- Look for “ECR” in the search bar and choose “Elastic Container Registry” from the search results.

- Go to “Repositories” under “Private registry” from the left sidebar.

- Click the “Create repository” button and set the repository name to computelab-ecs-repo.

- Leave all other settings as default, and click the “Create” button at the bottom of the page.

- Copy and save the “URI” of the repository. We’ll use it later.

Push the Docker image

Now, we’ll use an EC2 instance created earlier, computelab-ec2-instance, to build our Docker image.

- Go to the AWS Management Console and search for the “EC2” option.
- Click the “EC2” service from the search results to access the EC2 dashboard.

- Click the “Instances” option on the left sidebar under the “Instances” section. 
- Select computelab-ec2-instance and click the “Connect” button.


Make sure the connection type is “Connect using EC2 Instance Connect.”

Leave everything else at its default setting and select “Connect” at the end.

Follow the steps below to push the Docker image to computelab-ecs-repo:

In the EC2 instance terminal, run the commands given below to install Docker:

Ace Editor
Next, proceed with configuring the AWS CLI on the EC2 instance, 
which will enable us to push our image to the ECR private repository. 

Run the command given below and provide the required credentials. 
To access the credentials, click the key icon next to the lab timer on the top of this page. 
Provide us-east-1 as the region and none for the output format.

Ace Editor
Now, run the command given below to switch to the files and export your account ID required to build the Docker image:

Ace Editor
Note: Replace <ACCOUNT ID> with your account ID in the command above. 
Click the top-right corner in the AWS Management Console. Copy <ACCOUNT ID> from the drop-down menu.

Execute the command below to create a Docker client:

Ace Editor
Next, run the command given below to build a Docker image.

Note: This step takes 2–3 minutes to complete.

Ace Editor
After the build is completed, tag the image to push it to the repository:

Ace Editor
Lastly, run the following command to push this image to the repository:

Ace Editor
Navigate to the “Elastic Container Registry” console on AWS and click the “Repositories” option 
in the left-hand navigation pane.

Look for computelab-ecs-repo and save the “Repository URI” for later. 
We’ll need it in the “Create a Task Definition” task.

Congratulations on successfully pushing the Docker image to the ECR private repository. 
We’ll now use this image to create a container in Amazon ECS and deploy our application.


Test
Next
Educative: AI-Powered Interactive Courses for Developers
