University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2024/2025
Group: K4110c
Author: Bushmanov Kirill Alekseevich
Lab: Lab4
Date of create: 22.12.2024
Date of finished: 

# Лабораторная работа №4

### Запустил minikube в режиме Multi-Node Clusters с CNI плагином calico и Проверил работу CNI плагина Calico и количество нод
```bash
minikube start --network-plugin=cni --cni=calico --nodes=2
```

![Запуск minikube и calico](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/1.png "")

![Проверка подов](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/2.png "")

### Промаркировал ноды по географическому признаку 
```bash
minikube kubectl -- label nodes minikube zone=west
minikube kubectl -- label nodes minikube-m02 zone=east
```

![Маркировка нод](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/3.png "")

### Разработал манифест для Calico
[ippools.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/ippools.yaml)
```yaml
apiVersion: crd.projectcalico.org/v1
kind: IPPool
metadata:
  name: west-ippool
spec:
  cidr: 192.168.0.0/24
  ipipMode: Always
  natOutgoing: true
  nodeSelector: zone == "west"

---
apiVersion: crd.projectcalico.org/v1
kind: IPPool
metadata:
  name: east-ippool
spec:
  cidr: 192.168.1.0/24
  ipipMode: Always
  natOutgoing: true
  nodeSelector: zone == "east"
```

### Удалил дефолтный ippool

```bash
kubectl -- delete ippools default-ipv4-ippool
```

![Удаление дефолтного ippool](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/4.png "")

### Применил манифест 
```bash
kubectl apply -f ipools.yaml
```

![Применение манифеста](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/5.png "")

### Разработал deployment и добавил сервис
[deployment.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/deployment.yaml)

```yaml
apiVersion: apps/v1
kind: Deployment
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
          env:
            - name: REACT_APP_USERNAME
              value: "test_username"
            - name: REACT_APP_COMPANY_NAME
              value: "test_company"
```

![Применение deployment](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/6.png "")

![Добавление сервиса](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/7.png "")

### Сделал проброс и зашел на `localhost:3000` в браузере
```bash
kubectl port-forward service/frontend-service 3000:3000
```

![Проброс](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/8.png "")

![Браузер](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/9.png "")

###  Пинганул поды
![Пинг подов](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab4/Screenshots/10.png "")
