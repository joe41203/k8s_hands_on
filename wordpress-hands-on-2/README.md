MySQL の Service と Deployment

```yaml
kind: Service
apiVersion: v1
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
    tier: mysql
spec:
  ports:
    - port: 3306
  selector:
    app: wordpress
    tier: mysql
  clusterIP: None

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: wordpress
        tier: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.6
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-pass
                  key: password.txt
          ports:
            - containerPort: 3360
              name: mysql
```

Wordpress の Service と Deployment

```yaml
kind: Service
apiVersion: v1
metadata:
  name: wordpress
  labels:
    app: wordpress
    tier: web
spec:
  selector:
    app: wordpress
    tier: web
  type: NodePort
  ports:
    - name: wordpress-port
      port: 80
      targetPort: 80
      nodePort: 30180

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: wordpress-deployment
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: wordpress
        tier: web
    spec:
      containers:
        - image: wordpress
          name: wordpress
          env:
            - name: WORDPRESS_DB_HOST
              value: wordpress-mysql:3306
            - name: WORDPRESS_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-pass
                  key: password.txt
          ports:
            - containerPort: 80
              name: wordpress
```
