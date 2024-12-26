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
###Создал манифесты для создания deployment и сервиса
[deployment.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/deployment.yaml)
[service.yaml](https://github.com/rufe69/2024_2025-introduction_to_distributed_technologies-k4110--bushmanov_k_a/blob/main/lab2/service.yaml)

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
