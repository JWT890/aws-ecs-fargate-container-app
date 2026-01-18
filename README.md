# ecs-fargate-container-app

To start go the command line and create the fargate app folder by typing mkdir fargate app and cd fargate app.  
Next to Visual Studio Code and create a python app called app.py and put this info in it:  
<img width="398" height="176" alt="image" src="https://github.com/user-attachments/assets/3bee9162-0875-402a-a447-ce2a47b9aaa6" />  
Then create the Docker file for it like so:  
<img width="439" height="162" alt="image" src="https://github.com/user-attachments/assets/6a4f614b-cad7-40ba-a8eb-7b14e0ffa406" />  
Then run the command docker build -t fargate-app . and then docker run -p 8080:80 fargate-app
You should see this:  
<img width="1066" height="189" alt="image" src="https://github.com/user-attachments/assets/494bb988-990a-4f77-a31e-be5973ec3734" />  
Then go to http://localhost:8080 and see this:  
<img width="1908" height="904" alt="image" src="https://github.com/user-attachments/assets/b5a65960-e307-483b-9eb3-9fb114cb650d" />  

# ECR Repo
First go to AWS IAM and create a fargate user account by naming it fargate-cli-user
And give it the following policies:  
<img width="1309" height="266" alt="image" src="https://github.com/user-attachments/assets/e1c7eabc-90d6-4148-b778-5d25e5982177" />  
After getting it set up and AWS configured squared away type: aws ecr create-repository --repository-name fargate-app and get this output:  
<img width="814" height="316" alt="image" src="https://github.com/user-attachments/assets/c03fc9be-7f81-4125-8448-d1d5a7cb307a" />  
After that run the command $ECR_PASSWORD = aws ecr get-login-password --region us-east-1  
Then run echo $ECR_PASSWORD | docker login --username AWS --password-stdin <account_id>.dkr.ecr.us.east-1.amazonaws.com.  
Then run docker tag fargate-app:latest <account_id>.dkr.ecr.us-east-1.amazonaws.com.  
Then run docker push <account_id>.dkr.ecr.us-east-1.amazonaws.com/fargate-app:latest

# ECR Cluster
Type in the search bar in AWS, ECR which will take you to the Elastic Container Service and then click on clusters in the side bar which will take you to this page:  
<img width="1868" height="941" alt="image" src="https://github.com/user-attachments/assets/b50ba9ec-b870-4472-8e0f-620fc2902093" />  
Click on Create Cluster and you will see this:  
<img width="1837" height="912" alt="image" src="https://github.com/user-attachments/assets/6198b0ca-8f58-47da-b375-e341aea679ae" />  
Name the cluster fargate-cluster, infrastructure as fargate only, then click on create and wait a couple seconds.  
After creation go to the task definitions and click on create new task definition and you will see this:  
<img width="1578" height="899" alt="image" src="https://github.com/user-attachments/assets/646d6fb7-32cc-4b60-aa87-0c4ba0294342" />  
Name the task family fargate-task-app. Have the CPU at 0.5 and Memory at 1 GB. Then go create a role that has a use case of Elastic Container Service, name it ecsTaskExecutionRole and create the role. After creation, attach the AmazonECSTaskExecutionRolePolicy onto it. 
Then in Container name is fargate-app-task, then for the Image URL put '<account_id>.dkr.ecr.us-east-1.amazon.com/fargate-app:latest' in it, container port at 80 with tcp, then click on create.  
Then click on clusters and then click on the cluster that was created and you will see this:  
<img width="1581" height="509" alt="image" src="https://github.com/user-attachments/assets/8d61f238-4a20-45a5-9267-b5090592d262" />  
Then click on the cluster and you will see this:  
<img width="1555" height="661" alt="image" src="https://github.com/user-attachments/assets/cc4f9062-3af9-44ec-aea7-ed4d424c6754" />  
Then on the services section click on create.  
For task definition family, have it as fargate-task-app with defintion revision as 1 and service name as fargate-app-service. Then for environment select launch type, keep at as Fargate and version as latest.  
Then scroll down to security groups and click on create new security group, name it fargate-app-sg, description as Security group for Fargate app tasks. Then for inbound rules have the type as HTTP and source as anywhere.  
Then scroll down to laod balancing and click on use load balancing.  















