# Spring-Boot-K8s-App

Spring boot application with docker as container engine and kubernates as orchestration engine.


# Step to create docker image and run container on minikube:-

 1. Go to project path and build the project using following command
 
   mvn clean install
           
   It create executable jar file in project path/target folder.
   
 2. Then run following command in powershell to use local docker registry for minikube  
    cluster. 
    
    minikube docker-env
    minikube docker-env | Invoke-Expression
                   
 3. Then run following command is same powershell window
 
   docker build -t spring-boot-k8s-app:1.0.0 .
          
   It create docker image of name spring-boot-k8s-app and tag 1.0.0 where tag     
   is optional and default value is latest.
    
 5. Then run following command
    kubectl create -f deployment.yaml
    kubectl create -f clusterip.yaml
    
    It create kubernate Deployment object and ClusterIP service(it use to access app
    inside the cluster)                   
  
# Step to test application

 1. ClusterIP service is use to access app inside the cluster. So we need to do to port 
   forwarding to access app on localhost.
   
   
  Use following command for port forwarding
   
   kubectl port-forward service/spring-boot-k8s-service 8081:8081
   
   (This is only testing purpose. not recommended in production)
   
 2. Then Call following GET REST API 

     http://localhost:8081/api/v1/test
     
  It will return following response with statusCode as 200 in response header
     'Welcome to spring boot k8s example'

  