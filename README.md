# Installation
```bash
git clone https://github.com/Vlad-Misiukevich/m11_kafkaconnect_json_azure.git
```
# Requirements
* Windows OS
* Python 3.8
* kubernetes-cli
* azure-cli
* terraform
# Description
1. Login to Azure:  
`az login`
![img.png](images/img.png)
2. Deploy infrastructure with terraform:  
`terraform init`  
`terraform plan -out terraform.plan`  
`terraform apply terraform.plan`
![img_1.png](images/img_1.png)
3. Connect to kubernetes cluster:  
```
az aks get-credentials \
    --resource-group rg-vmisiukevich-westeurope \
    --name aks-vmisiukevich-westeurope \
    --subscription 45a58420-2e1a-4ba1-b23b-c55612222ef5
```
![img_2.png](images/img_2.png)
4. Run the proxy for Kubernetes API server:  
`kubectl proxy`  
5. Build and push image to DockerHub:  
![img_3.png](images/img_3.png)
6. Create the namespace to use:  
`kubectl create namespace confluent` 
![img_4.png](images/img_4.png)
7. Set this namespace to default for your Kubernetes context:  
`kubectl config set-context --current --namespace confluent`  
![img_5.png](images/img_5.png)
8. Add the Confluent for Kubernetes Helm repository:  
`helm repo add confluentinc https://packages.confluent.io/helm`  
`helm repo update`
![img_6.png](images/img_6.png)
9. Install Confluent for Kubernetes:  
`helm upgrade --install confluent-operator confluentinc/confluent-for-kubernetes`
![img_7.png](images/img_7.png)
10. Install all Confluent Platform components:  
`kubectl apply -f ./confluent-platform.yaml`
![img_8.png](images/img_8.png)
11. Install a sample producer app and topic:  
`kubectl apply -f ./producer-app-data.yaml`
![img_9.png](images/img_9.png)
12. Check that everything is deployed:  
`kubectl get pods -o wide `
![img_10.png](images/img_10.png)
13. Set up port forwarding to Control Center web UI from local machine:  
`kubectl port-forward controlcenter-0 9021:9021`
![img_11.png](images/img_11.png)
14. Browse to Control Center:
![img_12.png](images/img_12.png)
15. Create a kafka topic in Control Center:
![img_13.png](images/img_13.png)
16. Set up port forwarding to connect cluster from local machine:  
`kubectl port-forward controlcenter-0 8083:8083`
![img_14.png](images/img_14.png)
17. Create connector and read data from storage container into Kafka topic:  
`curl.exe -d "@C:\Users\Uladzislau_Misiukevi\PycharmProjects\m11_kafkaconnect_json_azure\connectors\azure-sour
ce-cc-expedia.json" -H "Content-Type: application/json" -X POST http://localhost:8083/connectors`
![img_17.png](images/img_17.png)
![img_16.png](images/img_16.png)
18. Mask time from the date field to format "0000-00-00 00:00:00":
![img_18.png](images/img_18.png)
19. Result:
![img_19.png](images/img_19.png)
![img_20.png](images/img_20.png)