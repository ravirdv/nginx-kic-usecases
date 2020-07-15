# Accessing KIC instance dashboard (NGINX+)


- Get a list of NGINX+ pods running in nginx-ingress namesapce.

```
# kubectl get pods -n nginx-ingress

Output:
NAME                  READY   STATUS    RESTARTS   AGE
nginx-ingress-6kdlb   1/1     Running   0          11d
nginx-ingress-95k5f   1/1     Running   0          7d15h
nginx-ingress-p6px4   1/1     Running   0          7d15h
```
- In above output, there are 3 KIC instances as we're running it as daemonset and this cluster has 3 nodes.

- NGINX Dashboard is running on port 8080 on path /dashboard.html

- Use kubectl port-forward command to map localhost port to port 8080 on KIC NGINX+ Instnace.

```
# kubectl port-forward -n nginx-ingress pod/nginx-ingress-95k5f 8080

Output:
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

Dashboard can be access at http://127.0.0.1:8080/dashboard.html
