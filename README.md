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
Name the load balancer fargate-app-alb then scroll on down to target group and select create new target group and name it fargate-app-tg. Then create service and wait for a few minutes.  
Before making the cluster service go to the ecsTaskExecutionRole and make sure it has the AmazonECSTaskExecutionPolicy and within the policy, go to the trust relationship tabs and make to see something like this:  
{  
  "Version": "2012-10-17",  
  "Statement": [  
    {  
      "Effect": "Allow",  
      "Principal":  {  
        "Service": "ecs-tasks.amazonaws.com"  
      },  
      "Action": "sts:AssumeRole"  
    }  
  }  
}  
After checking that go about creating the cluster service and you should see this after a few minutes:  
<img width="1563" height="689" alt="image" src="https://github.com/user-attachments/assets/5a369c62-5969-4dd4-8ad2-00c36d02bb5d" />  
Then click on the service and go the tasks portion and verify if they are in a running state like so:  
<img width="1540" height="261" alt="image" src="https://github.com/user-attachments/assets/ae55f900-69ca-4b07-b68a-3c1e0852f90e" />  
Then go to the EC2 section and click on load balancers and select the load balancer that was created from prior. You will see this page:  
<img width="1607" height="880" alt="image" src="https://github.com/user-attachments/assets/2180cfbf-2fa0-4aa0-9730-45cae3d78b95" />  
Then copy the dns name and open a new browser tab and paste the dns name with http:// at the beginning of it and you will see this:  
<img width="1856" height="1039" alt="image" src="https://github.com/user-attachments/assets/59e62132-7fc7-4712-bcbc-139cf814e496" />  

# CloudWatch
Go to the ECS tab and click on task definitions and select the fargate-task-app task and click on the option dropdown of create new revisision.  
<img width="1853" height="602" alt="image" src="https://github.com/user-attachments/assets/470541e8-b363-440a-963b-c457f45a83f3" />  
After making sure the logs portion of the container are in the right places, then click on create and the task definition will be created.  
Then go back to clusters, go to services, click on fargate-app-service, click on service auto scaling tab to see this:  
<img width="1568" height="906" alt="image" src="https://github.com/user-attachments/assets/0eedc055-e145-46b4-8835-6d4e0b96da88" />  
Click on the set number of tasks and set minimum to 2 and maximum to 6 and you will see this after saving:  
<img width="1567" height="812" alt="image" src="https://github.com/user-attachments/assets/d95bc211-2371-45ba-9a64-ccb1a64425e5" />  
Then click on create a scaling policy and see this:  
<img width="1593" height="784" alt="image" src="https://github.com/user-attachments/assets/a276937b-3874-4053-9d8b-ba51a6e9e61e" />  
Keep it on target tracking and name the service matric: ECSServiceAverageCPUUtilization. Set target percent utilization at 70%, scale in at 120 and scale out at 60.  
Then click create scaling policy and see this:  
<img width="1559" height="904" alt="image" src="https://github.com/user-attachments/assets/691aefd3-c9ff-4554-99e5-357d9daa1a48" />  
