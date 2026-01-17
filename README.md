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






