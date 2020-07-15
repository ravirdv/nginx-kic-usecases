# Active Health Check (NGINX+)


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

```
# kubectl port-forward -n nginx-ingress pod/nginx-ingress-95k5f 8080

Output:
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

Dashboard can be access at http://127.0.0.1:8080/dashboard.html

- On Dashboard, check ingress table for "tea-ingress-hc", you should be able to see health checks being performed on 3 pods.

```
➜  ~ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
...
tea-healthcheck-5d7ccccf4c-78p2m   1/1     Running   0          7d16h
tea-healthcheck-5d7ccccf4c-9zjt7   1/1     Running   3          7d15h
tea-healthcheck-5d7ccccf4c-xbrs6   1/1     Running   0          7d16h
...
```

- Get interactive shell access to any of the above three pods, and kill nginx process to simulate a failed app.
```
kubectl exec -it tea-healthcheck-5d7ccccf4c-9zjt7 sh
pkill nginx
```

- Observe that this pod is marked down instantly and upon process restart, its added back to the upstream.
