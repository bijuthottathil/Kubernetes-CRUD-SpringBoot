# springboot-crud-k8s
Run  Deploy Spring Boot CRUD Application With MySQL on K8S
![image](https://github.com/user-attachments/assets/f560b005-883c-4c03-936f-f59389420b0e)


Minikube started

![image](https://github.com/user-attachments/assets/1d431735-e117-4479-94f4-4239bcd39cbd)

Executed   eval $(minikube docker-env)


First we will deploy  dbdeploymendb.yaml

![image](https://github.com/user-attachments/assets/0d242265-cea2-4c29-92b7-02c3e54fe64d)


 kubectl apply -f db-deployment.yaml 

![image](https://github.com/user-attachments/assets/e1f2943b-0469-4165-9da8-fa19efec1ba1)

![image](https://github.com/user-attachments/assets/635ad902-2e63-4939-b525-4b9d302043c0)


Next step, will connect mysql pod and check schema strcucture

kubectl exec -it mysql-545ffd8b7d-hp8bv -- /bin/bash

I am able to login   mysql -h mysql -u root -p


![image](https://github.com/user-attachments/assets/1abced38-65d5-46de-a8a2-aa500aabfe9c)

mysql> show databases
    -> ;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mydatabase         |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.01 sec)


Database is ready, now I will switch to Spring Boot app configuration


![image](https://github.com/user-attachments/assets/465925a2-9c0a-4aee-a55c-05a095f16e22)

compiled jar is available here



![image](https://github.com/user-attachments/assets/9128bc7b-d589-4f26-a4b4-568884bd46ed)

Now we have to create dockerfile and build it

![image](https://github.com/user-attachments/assets/62b5f191-c16c-4cbe-9dcc-c307052df8ae)

docker build -t springboot-crud-k8s:1.0 .  ( later I found issue and used docker tag springboot-crud-k8s:1.0 bijuthottathil/springboot-crud-k8s:1.0  and docker push bijuthottathil/springboot-crud-k8s:1.0) (bijuthottathil is my git userid)

![image](https://github.com/user-attachments/assets/6d2fab42-7eaf-4c7b-8a82-2e2d7d1c487e)

D:\springboot-crud-k8s>docker images
REPOSITORY                    TAG       IMAGE ID       CREATED              SIZE
springboot-crud-k8s           1.0       d26a340f9903   About a minute ago   877MB
gcr.io/k8s-minikube/kicbase   v0.0.46   cef9f3c2e399   3 months ago         1.86GB
gcr.io/k8s-minikube/kicbase   <none>    fd2d445ddcc3   3 months ago         1.86GB

Now we need to create yaml file for service and app deployment
![image](https://github.com/user-attachments/assets/0158813e-69b5-4a9a-a4ac-35ec9460e4fc)


![image](https://github.com/user-attachments/assets/53b4c3cf-641f-45ac-9496-52509d6cef97)



![image](https://github.com/user-attachments/assets/b953e23b-d4ae-4d8e-bec8-eb376bb7ce54)

![image](https://github.com/user-attachments/assets/73be2830-d0a1-4b99-98cc-5ed69f5c0a1d)

PODs are not running. You can review this issue in Minikube Dashboard. Now we need to fix this issue

![image](https://github.com/user-attachments/assets/11d06fb1-0cc2-41a1-8606-c57e2f28ca4b)


Issue resolved after pushing image with my gituser name and modified deployment yaml with following reference

   image: bijuthottathil/springboot-crud-k8s:1.0

   docker build -t springboot-crud-k8s:1.0 .
    docker tag springboot-crud-k8s:1.0 bijuthottathil/springboot-crud-k8s:1.0
    docker push bijuthottathil/springboot-crud-k8s:1.0

   ![image](https://github.com/user-attachments/assets/a468438f-753f-4945-a009-1efc2b676d68)


All pods are running now

![image](https://github.com/user-attachments/assets/9f960a57-413e-4175-923a-6c94d9543991)


I can see table created 


![image](https://github.com/user-attachments/assets/795160b4-c527-4c61-ab03-19310c038f40)

![image](https://github.com/user-attachments/assets/d3a84fcb-7ed5-4e6f-8492-a482aab64d05)

D:\springboot-crud-k8s>minikube ip
192.168.49.2

D:\springboot-crud-k8s>kubectl get svc
NAME                  TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
kubernetes            ClusterIP   10.96.0.1      <none>        443/TCP          72m
mysql                 ClusterIP   None           <none>        3306/TCP         71m
springboot-crud-svc   NodePort    10.108.89.57   <none>        8080:31690/TCP   42m

![image](https://github.com/user-attachments/assets/e225a938-071d-4fe5-9648-edcdee79fd55)


I port forwarded service to 8080:8080


![image](https://github.com/user-attachments/assets/0e3d6ee7-db95-44ca-a329-ba9792b8c062)


Now I got this page

![image](https://github.com/user-attachments/assets/ad91d09d-ce88-46b6-920a-f2afc092055e)

Service is running. Now I need call API using postman or any other tools to pass json value and test

Using postman, I submitted my request with data

{
  "name":"watch",
   "qty":2,
   "price":3450
}

![image](https://github.com/user-attachments/assets/174608ff-cc8e-41d6-9430-1fe7f59d3d9b)


Navigaged to My SQL. You can see results

![image](https://github.com/user-attachments/assets/b92910df-6820-4322-90d7-5b705279bede)

Posted 3 records so far

![image](https://github.com/user-attachments/assets/4c049aa5-0a57-4419-96bb-78305e9d07b1)




![image](https://github.com/user-attachments/assets/92a5f286-ad8f-43a3-ab29-36936219b35a)

Update is working

![image](https://github.com/user-attachments/assets/ba1eb27e-1306-4ed8-a18c-057553cfb34e)


![image](https://github.com/user-attachments/assets/c6b73e3f-03de-40c3-9849-c2ce4ad01758)

Finally sharing Minikude Dashboard screenshots

![image](https://github.com/user-attachments/assets/4f0240c3-2ded-45b9-8bd2-7586e2ea01db)

![image](https://github.com/user-attachments/assets/bee69166-a350-4402-ab2e-9154f540ab14)


![image](https://github.com/user-attachments/assets/33051b79-5649-485d-91da-47a072c32458)


![image](https://github.com/user-attachments/assets/0dc11593-0c4c-4a0e-9cde-e92544887d56)

![image](https://github.com/user-attachments/assets/7b2e4cc6-3ff2-4f9d-8063-3f7af6f29446)

![image](https://github.com/user-attachments/assets/db7f9497-a847-494d-b4f1-5fc0a0521ed5)


