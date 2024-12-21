University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2024/2025  
Group: K4110c  
Author: Baban Victoria
Lab: Lab4  
Date of create: 19.12.2024  
Date of finished: 21.12.2024

# Лабораторная работа №4 "Сети связи в Minikube, CNI и CoreDNS"

## Описание
Это последняя лабораторная работа в которой вы познакомитесь с сетями связи в Minikube. Особенность Kubernetes заключается в том, что у него одновременно работают underlay и overlay сети, а управление может быть организованно различными CNI.

## Цель работы
Познакомиться с CNI Calico и функцией IPAM Plugin, изучить особенности работы CNI и CoreDNS.

## Ход работы

При запуске minikube устанавливаем плагин `CNI=calico` и режим работы `Multi-Node Clusters` и разворачиваем 2 ноды командой ```minikube start --network-plugin=cni --cni=calico --nodes 2 -p multinode-demo```

![minikube start](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/start.png)

Проверяем, что запустились 2 Node с помощью команды `kubectl get nodes`.

![get_nodes](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/get-nodes.png)

Посмотрим Pod'ы с меткой **calico-node** с помощью команды ```kubectl get pods -l k8s-app=calico-node -A```, чтобы проверить работу CNI Calico.

![get_pods](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/get-pods-calico.png)

Назначим метки узлам, с помощью команд:

```
kubectl label nodes multinode-demo zone=ru-east  
kubectl label nodes multinode-demo-m02 zone=ru-west
```

![add labels](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/add-label.png)

Создадим манифест для **IPPool** ресурса [ippool.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/ippool.yaml), для назначения IP адресов.

Чтобы применить написанный манифест, необходимо сначала установить **calicoctl**. Для этого скачаем [config-файл](https://github.com/projectcalico/calico/blob/master/manifests/calicoctl.yaml) с официального репозитория и выполним команду: `kubectl create -f calicoctl.yaml`.

![calicoctl_created](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/create%20calicoctl.png)

Удалим IPPool по-умолчанию с помощью команды ```kubectl delete ippools default-ipv4-ippool```.

![delete_default_ippool](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/delete-default-ippool.png)

Применим написанный манифест и создадим собственные IPPool'ы. Т.к. ОС Windows создадим файл конфигурации [calico.cfg.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/calico.cfg.yaml) и будем прокидывать его с запросом.
```
calicoctl create -f ippool.yaml --allow-version-mismatch --config=calico.cfg.yaml
```

![create ippools](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/create%20ippool.png)

Проверяем, что появилось два pool'а:

![get ippools](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/get%20ippools.png)

Создаем и применяем манифесты для развертывания [deployment.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/deployment.yaml) и сервиса [service.yaml](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/service.yaml).

![create deployment and service](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/apply%20deployment%20and%20service.png)

Пробрасываем порт для подключения к сервису через браузер: `kubectl port-forward service/frontend-service 8080:80`.

Переходим по ссылке: `http://localhost:8080/`.

![browser](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/web.png)

Пингуем с контейнера `frontend-deployment-799f5c59dd-hs5h7` контейнеру с IP-адресом: `ping 192.168.0.71` с помощью команды:

```
kubectl exec -ti frontend-deployment-799f5c59dd-hs5h7 -- sh
```

![ping_1](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/ping%20192.168.0.71.png)

Пингуем с контейнера `frontend-deployment-799f5c59dd-9jczl` контейнеру с IP-адресом: `ping 192.168.1.194` с помощью команды:

```
kubectl exec -ti frontend-deployment-799f5c59dd-9jczl -- sh
```

![ping_2](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/ping%20192.168.1.194.png)

Схема организации контейнеров и сервисов представлена ниже.

![Схема](https://github.com/ViktoriiaBaban/2024_2025-introduction_to_distributed_technologies-k4110c-baban_v/blob/master/lab4/images/lab4-schema.png)
