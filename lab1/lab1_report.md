University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2024/2025
Group: K4110c
Author: Bushmanov Kirill Alekseevich
Lab: Lab1
Date of create: 22.12.2024
Date of finished: 

# Лабораторная работа №1

### Установил докер (Docker Desktop)

### Добавил поддержку kubernetes

### Сделал pull для образа vault 

### Создал манифест для создания пода 
[test.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab1/test.yaml)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: vault
  labels:
    app: vault
spec:
  containers:
    - name: vault
      image: hashicorp/vault:latest
      ports:  
        - containerPort: 8200  
```

### Развернул под
```bash
    kubectl apply -f test.yaml
```
![Развернул под](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab1/Screenshots/1.png "")

### Посмотрел логи и нашел там root token 
```bash
kubectl logs -f vault)
``` 
(root token: hvs.mFeRO2DOdtBq3WackbWqc66Q)
![Посмотрел логи](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab1/Screenshots/2.png "")

### Создал сервис для пода 
```bash
kubectl expose pod vault --type=NodePort --port=8200)
```
### Пробросил порт хоста в порт сервиса 
```
kubectl port-forward service/vault 8200:8200
```
![Пробросил порт](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab1/Screenshots/3.png "")

### Зашел по адресу `http://localhost:8200/` и проверил что всё работает
![Интерфейс входа vault](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab1/Screenshots/4.png "")

### Залогинился с помощью токена и зашел в систему
![Интерфейс vault](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab1/Screenshots/5.png "")
