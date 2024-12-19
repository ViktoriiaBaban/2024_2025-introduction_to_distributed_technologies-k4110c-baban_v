University: [ITMO University](https://itmo.ru/ru/)   
Faculty: [FICT](https://fict.itmo.ru)   
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)   
Year: 2024/2025    
Group: K4110c    
Author: Baban Victoria    
Lab: Lab3   
Date of create: 19.12.2024    
Date of finished:    

**Лабораторная работа №3.**    
**Сертификаты и "секреты" в Minikube, безопасное хранение данных.**   
**Описание.**    
В данной лабораторной работе необходимо познакомитесь с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.    

**Цель работы.**  
Познакомиться с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube. 

**Ход работы.**   
Создаем  configMap с переменными: REACT_APP_USERNAME, REACT_APP_COMPANY_NAME с помощью команды ```kubectl create configmap my-configmap --from-literal=REACT_APP_USERNAME="Victoria Baban" --from-literal=REACT_APP_COMPANY_NAME="Itmo University"```.

![create configMap](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/create%20configmap.png)    

Создаем [replicaset.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/replicaset.yaml) с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и применяем его ```kubectl apply -f replicaset.yaml```
 
![replicaset](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/apply%20replicaset.png)

Создаем [service.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/service.yaml) и применяем его ```kubectl apply -f service.yaml``` 

![services](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/apply%20service.png)

Далее используем команду ```minikube addons enable ingress```, которая позволит использовать Ingress.

![enable ingress](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/enable%20ingress.png)   
     
После установки OpenSSL, генерируем TLS-сертификат. 

![create tls](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/openssl%20crt.png)     

Создаем секрет командой - ```kubectl create secret tls victoria-baban-tls --cert=tls.crt --key=tls.key```.

![create secret](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/create%20secret.png)     

Создаем [ingress.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/ingress.yaml) и применяем его ```kubectl apply -f ingress.yaml``` 

![apply ingress](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/apply%20ingress.png)

Также, добавляем IP адрес ingress и FQDN, то есть ```127.0.0.1 victoria.baban.ru``` в hosts файл, который лежит по пути: ```C:\Windows\System32\drivers\etc```.
  
Запускаем команду ```minikube tunnel```.  

![tunnel](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/tunnel.png)      
    
В браузере переходим по FQDN-имени.  

![website](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/victoria.baban.ru%20site.png)      
 
Проверяем наличие сертификата. 

![certificate](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/certificate.png)      
 
Схема организации контейнеров и сервисов представлена ниже.   

![schema](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab3/images/lab3-schema.png)       


**Результаты лабораторной работы.**     
- Файлы с разработанными манифестами с расширением .yaml.  
- Схема организации контейеров и сервисов, нарисованная в draw.io.  
- Скриншоты c результатами работы.   
