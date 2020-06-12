# Services and Networking
* **Pods are ephemeral**—They may come and go at any time, whether it’s because a pod is removed from a node to make room for other pods, because someone scaled down the number of pods, or because a cluster node has failed.
* Kubernetes assigns an IP address to a pod after the pod has been scheduled to a node and before it’s started—Clients thus can’t know the IP address of the server pod up front.
* **Horizontal scaling means multiple pods may provide the same service**—Each of those pods has its own IP address. Clients shouldn’t care how many pods are backing the service and what their IPs are. They shouldn’t have to keep a list of all the individual IPs of pods. Instead, all those pods should be accessible through a single IP address.

## Services
* A Kubernetes Service is a resource you create to make a single, constant point of entry to a group of pods providing the same service.
* Label selectors determine which pods belong to the Service.
* The easiest way to create a service is through **kubectl expose** command.
* You can create service manually by posting a YAML to kubernetes API server.
```YAML
apiVersion: v1
kind: Service
metadata:
  name: kubia
spec:
  ports:
  - port: 80               # The Port this service will be available on
    targetPort: 8080       # The container port the service will forward to
  selector:                # All the pods with app=kubia label will be part of this service
    app: kubia
```
* Few ways to make a service accessible externally, **NodePort**, **LoadBalancer**, **Ingress**
* **NodePort**- By creating a NodePort service, you make Kubernetes reserve a port on all its nodes (the same port number is used across all of them) and forward incoming connections to the pods that are part of the service.
```YAML
apiVersion: v1
kind: Service
metadata:
  name: kubia-nodeport
spec:
  type: NodePort             # Set the service type to NodePort
  ports:
  - port: 80                 # This is the port of the servics's internal cluster IP
    targetPort: 8080         # This is the target port of the backing pods
    nodePort: 30123          # The service will be accessible through port 30123 of each of your cluster nodes
  selector:
    app: kubia
```
