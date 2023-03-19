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








