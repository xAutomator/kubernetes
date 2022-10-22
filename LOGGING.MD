#Kubernetes Logging Cheatsheet


### Appl;ication Logs
   
    k logs $podname # outputs all logs when pod has one container
    k logs $$podname $containername # gets the logs for a specific container in a pod
    k logs $podname -f # Stream the logs

If you have a kubernetes metrics server installed you can use the following commands:

    k top no
    k top po

### Node Monitoring

Download and install the kubernetes metrics server

    git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
    cd kubernetes-metrics-server
    k apply -f .