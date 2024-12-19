University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)   
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)   
Year: 2024/2025  
Group: K4110c  
Author: Baban Victoria  
Lab: Lab1  
Date of create: 19.12.2024  
Date of finished:   

**Лабораторная работа №1.  
Установка Docker и Minikube, мой первый манифест.**  
**Описание.**    
Это первая лабораторная работа, в которой тестируется Docker, устанавливается Minikube и реализуется первый "под".  
**Цель работы.**    
Цель лабораторной работы заключается в ознакомлении с инструментами Minikube и Docker, реализации своего первого "пода".  
**Ход работы.**  
После установки Docker и Minikube на рабочий компьютер был развернут minikube cluster с помощью команды ```minikube start```. 
 
Был создан файл [vault-pod.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab1/vault-pod.yaml) для развертывания пода с образом HashiCorp Vault.  

Применяем манифест, используя команду ```kubectl apply -f vault-pod.yaml```. И делаем кластер доступным снаружи с помощью ```minikube kubectl -- expose pod vault --type=NodePort --port=8200```

![Apply and expose pod](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab1/img/expose%20pod.png)  

Проверяем  появление Pod, используя команду ```minikube kubectl -- get pods```.  И прокидываем порт компьютера в контейнер с помощью команды ```minikube kubectl -- port-forward service/vault 8200:8200```

![Get pods and port forward](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab1/img/port-forward.png)

Теперь можно зайти в Vault по ссылке http://localhost:8200. С помощью команды ```minikube kubectl -- logs service/vault``` находим токен для авторизации (в строке Root Token) и вводим его.

![Vault auth page](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab1/img/vault-auth.png)

Произведена успешная авторизация. 
![Vault dashboard](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab1/img/vault-dashboard.png)
 
Останавливаем Minikube с помощью команды minikube stop.  

Схема организации контейнеров и сервисов представлена ниже.  
![Schema](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab1/img/lab1-schema.png)    

  
**Результат данной работы:**  
- Файл с разработанным манифестом для развертывания "пода" с расширением .yaml.  
- Схема организации контейеров и сервисов, нарисованная в draw.io.  
- Скриншоты c результатами работы.  
