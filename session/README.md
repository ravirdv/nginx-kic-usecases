# Session Persistence

- In this demo we are using below NGINX+ KIC Ingress annotation for performing session persistence.

```
nginx.com/sticky-cookie-services
```

- This injects a cookie to route requests to corrent pod.

- Open this page in browser : http://session.appdeck.io and refresh it, nice that server ID remains same.

- Open the same page in different browser or in incognito mode, check that server ID is different.