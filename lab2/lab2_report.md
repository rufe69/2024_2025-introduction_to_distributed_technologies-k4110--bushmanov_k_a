University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2024/2025
Group: K4110c
Author: Bushmanov Kirill Alekseevich
Lab: Lab2
Date of create: 22.12.2024
Date of finished: 


# Лабораторная работа №2

### Сделал pull для образа ifilyaninitmo/itdt-contained-frontend

### Создал манифесты для создания deployment и сервиса
[deployment.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/deployment.yaml)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: ifilyaninitmo/itdt-contained-frontend:master
        env:
        - name: REACT_APP_USERNAME
          value: "test_username"
        - name: REACT_APP_COMPANY_NAME
          value: "test_company" 
        ports:
        - containerPort: 86
```

[service.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/service.yaml)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
spec:
  type: NodePort
  ports:
  - port: 87
    nodePort: 31000
    targetPort: 3000
  selector:
    app: frontend
```

### Развернул deployment 
```bash
kubectl apply -f deployment.yaml
```

### Развернул сервис 
```bash
kubectl apply -f service.yaml
```

### Проверил, что поды и сервис работают
```bash
kubectl get pods
kubectl get services
```
![Развернутые поды и сервис](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/Screenshots/1.png "")

### Сделал проброс порта 8585 хоста на порт 87 сервиса 
```bash
kubectl port-forward service/frontend-service 8585:87)
```
![Проброс](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/Screenshots/2.png "")

### Зашел по адресу `http://localhost:8585/` и проверил что всё работает и значение переменных среды отображаются на странице
![Скриншот сайта](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/Screenshots/3.png "")

### При обновлении страницы ничего не изменялось

### Зашел в логи контейнеров и сделал их скриншот
![Логи](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/Screenshots/4.png "")
