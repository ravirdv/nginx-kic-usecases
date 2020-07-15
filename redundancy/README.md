# Redundancy and auto healing

- Get a list of NGINX+ pods running in nginx-ingress namespace.

```
# kubectl get pods -n nginx-ingress

Output:
NAME                  READY   STATUS    RESTARTS   AGE
nginx-ingress-6kdlb   1/1     Running   0          11d
nginx-ingress-95k5f   1/1     Running   0          7d15h
nginx-ingress-p6px4   1/1     Running   0          7d15h
```

- In above output, there are 3 KIC instances as we're running it as daemonset and this cluster has 3 nodes.

- Run following command to fire continuos HTTP request to this KIC:

```
   while true; do curl healthy-tea.appdeck.io/tea; sleep 0.2; done;
```

- Delete any ingress pod while requests are being sent, check that requests should not fail continuously.

```
# kubectl delete -n nginx-ingress pod/nginx-ingress-p6px4

