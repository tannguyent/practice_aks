1.deployment by command

-- create deployment 
kubectl create deployment node-api-deployment --image=localhost:5000/node-api.local --port=80 --replicas=2

--create services
kubectl expose deployment node-api-service --port=80 --target-port=3000 --type=NodePort

-- check 
kubectl get deployment
kubectl get nodes
kubectl get svc

-- show log 
kubectl describe pods
kubectl logs pod node-api-deployment-xxx
kubectl logs deployment/node-api-deployment

-- delete 
kubectl delete deployment node-api.deployment
kubectl delete service node-api-services

2.deploy by yaml file
-- create
kubectl create .\deploy\k8s.yaml

--delete 
kubectl delete .\deploy\k8s.yaml


3.deployment by aks 

--create acr
az acr create -n tannt89acr -g kuberg -l southeastasia --sku basic

-- login 
az acr login

-- push image 
docker push name.azurecr.io/image:tag

--create az service principal 
az ad sp create-for-rbac  --skip-assignment

-- grant perrmission to access 
az role assignment create --assignee "" --role Reader --scope $acrId

--create aks cluster
az aks create --name akscluster --resource-group kuberg --node-count 1 --generate-ssh-keys --service-principal "" --client-secret ""

-- deploy
-- local service using NodePort
-- kubectl apply -f .\deploy\k8s-local.yaml


-- aks service using LoadBalancer
kubectl apply -f .\deploy\k8s.yaml


-- delete 
kubectl delete -f .\deploy\k8s.yaml


4. update image 
docker build . -t tannt89acr.azurecr.io/nodeapikube:v2
docker push tannt89acr.azurecr.io/nodeapikube:v2
kubectl set image deployment node-api-deployment node-api=tannt89acr.azurecr.io/nodeapikube:v2