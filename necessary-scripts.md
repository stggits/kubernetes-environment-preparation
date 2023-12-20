## Create a deployment
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-dpl
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-dpl
  template:
    metadata:
      labels:
        app: nginx-dpl
    spec:
      containers:
      - name: nginx-dpl
        image: nginx
        resources:
          limits:
            memory: "256Mi"
            cpu: "500m"
        ports:
        - containerPort: 80
```

## Create a service
```yml
apiVersion: v1
kind: Service
metadata:
  name: nginx-svc
spec:
  selector:
    app: nginx-dpl
  ports:
  - port: 80
    targetPort: 80
```

## Create an ingress
```yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
 name: nginx-dpl
 annotations:
   nginx.ingress.kubernetes.io/add-base-url: "true"
   nginx.ingress.kubernetes.io/rewrite-target: /
spec:
 ingressClassName: nginx
 rules:
   - host: afsk8s.local
     http:
       paths:
         - pathType: Prefix
           backend:
            service:
              name: nginx-svc
              port:
                number: 80
           path: /
```