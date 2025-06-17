- kind get clusters // returns available cluster

- kubectl cluster-info --context kind-<cluster-name>

- kubectl get namespaces // "default" is the default namespace if you dont create any namespace then resources goes in this name space

- kubectl get pods -n kube-system // gets pod from the namespace kube-system

- kubectl create ns nginx // creates a new namespace

- kubectl delete namesapce <namespace_name>

- kubectl apply -f namespace.yaml // applies or updates the changes

// how to get inside pod?
 kubectl exec -it pod/nginx-pod -n <namesapce_name> -- bash

// describe pod
- kubectl describe  pod/nginx-pod -n nginx

// get more info for pod
- kubectl get pods -n nginx -o wide

//** Persistent volume


-  kubectl get pv // get pv

- kubectl delete pv/<pv_name> // delete pv

// explore headless service


// installing ingress controller for KIND cluster
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml

// port forwarding for KIND clusters
kubectl port-forward service/ingress-nginx-controller -n ingress-nginx 80:80 
