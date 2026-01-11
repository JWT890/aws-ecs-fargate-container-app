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




