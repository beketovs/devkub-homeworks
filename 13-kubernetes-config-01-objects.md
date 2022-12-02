# Домашнее задание к занятию "13.1 контейнеры, поды, deployment, statefulset, services, endpoints"
Настроив кластер, подготовьте приложение к запуску в нём. Приложение стандартное: бекенд, фронтенд, база данных. Его можно найти в папке 13-kubernetes-config.

## Задание 1: подготовить тестовый конфиг для запуска приложения
Для начала следует подготовить запуск приложения в stage окружении с простыми настройками. Требования:
* под содержит в себе 2 контейнера — фронтенд, бекенд;
* регулируется с помощью deployment фронтенд и бекенд;
* база данных — через statefulset.

***
[stage.yaml](https://github.com/beketovs/devkub-homeworks/blob/main/13-kubernetes-config/stage.yaml)

```
root@master1:/home/beketov/mymanifests# k get pods
NAME                          READY   STATUS    RESTARTS   AGE
front-back-6fd764f75d-4jrcx   2/2     Running   0          46s
postgres-0                    1/1     Running   0          46s
root@master1:/home/beketov/mymanifests# k get svc
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
front-svc    NodePort    10.233.40.125   <none>        80:30080/TCP   48s
kubernetes   ClusterIP   10.233.0.1      <none>        443/TCP        47h
root@master1:/home/beketov/mymanifests# k get statefulsets.apps 
NAME       READY   AGE
postgres   1/1     56s
```
***
[prod.yaml](https://github.com/beketovs/devkub-homeworks/blob/main/13-kubernetes-config/prod.yaml)
## Задание 2: подготовить конфиг для production окружения
Следующим шагом будет запуск приложения в production окружении. Требования сложнее:
* каждый компонент (база, бекенд, фронтенд) запускаются в своем поде, регулируются отдельными deployment’ами;
* для связи используются service (у каждого компонента свой);
* в окружении фронта прописан адрес сервиса бекенда;
* в окружении бекенда прописан адрес сервиса базы данных.

***

```
root@master1:/home/beketov/mymanifests# k get pods
NAME                     READY   STATUS    RESTARTS   AGE
back-655c6949bb-rzmpq    1/1     Running   0          43s
front-685c5db79b-ghlhx   1/1     Running   0          43s
postgres-0               1/1     Running   0          43s
root@master1:/home/beketov/mymanifests# k get svc
NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
back-svc       ClusterIP   10.233.34.139   <none>        9000/TCP       49s
front-svc      NodePort    10.233.43.56    <none>        80:30080/TCP   49s
kubernetes     ClusterIP   10.233.0.1      <none>        443/TCP        2d
postgres-svc   ClusterIP   10.233.1.185    <none>        5432/TCP       49s
```
***

## Задание 3 (*): добавить endpoint на внешний ресурс api
Приложению потребовалось внешнее api, и для его использования лучше добавить endpoint в кластер, направленный на это api. Требования:
* добавлен endpoint до внешнего api (например, геокодер).

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

В качестве решения прикрепите к ДЗ конфиг файлы для деплоя. Прикрепите скриншоты вывода команды kubectl со списком запущенных объектов каждого типа (pods, deployments, statefulset, service) или скриншот из самого Kubernetes, что сервисы подняты и работают.

---
