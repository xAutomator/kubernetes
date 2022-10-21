```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: amass
spec:
  replicas: 2
  selector:
    matchLabels:
      app: amass
  template:
    metadata:
      labels:
        app: amass
    spec:
      containers:
      - name: amass
        envFrom:
          - secretRef:
              name: elastic-creds
        imagePullPolicy: Always
        image: farcast22/amass:latest
```