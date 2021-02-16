#实践


## Module 1 - Create a Kubernetes cluster
kubectl version

kubectl cluster-info

kubectl get nodes


## Module 2 - Deploy an app
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1


kubectl get deployments

kubectl proxy





## Module 3 - Explore your app

kubectl get pods


kubectl describe pods

export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME

curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/

kubectl logs $POD_NAME

kubectl exec $POD_NAME env


kubectl exec -ti $POD_NAME bash



## Module 4 - Expose your app publicly

kubectl get services
<!-- 暴露服务 -->
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080

kubectl describe services/kubernetes-bootcamp

kubectl describe deployment

kubectl get pods -l run=kubernetes-bootcamp

kubectl get services -l run=kubernetes-bootcamp


export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
echo Name of the Pod: $POD_NAME

<!-- 打标签 -->
kubectl label pod $POD_NAME app=v1

kubectl describe pods $POD_NAME

kubectl get pods -l app=v1

kubectl delete service -l run=kubernetes-bootcamp

kubectl exec -ti $POD_NAME curl localhost:8080


## Module 5 - Scale up your app

kubectl scale deployments/kubernetes-bootcamp --replicas=4

kubectl get deployments

kubectl get pods -o wide

## Module 6 - Update your app


kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2


kubectl rollout status deployments/kubernetes-bootcamp

kubectl rollout undo deployments/kubernetes-bootcamp

