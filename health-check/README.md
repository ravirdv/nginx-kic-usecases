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

- On the N+ KIC Dashboard, locate the ingress table for the **tea-health-svc**. The table should be titled *default-tea-ingress-hc-healthy-tea.appdeck.io-tea-health-svc-80*
- Notice in the table, three health check server entries appear, displaying the results of 3 active health checks containerized in Kuberenetes pods.

- Next, tail the status log of all the Healthcheck service pods for the Tea application:
```
➜  ~ kubectl get po --selector=app=tea-healthcheck -w -n default
NAME                               READY   STATUS    RESTARTS   AGE
...
tea-healthcheck-5d7ccccf4c-78p2m   1/1     Running   0          7d16h
tea-healthcheck-5d7ccccf4c-9zjt7   1/1     Running   3          7d15h
tea-healthcheck-5d7ccccf4c-xbrs6   1/1     Running   0          7d16h
...
```

- Get interactive shell access to any of the above three pods, and kill nginx process to simulate a failed app.
```
kubectl exec -it tea-healthcheck-5d7ccccf4c-9zjt7 -- sh
pkill nginx
```

- Observe from the status log of the Healthcheck service pods, this pod is marked down instantly and upon process restart, its added back to the upstream.
- Observe the disapperance of one entry in the ingress table for the **tea-health-svc**, followed by the appearance of that entry after several seconds.
