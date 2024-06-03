Nome: Klelvis Carlos de Andrade 

**# Projeto-final-k8s**
A escolha das imagens são baseadas nas aulas, ainda esta aprendendo o que cada imagem faz, então não consegui desenvolver uma aplicação que interaja entre elas, no entando, todas estão funcionando corretamente e conseguimos testar cada uma. 

**AMBIENTE DO MINIKUBE:**
Minikube v1.31.2 on Ubuntu 22.04 (amd64)
Docker container (CPUs=2, Memory=2200MB) 


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


**Testes de ambiente**
![image](https://github.com/KitejBR/Projeto-final-k8s/assets/147888865/5c8c6dc3-69c6-44ed-85be-6be53d49b821)
![image](https://github.com/KitejBR/Projeto-final-k8s/assets/147888865/a8da11b6-ee20-4798-9474-fd43669d53a2)
![image](https://github.com/KitejBR/Projeto-final-k8s/assets/147888865/8206b2d1-c8b9-486d-a857-db6eca8ad013)


**Comandos utilizados nos testes:**
kubectl get pods -n k8s-prd -o wide
kubectl get svc -n k8s-prd
kubectl get deployments -n k8s-prd
kubectl get ns

**Teste de verificação das variáveis do Configmap e Secret**
![image](https://github.com/KitejBR/Projeto-final-k8s/assets/147888865/13480faf-6041-46e2-b44e-099d54bc3486)

**Comandos utilizados nos testes:**
kubectl get cm -n k8s-prd
kubectl get secret -n k8s-prd 
kubectl  describe cm  -n k8s-prd configmap-k8s
kubectl  describe secret  -n k8s-prd secret-k8s
kubectl exec -it -n k8s-prd deploy-apache-k8s-6b5958bf88-8l4mq -- env | grep -i DATABASE
kubectl exec -it -n k8s-prd deploy-nginx-k8s-8558fbddb8-cprp9 -- env | grep -i DATABASE
**Obsercação: O nome dos pods serão diferente dos comandos acima.**
