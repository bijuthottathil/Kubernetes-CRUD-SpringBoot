# springboot-crud-k8s
Run &amp; Deploy Spring Boot CRUD Application With MySQL on K8S


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

docker build -t springboot-crud-k8s:1.0 .



