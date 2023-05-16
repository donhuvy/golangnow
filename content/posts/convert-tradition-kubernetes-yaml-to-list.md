---
categories:
  - Kubernetes
tags:
  - Kubernetes
title: "Convert Tradition Kubernetes Yaml to List"
date: 2023-04-28T16:49:45+07:00
draft: false
---

Tradition format

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: file-service-data
    namespace: web
spec:
    accessModes:
    - ReadWriteOnce
    resources:
        requests:
            storage: 4Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name:  file-service
  namespace: web
spec:
  selector:
    matchLabels:
      app: file-service
  replicas: 1
  template:
    metadata:
      labels:
        app: file-service
    spec:
      containers:
      - name:  file-service
        image:  web-app-file-service
        imagePullPolicy: Never
        ports:
        - containerPort:  8000
        env:
        - name: PORT
          value: "8000"
        # db
        - name: PG_URL
          valueFrom:
            secretKeyRef:
              name: files-service-pg-secret
              key: DB_URL
        # config
        - name: FILES_DIR
          value: /files

        # resource
        resources:
          requests:
            memory: 128Mi
            cpu: "0.1"
          limits:
            memory: 256Mi
            cpu: "0.1"
        # volumes
        volumeMounts:
          - mountPath: /files
            name: file-service-vol
      volumes:
        - name: file-service-vol
          persistentVolumeClaim:
            claimName: file-service-data
      restartPolicy: Always 

---
apiVersion: v1
kind: Service
metadata:
  name: file-service
  namespace: web
spec:
  selector:
    app: file-service
  type: ClusterIP
  ports:
  - port: 8000
    targetPort: 8000
```

Apply Kind List

```yaml
apiVersion: v1
kind: List
items:
  - apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: file-service-data
      namespace: web
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 4Gi
  - apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: file-service
      namespace: web
    spec:
      selector:
        matchLabels:
          app: file-service
      replicas: 1
      template:
        metadata:
          labels:
            app: file-service
        spec:
          containers:
            - name: file-service
              image: web-app-file-service
              imagePullPolicy: Never
              ports:
                - containerPort: 8000
              env:
                - name: PORT
                  value: "8000"
                # db
                - name: PG_URL
                  valueFrom:
                    secretKeyRef:
                      name: files-service-pg-secret
                      key: DB_URL
                # config
                - name: FILES_DIR
                  value: /files

              # resource
              resources:
                requests:
                  memory: 128Mi
                  cpu: "0.1"
                limits:
                  memory: 256Mi
                  cpu: "0.1"
              # volumes
              volumeMounts:
                - mountPath: /files
                  name: file-service-vol
          volumes:
            - name: file-service-vol
              persistentVolumeClaim:
                claimName: file-service-data
          restartPolicy: Always

  - apiVersion: v1
    kind: Service
    metadata:
      name: file-service
      namespace: web
    spec:
      selector:
        app: file-service
      type: ClusterIP
      ports:
        - port: 8000
          targetPort: 8000
```