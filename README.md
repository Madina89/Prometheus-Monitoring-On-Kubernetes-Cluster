# Prometheus-Monitoring-On-Kubernetes-Cluster
How to Setup Prometheus Monitoring On Kubernetes Cluster
# Steps:
  1.  First Create a Namespace Kubernetes namespace for all our monitoring components.  `kubectl create namespace monitoring`
  2.  Create a file named clusterRole   `kubectl create -f clusterRole.yaml`
  3.  Create a file called config-map `kubectl create -f config-map.yaml`
  4.  Create a deployment on monitoring namespace and  Create a file named prometheus-deployment `kubectl create  -f prometheus-deployment.yaml`
  5. You can check the created deployment using the following command  `kubectl get deployments --namespace=monitoring`
 
 # Connecting To Prometheus Dashboard
    1. You can view the deployed Prometheus dashboard in two ways: 
              1.Using Kubectl port forwarding.
              2.Exposing the Prometheus deployment as a service with NodePort or a Load Balancer.
              
 # Using Kubectl port forwarding
Using kubectl port forwarding, you can access the pod from your workstation using a selected port on your localhost.
 1. First, get the Prometheus pod name `kubectl get pods --namespace=monitoring`
 2. The output will look like the following:
      `kubectl get pods --namespace=monitoring`
`NAME                                     READY   STATUS    RESTARTS   AGE`
`prometheus-deployment-56f854c695-5pspk   1/1     Running   0          2d19h`
3. Execute the following command with your pod name to access Prometheus from localhost port 8080.
Note: Replace prometheus-monitoring-3331088907-hm5n1 with your pod name.
`kubectl port-forward prometheus-monitoring-3331088907-hm5n1 8080:9090 -n monitoring`
4. Now, if you access http://localhost:8080 on your browser, you will get the Prometheus home page.

# Exposing Prometheus as a Service.
To access the Prometheus dashboard over a IP or a DNS name, you need to expose it as Kubernetes service.

1. Create a file named prometheus-service.yaml and copy the following contents.
2. We will expose Prometheus on all kubernetes node IP’s on port 30000.
3. Create the service using the following command `kubectl create -f prometheus-service.yaml --namespace=monitoring`
4. Once created, you can access the Prometheus dashboard using any Kubernetes node IP on port 30000. 
5. If you are on the cloud, make sure you have the right firewall rules for accessing the apps.
You should see this output when everything works correctly.
<img width="1259" alt="Screenshot 2019-11-13 at 2 56 51 PM" src="https://user-images.githubusercontent.com/50155760/68804249-eba1e100-0626-11ea-88fa-17b330d25a42.png">
6.Now if you browse to status --> Targets, you will see all the Kubernetes endpoints connected to Prometheus automatically using service discovery as shown below. So you will get all kubernetes container and node metrics in Prometheus.
<img width="1248" alt="Screenshot 2019-11-13 at 2 57 36 PM" src="https://user-images.githubusercontent.com/50155760/68804319-17bd6200-0627-11ea-9533-3c1220c96c43.png">
7. You can head over the homepage and select the metrics you need from the drop-down and get the graph for the time range you mention. An example graph for container memory utilization is shown below.
8. Setting Up Kube State Metrics. Kube state metrics service will provide many metrics which is not available by default. Please make sure you deploy Kube state metrics to monitor all your kubernetes API objects like deployments, pods, jobs, cronjobs etc. 


