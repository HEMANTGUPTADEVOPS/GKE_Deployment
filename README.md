# GKE_Deployment
GKE_Deployment on Google Kubernetes Engine:

We have two file deployment.yml and service.yml
1.Deployment.yml :
It is the file that consist of all paramters that say how our python app going to deploy on the Google Kubernetes Engine

2.Service.yml:
It is the file that tells how are we going to expose our application the outside world

For Deployment :
$kubectl apply -f deployment.yml

For Exposing a service over loadbalancer:
$kubectl apply -f service.yml

Solution Design:

As we have to deploy our application with zero downtime so we are using two variables of the kubernetes system :
1.maxSurge: That is how many new pods(our python app code) will be created when we are going to deploy the new image over our cluster
2.maxUnavailable:That is number of pods from that set that can be unavailable after the eviction or during deployment

So we have set the variable maxSurge as 1 so that during the deployment atleast one pod will be created immediately and maxUnavailable as 1 so that during deployment if pods are successfully created then pods will be terminated 1 by 1 at a time not both the pod so our application will be highly avialable.
This is somewhat of rolling upgrade but if we have some more resources we can go for blue green deployment as well where we can launch 2 another instance and then terminate the old instance one by one by saying the strategy as maxSurge:2 and maxUnavaible:1

CPU Based Autoscaling Strategy:
For Keeping the increase in launch we have created a horizontal pod autoscalar which will spin up 2 more pods if cpu goes above 50% on both the running pods and cools down as minimum if cpu load comes to normal that is below then 50% on both the pods in the last part of deployment.yml 

For Exposing to outside world:
Here we are making use of Loadbalancer for exposing our application to outside world on particular ports

http://k8s.34.93.150.157.xip.io/

For making such domain we have updated in our nginx as server_name as k8s.34.93.150.157.xip.io

We can as well make use of ingress for exposing our services internal within other application but as this was just a start will do it.











