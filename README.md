**# Projeto-final-k8s**
A escolha das imagens são baseadas nas aulas, ainda esta aprendento o que cada imagem faz, então não consegui desenvolver uma aplicação que interaja entre elas, no entando, todas estão funcionando corretamente e conseguimos testar cada uma. 

**AMBIENTE DO MINIKUBE:**
kitej@DESKTOP-K8JPMU6:~$ minikube start
😄  minikube v1.31.2 on Ubuntu 22.04 (amd64)
✨  Using the docker driver based on user configuration
🎉  minikube 1.33.1 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.33.1
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.27.4 on Docker 24.0.4 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

**VERSÃO DO MINIKUBE**
minikube version: v1.31.2
commit: fd7ecd9c4599bef9f04c0986c4a0187f98a4396e

**VERSÃO DO DOCKER**
Client: Docker Engine - Community
 Version:           26.1.0
 API version:       1.45
 Go version:        go1.21.9
 Git commit:        9714adc
 Built:             Mon Apr 22 17:06:41 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          26.1.0
  API version:      1.45 (minimum version 1.24)
  Go version:       go1.21.9
  Git commit:       c8af8eb
  Built:            Mon Apr 22 17:06:41 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.31
  GitCommit:        e377cd56a71523140ca6ae87e30244719194a521
 runc:
  Version:          1.1.12
  GitCommit:        v1.1.12-0-g51d5e94
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

**Projeto**
O projeto constitui em:

2 Deployments (Nginx e Apache);
1 Configmap;
1 Secret;
1 Namespace;
1 Service.

**Segue o conteúdo do arquivo YAML**

#Namespace
apiVersion: v1
kind: Namespace
metadata:
  name: k8s-prd

---

#Configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap-k8s
  namespace: k8s-prd
data:
  DATABASE: bXlzcWw=
  DATABASE_USER: am9hb3NpbHZh
  DATABASE_PASSWORD: SzhzVGFrMW5n

---

#Secret
apiVersion: v1
kind: Secret
metadata:
  name: secret-k8s
  namespace: k8s-prd
data:
  DATABASE: bXlzcWw=
  DATABASE_USER: am9hb3NpbHZh
  DATABASE_PASSWORD: SzhzVGFrMW5n

---

#Deployment Nginx
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-nginx-k8s
  namespace: k8s-prd
spec:
  replicas: 1
  selector:
    matchLabels: 
      app: svc-k8s
  template:
    metadata:
      labels:
        app: svc-k8s
    spec:
      containers:
        - name: nginx
          image: nginx
          resources:
            requests:
              cpu: "500m"
              memory: "128Mi"
            limits: 
              cpu: "1000m"
              memory: "256Mi"
          ports:
          - containerPort: 80
          envFrom:
            - configMapRef:
                name: configmap-k8s

---

#Deployment Apache
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-apache-k8s
  namespace: k8s-prd
spec:
  replicas: 1
  selector:
    matchLabels: 
      app: svc-k8s
  template:
    metadata:
      labels:
        app: svc-k8s
    spec:
      containers:
        - name: apache
          image: httpd
          resources:
            requests:
              cpu: "500m"
              memory: "128Mi"
            limits: 
              cpu: "1000m"
              memory: "256Mi"
          ports:
          - containerPort: 80
          envFrom:
            - secretRef:
                name: secret-k8s
--- 

#Service
apiVersion: v1
kind: Service
metadata:
  name: svc-k8s
  namespace: k8s-prd
spec:
  selector:
    app: svc-k8s
  ports:
  - port: 80

_________________________________________________________________________________________________________

**Testes de ambiente**
![image](https://github.com/KitejBR/Projeto-final-k8s/assets/147888865/19d062ab-9681-476c-ab9d-5797f8ccac05)
![image](https://github.com/KitejBR/Projeto-final-k8s/assets/147888865/0e098747-5794-4157-8985-a8ce8cb39ccf)

**Comandos utilizados nos testes:**
kubectl get pods -n k8s-prd -o wide
kubectl get svc -n k8s-prd
kubectl get cm -n k8s-prd
kubectl get secret -n k8s-prd 
kubectl  describe cm  -n k8s-prd configmap-k8s
kubectl  describe secret  -n k8s-prd secret-k8s

**Teste de verificação das variáveis do Configmap e Secret**
![image](https://github.com/KitejBR/Projeto-final-k8s/assets/147888865/4b7697fa-c363-4638-87ed-d79af5f5271b)

**Comandos utilizados nos testes:**
kubectl exec -it -n k8s-prd deploy-apache-k8s-6b5958bf88-8l4mq -- env | grep -i DATABASE
kubectl exec -it -n k8s-prd deploy-nginx-k8s-8558fbddb8-cprp9 -- env | grep -i DATABASE
**Obsercação: O nome dos pods serão diferente dos comandos acima.**
