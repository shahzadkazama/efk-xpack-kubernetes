# efk-xpack-kubernetes

### cloning the project

    git clone https://github.com/shahzadkazama/efk-xpack-kubernetes.git
    cd efk-xpack-kubernetes

### create namespace logging

    kubectl apply -f namespace.yaml
    kubectl get ns

### create eleasticsearch all services 

    cd elasticsearch/
    kubectl apply -f .
    kubectl get pods -n logging
    kubectl get all -n logging

### open screen and run below command

    screen -S efk
    kubectl logs -f -n logging $(kubectl get pods -n logging | grep elasticsearch-master | sed -n 1p | awk '{print $1}') | grep "Cluster health status changed from \[YELLOW\] to \[GREEN\]"
    
    #exit from screen

### generate auto password

    kubectl exec -it $(kubectl get pods -n logging | grep elasticsearch-client | sed -n 1p | awk '{print $1}') -n logging -- bin/elasticsearch-setup-passwords auto -b

### find the password for elastic user from above command and paste it according

    kubectl create secret generic elasticsearch-pw-elastic -n logging --from-literal password=********************

### create kibana all services

    cd kibana/
    kubectl apply -f .
    kubectl get pods -n logging

### verify the yellow to green status

    screen -r efk

### create fluentd services

    cd fluentd/
    kubectl apply -f fluentd-configMap.yaml
    kubectl apply -f fluentd-daemonSet.yaml
    kubectl logs -f fluentd-vwxdx -n logging
