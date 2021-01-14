## Introcução
````
     - 

````

## Objetivo
````
     - Orquestrar os containers de forma escalavel conforme o desempenho imposto pela maquina virtual.
     - Reiniciando aplicações altomaticamente em casos de falhas.
````
##  A arquitetura do Kubernetes

#### Master
````
     - Gerenciar o cluster
     - Manter e atualizar o estado desejado dos POD's
     - Receber e executar novos comandos
````

#### Node
````
      - Executar as aplicações
      - Administra os POD's que por sua vez manipula os CONTAINES
````


## Instalando no Linux
##### K8S
````
      - sudo apt-get install curl -y
      - curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes             release/release/stable.txt)/bin/linux/amd64/kubectl"
      - chmod +x ./kubectl
      - sudo mv ./kubectl /usr/local/bin/kubectl
````

##### Minikube
````
      - curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.12.1/minikube-linux-amd64 \ && chmod +x minikube
      - sudo install minikube /usr/local/bin/
````

### Exec Linux
Iniciando minikube
____
     - minikube start --vm-driver=virtualbox
#### K8S POD's
Concepts:
_____
     ° É um recurso que encapsular um ou mais container no k8s
     ° Se todos os containers no POD's pararem o POD parar.
        - Containers
            ° Como possuem IP's diferentes, containers em pods diferentes podem utilizar o mesmo número de porta.
            ° Containers dentro de um mesmo pod conseguem se comunicar via localhost.

Command:
_____
````
    ° Criando POD's utilizando imagens
        - kubectl run nginx-pod ---image=nginx:latest
        - kubectl run <NOME_POD> ---image=<NOME_IMAGE>
    ° Criando POD's Declarativa
        - kubectl apply -f pod.yaml
        - kubectl apply -f <FILE_NAME>
    ° Consultando POD's
        - kubectl get pods
        - kubectl get pods --watch
    ° Consultando descrição do POD
        - kubectl describe pod nginx-pod
        - kubectl describe pod <NOME_POD>
    ° Editando os POD's
        - kubectl edit pod nginx-pod
        - kubectl edit pod <NOME_POD>
    ° Deletando os POD's imagens
        - kubectl delete pod nginx-pod
        - kubectl delete pod <NOME_POD>
    ° Deletando POD's Declarativa
        - kubectl delete -f pod.yaml
        - kubectl delete -f <FILE_NAME>
````
#### K8S DEPLOYMENT
````
    ° Criando DEPLOYMENT
        - kubectl create deployment hello-nginx --image=nginx:1.17-alpine
    ° Consultando DEPLOYMENT
        - kubectl get deployments
    ° Deletando todos os DEPLOYMENT
        - kubectl delete deployments --all
````
#### K8S SERVICES
##### Parque que serve?

     ° Abstrações para expor aplicações excutando em um ou mais pods
     ° Proveem IP's fixos para comunicação
     ° Proveem um DNS para um ou mais pods
     ° São capaszes de balanceamento de carga
----------------------------------------------------

     ° ClusterIP
          - Serve para comunicar diferentes POD's do mesmo cluster
     ° NodePort
     ° LoadBalancer
----------------------------------------------------

````
    ° Criando service
        - kubectl expose deployment hello-nginx --type=LoadBalancer --port=80
    ° Consultando services
        - kubectl get services
        - kubectl get svc
    ° Testando service
        - minikube service hello-nginx
    ° Deletando todos os SERVICES
        - kubectl delete services hello-nginx
   
````
#### K8S CONFIGMAPS
````
    ° Mosrtrado configmaps
        - kubectl get configmaps
````
#### K8S PERSISTENTVOLUMECLAIM
````
    ° Mosrtrado persistentvolumeclaim
        - kubectl get persistentvolumeclaim
        - kubectl get pvc
    ° Deletando todos os PERSISTENTVOLUMECLAIM
        - kubectl delete persistentvolumeclaim <NAME-persistentvolumeclaim>
        - kubectl delete pvc <NAME-persistentvolumeclaim>
````
#### K8S SECRETS
````
    ° Criando Secret mysql
        - kubectl create secret generic mysql-pass --from-literal=password='MyPasswordRoot'
    ° Mosrtrado Secret
        - kubectl get secrets
````
#### K8S Acessando containe
Exemplo:
____________________________
```` 
    - kubectl exec -it <NAME-POD> bash
    - kubectl exec -it mysql-server-7885f79678-x7fw6 bash
````

#### Testando Autoscaler
```` 
    ° Comsultando HPA
        - kubectl get hpa
    ° Criando POD e SERVICE aparti de uma image do DOCKER
        - kubectl run php-apache-hpa --image=chicowall/php-apache-hpa --requests=cpu=200m --expose --port=80
        ° Testando (criando pod/maquina busybox) e acessando via bash
            - kubectl run -it loader --image=busybox /bin/sh
            ° Teste estresse (Consultando POD "php-apache-hpa" )
                - wget -q -O- http://php-apache-hpa.default.svc.cluster.local;
            ° Teste estressando Autoscaler
                - while true; do wget -q -O- http://php-apache-hpa.default.svc.cluster.local; done;
```` 

## Dicas
#### Testando de maneira declarativa .Yaml
     - https://kubeyaml.com/