# Spring-Boot-K8s-App

Spring boot application with docker as container engine and kubernates as orchestration engine.


# Step to create docker image

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
    
            
# Step to create kubernates deployment object

 1. Run following command to create deployment object.
 
       kubectl create -f deployment.yaml
       
   	It also create kubernates Pod and Replicaset object
   	 
        
# Step to create ClusterIp service
 
 1. Deployment object are expose to internal cluster using clusterip service.
 
 2. Run following command to create clusterip service
 
     kubectl create -f clusterip.yaml
     
 3. For testing purpose we use port forwarding so we will able to access clusterip   
    service on localhost
    
    run following command for port forwarding (This is only testing purpose. not required 
    in production)
    
    kubectl port-forward service/spring-boot-k8s-service 8081:8081
    
 4.  Then Call following GET REST API 

     http://localhost:8081/api/v1/test
     
  It will return following response with statusCode as 200 in response header
     'Welcome to spring boot k8s example'
   
# Step to create Nodeport service
 
 1. Deployment object are expose to external cluster using nodeport service.

 2. Run following command to create nodeport service. It also create clusterip service 
    implicitaly.
 
     kubectl create -f nodeport.yaml
          
 3. Then get your minikube IP address using following command
 
     minikube ip
     
    and use this IP to call following GET REST API 
    
    http://<minikube ip>:30180/api/v1/test
     
   It will return following response with statusCode as 200 in response header
     'Welcome to spring boot k8s example'
     
# Step to create Loadbalancer service

 1. Deployment object are expose to external cluster using loadbalancer service.
    It use in cloud environment as it create loadbalancer resource for external access.
    
 2. For access loadbalancer service on local minikube setup
    add 'externalIPs' tag with value as output of following command
    
     minikube ip
     
   If this value is not mention then external ip for loadbalancer service is pending state.

 3. Run following command to create loadbalancer service. It also create clusterip  
    service and nodeport implicitaly.
 
     kubectl create -f loadbalancer.yaml
          
 4. Then call following GET REST API 
    
    http://<minikube ip>:8081/api/v1/test
      
   It will return following response with statusCode as 200 in response header
     'Welcome to spring boot k8s example'


  