University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2024/2025 
Group: K4112C  
Author: Baban Victoria  
Lab: lab4  
Date of create: 19.12.2024  
Date of finished: - 

**Лабораторная работа №2.**    
**Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса.**   
**Описание.**  
В данной лабораторной работе необходимо выполнить развертывание полноценного веб сервиса с несколькими репликами.   
**Цель работы.**  
Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомиться с сетевыми сервисами и развернуть свое веб приложение.   
**Ход работы.**    
Запускаем Minikube, используя команду ```minikube start```.  
Cоздаем [deployment.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/deployment.yaml) с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и передаем переменные в эти реплики: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME. 
И создаем [service.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/service.yaml) - сервис, через который будет доступ на эти "поды".  
Применяем созданные манифесты с помощью команд

```
minikube kubectl -- apply -f deployment.yaml
minikube kubectl -- apply -f service.yaml
```

![Apply](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/img/apply%20configurations.png)   
 
Проверяем добавление подов и сервиса с помощью команд 
```
kubectl get pods
kubectl get services
```
![Pods](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/img/pods.png) 
![Services](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/img/services.png)    
    
Далее пробрасываем порты для доступа к контейнерам через веб-браузер с помощью команды ```minikube kubectl -- port-forward service/frontend-service 55684:3000``` и открываем страницу http://localhost:55684 в браузере.
![web](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/img/web.png)   

Видим, что переменные REACT_APP_USERNAME, REACT_APP_COMPANY_NAME остаются неизменными. Это происходит потому, что они используются приложением для настройки окружения. 
А вот имя контейнера изменяется, т.к. оно уникально идентифицирует конкретный контейнер внутри кластера.

Проверяем логи контейнеров.  
![logs](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/img/logs.png)    
  
Схема организации контейнеров и сервисов представлена ниже.   
![Schema](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab2/img/lab2-schema.png)      
  

**Результаты лабораторной работы.**   
- Файлы с разработанными манифестами с расширением .yaml.  
- Схема организации контейеров и сервисов, нарисованная в draw.io.  
- Скриншоты c результатами работы.  
