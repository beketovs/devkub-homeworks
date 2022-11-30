# Домашнее задание к занятию "12.4 Развертывание кластера на собственных серверах, лекция 2"
Новые проекты пошли стабильным потоком. Каждый проект требует себе несколько кластеров: под тесты и продуктив. Делать все руками — не вариант, поэтому стоит автоматизировать подготовку новых кластеров.

## Задание 1: Подготовить инвентарь kubespray
Новые тестовые кластеры требуют типичных простых настроек. Нужно подготовить инвентарь и проверить его работу. Требования к инвентарю:
* подготовка работы кластера из 5 нод: 1 мастер и 4 рабочие ноды;
* в качестве CRI — containerd;
* запуск etcd производить на мастере.

***
root@master1:/etc/kubernetes# kubectl get nodes
NAME      STATUS   ROLES           AGE   VERSION
master1   Ready    control-plane   26m   v1.25.4
node1     Ready    <none>          24m   v1.25.4
node2     Ready    <none>          24m   v1.25.4
node3     Ready    <none>          24m   v1.25.4
node4     Ready    <none>          24m   v1.25.4
  
Кубелет запущен с флагом --container-runtime-endpoint=unix:///var/run/containerd/containerd.sock
  
root@master1:/etc/kubernetes# systemctl status etcd
● etcd.service - etcd
     Loaded: loaded (/etc/systemd/system/etcd.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2022-11-30 00:06:47 UTC; 33min ago
   Main PID: 1061699 (etcd)
      Tasks: 11 (limit: 4612)
     Memory: 69.3M
     CGroup: /system.slice/etcd.service
             └─1061699 /usr/local/bin/etcd


***

## Задание 2 (*): подготовить и проверить инвентарь для кластера в AWS
Часть новых проектов хотят запускать на мощностях AWS. Требования похожи:
* разворачивать 5 нод: 1 мастер и 4 рабочие ноды;
* работать должны на минимально допустимых EC2 — t3.small.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
