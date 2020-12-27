#### `history` of getting started



    `sudo apt-get update`

    `sudo apt-get install -y docker.io apt-transport-https ca-certificates curl software-properties-common gnupg2 conntrack`

    `sudo systemctl enable docker.service`

    `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64   && chmod +x minikube`
 
    5  sudo mkdir -p /usr/local/bin/ && sudo install minikube /usr/local/bin/
    6  sudo minikube start --vm-driver=none
    7  sudo minikube status
    8  curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl && chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl
    9  kubectl version --client
    
    *kubernetes management* 

    10  alias k='sudo kubectl'
    11  k version --client
    13  alias minikube='sudo minikube'
    15  alias | grep -i sudo >> ~/.bashrc
    16  tail -10 ~/.bashrc
    17  k cluster-info
    18  k deployment cookie --image=k8s.gcr.io/echoserver:1.10
    19  k create deployment cookie --image=k8s.gcr.io/echoserver:1.10
     
    21  k expose deployment cookie --type=NodePort --port=8080
    23  k get pods
    24  k get svc
    25  curl 10.105.179.230:8080
    26  minikube service cookie --url
    27  curl http://10.0.2.15:30623
    
    29  k logs cookie-5c9d44dbdf-gr8r5
    30  k get pods
    31  k get svc
    32  k delete svc cookie
    33  k delete deployment cookie
 
    37  k get node
    38  k get nodes -o wide
    39  k describe node
    40  k get node -o yaml
    41  k get node -o json

#### create/update based on yml

    k create -f mypod.yml
    k apply -f mypod.yml

#### labels

    k get pod --show-labels
    k get pod -L env -L app
    k get pod -l env=dev
    k get pod -l env!=dev
    k get pod -l env in (prod,dev)
    k get pod -l env notin (prod,dev)

    k get node --show-labels
    k get node -L env
    k label node kub-node1 env=dev

    k delete pod -l env=dev

#### annotations

    k describe pod mypod
    k annotate pod mypod notes='asdsdgsdgsgd'

#### ns

    k get ns
    k create -f custom-namespace.yaml
    k create -f mypod.yml -n custom-namespace
    k get pod -n custom-namespace
    k get pods --namespace kube-system

#### replicaControllers
    k create -f mypod-rc.yaml
    k get pods
    k get rc
    k scale rc mypod --replicas=10
#### replicaSet

    k create -f mypod-replicaset.yaml
    k get pods
    k get rs
    k delete rs mypod

#### DaemonSet, run on all nodes

    k create -f daemon-set.yml
    k get ds
    k get node --show-labels
    k get node -L disk
    k label node minikube disk=hdd --overwrite

#### jobs, do not restart after they are complete unlike containers maintained by replicasets

    k create -f job.yml
    k get jobs

#### cronjobs

    k create -f cronjob.yml
    k get cronjobs

#### check-up

    k get events | k get events -w

#### services

    k create -f mypod-svc.yml
    k get svc
    k describe service podsvc
    k get endpoints

#### dns

    k exec mypod -- env
    k exec mypod -- cat /etc/resolv.conf
    k get svc
    k exec mypod -- curl 10.110.207.248
    k exec mypod -- curl podsvc

#### volumes to mount in mongodb.yml

    k create -f mongodb.yml
    k exec -it mongodb -- mongo
    use mydb
    db.test.insert({name:'test'})
    db.test.find()
    exit


    k delete pod mongodb
    k create -f mongodb.yml
    k exec -it mongodb -- mongo
    use mydb
    db.test.find()
