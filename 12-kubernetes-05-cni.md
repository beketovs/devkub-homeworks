# Домашнее задание к занятию "12.5 Сетевые решения CNI"
После работы с Flannel появилась необходимость обеспечить безопасность для приложения. Для этого лучше всего подойдет Calico.
## Задание 1: установить в кластер CNI плагин Calico
Для проверки других сетевых решений стоит поставить отличный от Flannel плагин — например, Calico. Требования: 
* установка производится через ansible/kubespray;
* после применения следует настроить политику доступа к hello-world извне. Инструкции [kubernetes.io](https://kubernetes.io/docs/concepts/services-networking/network-policies/), [Calico](https://docs.projectcalico.org/about/about-network-policy)

***
```
root@master1:/etc/kubernetes# k get -n=kube-system pods | grep calico
calico-kube-controllers-d6484b75c-f5pjn   1/1     Running   0          35m
calico-node-4vl2w                         1/1     Running   0          36m
calico-node-6775j                         1/1     Running   0          36m
calico-node-cbjqh                         1/1     Running   0          36m
calico-node-mr7m2                         1/1     Running   0          36m
calico-node-zgf88                         1/1     Running   0          36m
```
***

## Задание 2: изучить, что запущено по умолчанию
Самый простой способ — проверить командой calicoctl get <type>. Для проверки стоит получить список нод, ipPool и profile.
Требования: 
* установить утилиту calicoctl;
* получить 3 вышеописанных типа в консоли.

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
