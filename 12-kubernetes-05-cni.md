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

Без Network Policy:

root@master1:~/kubernetes-for-beginners/# k get deployments.apps | grep hello
hello-world   2/2     2            2           27m

root@master1:~/kubernetes-for-beginners/# k get service  | grep example
example-service   NodePort    10.233.16.5     <none>        8080:31485/TCP   2m10s

# Примечание (не прописывал в DNS):
# master1 - 10.0.0.237
# node1 - 10.0.0.200 

beketov@beketovs-MacBook-Pro repo_netology % curl http://10.0.0.237:31485
Hello Kubernetes!

beketov@beketovs-MacBook-Pro repo_netology % curl http://10.0.0.200:31485
Hello Kubernetes!

C применением Network Policy:

root@master1:~/manifests# k get networkpolicies.networking.k8s.io 
NAME        POD-SELECTOR                AGE
my-policy   run=load-balancer-example   49s
root@master1:~/manifests# cat network_policy.yaml 
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: load-balancer-example
  policyTypes:
    - Ingress

root@master1:~/manifests# k get networkpolicies.networking.k8s.io 
NAME        POD-SELECTOR                AGE
my-policy   run=load-balancer-example   49s

beketov@beketovs-MacBook-Pro repo_netology % curl http://10.0.0.237:31485
curl: (28) Failed to connect to 10.0.0.237 port 31485 after 75005 ms: Operation timed out

beketov@beketovs-MacBook-Pro repo_netology % curl http://10.0.0.200:31485
curl: (28) Failed to connect to 10.0.0.200 port 31485 after 75007 ms: Operation timed out

```



***

## Задание 2: изучить, что запущено по умолчанию
Самый простой способ — проверить командой calicoctl get <type>. Для проверки стоит получить список нод, ipPool и profile.
Требования: 
* установить утилиту calicoctl;
* получить 3 вышеописанных типа в консоли.
  
***
```
root@master1:~/manifests# sudo calicoctl get nodes
NAME      
master1   
node1     
node2     
node3     
node4 
  
root@master1:~/manifests# sudo calicoctl get ipPool
NAME           CIDR             SELECTOR   
default-pool   10.233.64.0/18   all()  
  
root@master1:~/manifests# sudo calicoctl get profile 
NAME                                                 
projectcalico-default-allow                          
kns.default                                          
kns.kube-node-lease                                  
kns.kube-public                                      
kns.kube-system                                      
ksa.default.default                                  
ksa.kube-node-lease.default                          
ksa.kube-public.default                              
ksa.kube-system.attachdetach-controller              
ksa.kube-system.bootstrap-signer                     
ksa.kube-system.calico-kube-controllers              
ksa.kube-system.calico-node                          
ksa.kube-system.certificate-controller               
ksa.kube-system.clusterrole-aggregation-controller   
ksa.kube-system.coredns                              
ksa.kube-system.cronjob-controller                   
ksa.kube-system.daemon-set-controller                
ksa.kube-system.default                              
ksa.kube-system.deployment-controller                
ksa.kube-system.disruption-controller                
ksa.kube-system.dns-autoscaler                       
ksa.kube-system.endpoint-controller                  
ksa.kube-system.endpointslice-controller             
ksa.kube-system.endpointslicemirroring-controller    
ksa.kube-system.ephemeral-volume-controller          
ksa.kube-system.expand-controller                    
ksa.kube-system.generic-garbage-collector            
ksa.kube-system.horizontal-pod-autoscaler            
ksa.kube-system.job-controller                       
ksa.kube-system.kube-proxy                           
ksa.kube-system.namespace-controller                 
ksa.kube-system.node-controller                      
ksa.kube-system.nodelocaldns                         
ksa.kube-system.persistent-volume-binder             
ksa.kube-system.pod-garbage-collector                
ksa.kube-system.pv-protection-controller             
ksa.kube-system.pvc-protection-controller            
ksa.kube-system.replicaset-controller                
ksa.kube-system.replication-controller               
ksa.kube-system.resourcequota-controller             
ksa.kube-system.root-ca-cert-publisher               
ksa.kube-system.service-account-controller           
ksa.kube-system.service-controller                   
ksa.kube-system.statefulset-controller               
ksa.kube-system.token-cleaner                        
ksa.kube-system.ttl-after-finished-controller        
ksa.kube-system.ttl-controller                       

  
  
```
***

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
