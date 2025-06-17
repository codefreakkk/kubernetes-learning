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

| Platform             | Install Command                                                                                     | Notes                               |
| -------------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------- |
| kind                 | `kubectl apply -f https://kind.sigs.k8s.io/...`                                                     | Specially configured for Docker     |
| kubeadm (cloud)      | `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/.../cloud/deploy.yaml` | Works on most cloud providers       |
| kubeadm (bare metal) | Same as above + change `LoadBalancer` to `NodePort`, or use MetalLB                                 | Manual setup needed for external IP |

* Some Notes related to ingress
Ingress on Cloud with Kubeadm
When you're on cloud + kubeadm, here’s what happens:
You install the NGINX Ingress Controller using:

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/cloud/deploy.yaml
This manifest creates:

A Deployment running the Ingress NGINX controller pod

A Service of type LoadBalancer called ingress-nginx-controller

Because you're on a cloud provider, Kubernetes talks to the cloud API and automatically creates a cloud Load Balancer (like:

AWS → ELB

GCP → Network Load Balancer

Azure → Azure LB)

This cloud Load Balancer gets an external IP address, which you can use or map a domain name to.

When a request comes in:

It hits the cloud Load Balancer via its external IP

That forwards the request to the Service (ingress-nginx-controller)

The Service forwards traffic to the Ingress Controller Pod

The Ingress Controller uses your Ingress rules to route traffic to the correct Service (your app) inside the cluster

Data Flow Visual:

Browser --> Cloud Load Balancer (external IP)
              |
              v
      Service: LoadBalancer (ingress-nginx-controller)
              |
              v
     Pod: ingress-nginx controller
              |
              v
     Your app’s Service → Deployment


// port forwarding for KIND clusters
kubectl port-forward service/ingress-nginx-controller -n ingress-nginx 80:80 
