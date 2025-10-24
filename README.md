# Projeto: GitOps na prática com Kubernetes + ArgoCD
📋 Descrição

Este projeto tem como objetivo aplicar o conceito de GitOps utilizando Kubernetes e ArgoCD, para realizar o deploy automatizado da aplicação Online Boutique, composta por diversos microserviços da Google Cloud Platform.

A ideia é demonstrar como as empresas modernas automatizam deploys e mantêm o estado da infraestrutura versionado no GitHub.

🧱 Tecnologias utilizadas

1. Rancher Desktop (Kubernetes local - k3s)
2. Kubectl4
3. ArgoCD
4. GitHub
5. Docker

🎯 Objetivo do projeto

Executar o conjunto de microserviços Online Boutique dentro de um cluster Kubernetes local, controlado via GitOps com ArgoCD, a partir de um repositório público no GitHub.

⚙️ Pré-requisitos

Antes de iniciar, garanta que você possui:

1. Rancher Desktop instalado e com Kubernetes habilitado
2. Kubectl funcionando (kubectl get nodes)
3. Docker em execução
4. Conta no GitHub

# 🚀 Etapas do Projeto
## Etapa 1 – Fork e Repositório GitHub
### 📘 O que é um Fork?

O fork cria uma cópia pessoal de um repositório dentro da sua conta GitHub.
Neste projeto, o fork é feito do repositório oficial da Google:
👉 https://github.com/GoogleCloudPlatform/microservices-demo

![fork do repositorio](/images/fork.png)

Essa cópia permite:

Examinar e modificar o código da aplicação Online Boutique;

Criar seu próprio repositório GitOps com os manifests necessários para o ArgoCD;

Ter controle completo sobre a versão do código usada no deploy.

### 🧩 Estrutura do repositório GitOps

Após o fork, cria-se um repositório limpo e dedicado ao ArgoCD, contendo apenas o arquivo necessário para o deploy.

Criar um repositório no GitHub chamado gitops-microservices

![repositorio](/images/repositorio.png)

Criar a seguinte estrutura:

gitops-microservices/
└── k8s/
    └── online-boutique.yaml


Copiar o conteúdo do arquivo original:
microservices-demo/release/kubernetes-manifests.yaml

![online-boutique](/images/online-boutique.png)

Fazer commit e push para o GitHub.



Esse repositório se tornará a fonte de verdade do ArgoCD.

