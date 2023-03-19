### Complete Application setup with Kubernetes Components

**Workflow of Application which needs to be designed**

![image](https://user-images.githubusercontent.com/52998083/224530110-a6a1fee2-509c-43ad-bfc7-c94b19c4ddd2.png)


**Interal working of designed application**

![image](https://user-images.githubusercontent.com/52998083/224530051-49f1a22a-23d6-4bb6-841f-78f4e11b946a.png)



## 1.Create Yaml for Secrets

Create yaml for secrets which would be storing MongoDB secrets in base64 crypted way and which would be used by MongoDB and MongoExpress Pods

1. You could use https://www.base64encode.org/ to encrypt your username and password in base64 format </br>


**yaml file for secrets**
```js
apiVersion: v1
kind: Secret
metadata:
  name:  mongodb-secret
type: Opaque  
data:
   mongo-root-username:  dXNlcm5hbWU=
   mongo-root-password:  cGFzc3dvcmQ=

```


![image](https://user-images.githubusercontent.com/52998083/226157094-44485c75-943c-48fa-addb-34d885e7426a.png)

![image](https://user-images.githubusercontent.com/52998083/226157122-a26d9ab1-a47d-4523-b561-319f5ec4913d.png)


2.Save the file as mongo-secret.yaml.</br>

3.Create the secrets within cluster by running the created mongo-secrets.yaml file using command **kubectl apply -f mongo-secret.yaml**

![image](https://user-images.githubusercontent.com/52998083/226157401-466b4291-c144-4dec-b9ff-58da02239149.png)

![image](https://user-images.githubusercontent.com/52998083/226157429-42da7764-05db-4e11-a742-6ea121a16e46.png)



## 2.Create Deployment.yaml for mongodb

1.Create yaml file for mongoDb deployment

```js

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
   app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
          image: mongo
          ports:
            - containerPort: 27017
          env:
            - name: MONGO_INITDB_ROOT_USERNAME
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-username
            - name: MONGO_INITDB_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-password

```

2. Save the file as mongo.yaml.

3.Create the deployment by running mongo.yaml file using command **kubectl apply -f mongo.yaml**

![image](https://user-images.githubusercontent.com/52998083/226159553-8f553d01-99aa-498e-8ee5-872734e6cba0.png

![image](https://user-images.githubusercontent.com/52998083/226159621-d7746170-d017-4641-8f98-db000ae5df9c.png)

![image](https://user-images.githubusercontent.com/52998083/226159654-70f0782e-f0de-4942-9351-80a7ed911b4d.png)



**Understanding mongo.yaml deployment file**

![image](https://user-images.githubusercontent.com/52998083/226159249-1df556b1-5632-4f84-b541-3ddbd73aeb15.png)


## 3.Create Internal Service for MongoDB so that other PODs/Service could communicate with MongoDB

1. Create yaml for mongodb-service

```js

apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
  - protocol: TCP
    port: 27017
    targetPort: 27017

```

2.We can keep the Service yml is same file as that deployment yaml. We just need to separate documents using **---**

3.Create the service by running mongo.yaml file using command **kubectl apply -f mongo.yaml**

![image](https://user-images.githubusercontent.com/52998083/226173375-362ff1b6-0c62-409d-878b-e7bff4c8945d.png)



**Understanding mongodb-service.yml file**

![image](https://user-images.githubusercontent.com/52998083/226173159-a1440e2c-23de-43b1-9acd-429dd90883e4.png)



## 4.Create ConfigMap for Storing DB Server URL










