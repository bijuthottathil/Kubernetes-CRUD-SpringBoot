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
