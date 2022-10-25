# Домашнее задание к занятию "12.2 Команды для работы с Kubernetes"
Кластер — это сложная система, с которой крайне редко работает один человек. Квалифицированный devops умеет наладить работу всей команды, занимающейся каким-либо сервисом.
После знакомства с кластером вас попросили выдать доступ нескольким разработчикам. Помимо этого требуется служебный аккаунт для просмотра логов.

## Задание 1: Запуск пода из образа в деплойменте
Для начала следует разобраться с прямым запуском приложений из консоли. Такой подход поможет быстро развернуть инструменты отладки в кластере. Требуется запустить деплоймент на основе образа из hello world уже через deployment. Сразу стоит запустить 2 копии приложения (replicas=2). 

Требования:
 * пример из hello world запущен в качестве deployment
 * количество реплик в deployment установлено в 2
 * наличие deployment можно проверить командой kubectl get deployment
 * наличие подов можно проверить командой kubectl get pods

***
```
beketov@beketovs-MacBook-Pro ~ % k get deployments.apps            
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   2/2     2            2           63s
beketov@beketovs-MacBook-Pro ~ % k get pods
NAME                         READY   STATUS    RESTARTS   AGE
hello-node-697897c86-k8bm5   1/1     Running   0          7s
hello-node-697897c86-ssvgg   1/1     Running   0          68s
```
***


## Задание 2: Просмотр логов для разработки
Разработчикам крайне важно получать обратную связь от штатно работающего приложения и, еще важнее, об ошибках в его работе. 
Требуется создать пользователя и выдать ему доступ на чтение конфигурации и логов подов в app-namespace.

Требования: 
 * создан новый токен доступа для пользователя
 * пользователь прописан в локальный конфиг (~/.kube/config, блок users)
 * пользователь может просматривать логи подов и их конфигурацию (kubectl logs pod <pod_id>, kubectl describe pod <pod_id>)


***
```
beketov@beketovs-MacBook-Pro ~ % kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /Users/beketov/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Tue, 25 Oct 2022 19:06:30 +04
        provider: minikube.sigs.k8s.io
        version: v1.27.1
      name: cluster_info
    server: https://127.0.0.1:51436
  name: minikube
contexts:
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Tue, 25 Oct 2022 19:06:30 +04
        provider: minikube.sigs.k8s.io
        version: v1.27.1
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /Users/beketov/.minikube/profiles/minikube/client.crt
    client-key: /Users/beketov/.minikube/profiles/minikube/client.key
```
***

## Задание 3: Изменение количества реплик 
Поработав с приложением, вы получили запрос на увеличение количества реплик приложения для нагрузки. Необходимо изменить запущенный deployment, увеличив количество реплик до 5. Посмотрите статус запущенных подов после увеличения реплик. 

Требования:
 * в deployment из задания 1 изменено количество реплик на 5
 * проверить что все поды перешли в статус running (kubectl get pods)

***
```
beketov@beketovs-MacBook-Pro ~ % k get pods
NAME                         READY   STATUS    RESTARTS   AGE
hello-node-697897c86-bcdd4   1/1     Running   0          3s
hello-node-697897c86-gk4lk   1/1     Running   0          3s
hello-node-697897c86-k8bm5   1/1     Running   0          2m48s
hello-node-697897c86-ssvgg   1/1     Running   0          3m49s
hello-node-697897c86-t7xpn   1/1     Running   0          3s
```
***

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
