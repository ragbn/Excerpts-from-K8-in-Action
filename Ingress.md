# Ingress
* The act of going in or entering; the right to enter; a means or place of entering; entryway.
* Multiple services can be exposed through a single Ingress.
* Ingresses operate at the application layer of the network stack (HTTP) and can provide features such as cookie-based session affinity and the like, which services can’t.
* An Ingress controller needs to be running in the cluster.
* For example, Google Kubernetes Engine uses Google Cloud Platform’s own HTTP load-balancing features to provide the Ingress functionality.
* YAML manifest for the Ingress
```YAML
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: kubia
spec:
  rules:
  - host: kubia.example.com               # This ingress maps the kubia.example.com domain name to your service
    http:
      paths:
      - path: /                           # All requests will be sent to port 80 of the kubia-nodeport service
        backend:
          serviceName: kubia-nodeport
          servicePort: 80
```
* Exposing multiple services on same host, but different paths
```YAML
...
  - host: kubia.example.com
    http:
      paths:
      - path: /kubia                # Requests to kubia.example.com/kubia will be routed to the kubia service
        backend:
          serviceName: kubia
          servicePort: 80
      - path: /bar                  # Requests to kubia.example.com/bar will be routed to the bar service
        backend:
          serviceName: bar
          servicePort: 80
```
* Ingress exposing mutliple services on different hosts
```YAML
 spec:
  rules:
  - host: foo.example.com          # Requests for foo.example.com will be routed to service foo
    http:
      paths:
      - path: /
        backend:
          serviceName: foo
          servicePort: 80
  - host: bar.example.com          # request for bar.example.com will be routed to service bar
    http:
      paths:
      - path: /
        backend:
          serviceName: bar
          servicePort: 80
```

