University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2024/2025
Group: K4110c
Author: Bushmanov Kirill Alekseevich
Lab: Lab3
Date of create: 22.12.2024
Date of finished: 

# Лабораторная работа №3

### Сделал pull для образа ifilyaninitmo/itdt-contained-frontend:master

### Включил Ingress
```bash
minikube addons enable ingress
```

![Включение Ingress](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/Screenshots/1.png "")

### Сгенерировал TLS сертификат через OpenSSL 
![Генерация сертификата](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/Screenshots/2.png "")

[Ключ](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/key.key)

[Сертификат](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/cert.crt)

### Создал секрет с сертификатом
![Создание секретов](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/Screenshots/3.png "")

### Создал манифест с созданием ConfigMap, ReplicaSet, Service и Ingress
[config.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/config.yaml)
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: itdt-contained-frontend-config
data:
  REACT_APP_USERNAME: "test_username"
  REACT_APP_COMPANY_NAME: "test_company_name"

---

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: itdt-contained-frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: itdt-contained-frontend
  template:
    metadata:
      labels:
        app: itdt-contained-frontend
    spec:
      containers:
      - name: frontend
        image: ifilyaninitmo/itdt-contained-frontend:master
        ports:
        - containerPort: 3000
        envFrom:
        - configMapRef:
            name: itdt-contained-frontend-config

---

apiVersion: v1
kind: Service
metadata:
  name:  itdt-contained-frontend-service
spec:
  type: ClusterIP
  ports:
  - port: 3000
  selector:
    app: itdt-contained-frontend

---

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: itdt-contained-frontend-ingress
spec:
  ingressClassName: nginx
  tls:
    - hosts: 
      - bushmanov.com
      secretName: test-tls
  rules:
    - host: bushmanov.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service: 
                name: itdt-contained-frontend-service
                port:
                  number: 3000
```

```bash
kubectl apply -f config.yaml
```

![apply config](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/Screenshots/4.png "")

### Добавил запись localhost bushmanov.com в etc/hosts

### Выполнил команду minikube tunnel

### Посмотрел сертфиикат в браузере по адресу bushmanov.com

![Сертификат](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab3/Screenshots/5.png "")

