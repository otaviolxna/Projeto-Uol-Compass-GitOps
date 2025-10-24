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

![Criando o repositorio](/images/repositorio.png)

Criar a seguinte estrutura:

gitops-microservices/
└── k8s/
    └── online-boutique.yaml


Copiar o conteúdo do arquivo original:
microservices-demo/release/kubernetes-manifests.yaml

![Criando o arquivo online-boutique](/images/online-boutique.png)

Fazer commit e push para o GitHub.

Esse repositório se tornará a fonte de verdade do ArgoCD.

## Etapa 2 – Instalar o ArgoCD no Cluster Kubernetes

Crie o namespace e instale o ArgoCD com os manifests oficiais:

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

![Instalação do ArgoCD](/images/argo.png)

Verifique os pods:

kubectl get pods -n argocd

![Verificação dos pods](/images/pods.png)

A instalação será concluída quando todos estiverem com STATUS = Running.

# Etapa 3 – Acessar o Painel do ArgoCD

Crie o port-forward para acessar via navegador:

kubectl port-forward svc/argocd-server -n argocd 8080:443

![Interface ArgoCD](/images/interface.png)

Acesse:
👉 https://localhost:8080 no navegador de sua preferência

Usuário padrão: admin
Senha inicial:

No PowerShell rode o comando para descobrir a senha do ArgoCD:
kubectl -n argocd get secret argocd-initial-admin-secret `
 -o jsonpath="{.data.password}" | %{ [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_)) }

![Descobrindo sua Senha](/images/senha.png)

Após o primeiro login, é recomendável alterar a senha.

![Site do ArgoCD](/images/navegador.png)

# Etapa 4 – Criar o App no ArgoCD

No painel web, clique em NEW APP e preencha:

Application Name -> online-boutique
Project	         -> default

![Criação do APP](/images/p1.png)

Repository URL -> URL do seu repositório Git
Revision	   -> main
Path	       -> k8s

![Criação do APP](/images/p2.png)

Cluster URL	-> https://kubernetes.default.svc
Namespace	-> default

![Criação do APP](/images/p3.png)

Depois clique em Create → Sync → Synchronize

![Sincronização](/images/sync.png)

O ArgoCD fará o deploy automático da aplicação.

# Etapa 5 – Verificar o Deploy

Verifique se os pods foram criados:

kubectl get pods

Saída esperada:

adservice-xxxx                 Running
cartservice-xxxx               Running
frontend-xxxx                  Running
...

![Verificação do Deploy](/images/deploy.png)

# Etapa 6 – Acessar o Frontend da Aplicação

Como o serviço frontend-external é do tipo ClusterIP, é necessário um port-forward:

kubectl port-forward svc/frontend-external 8081:80

![Subindo Site](/images/subindo.png)


Acesse no navegador:
👉 http://localhost:8081

Você verá a loja Online Boutique funcionando 🎉

![Site Online](/images/site.png)

# Etapa 7 – (Opcional) Customizar o Manifest

Para testar o comportamento GitOps:

Edite o arquivo online-boutique.yaml no GitHub.

Altere, por exemplo:

replicas: 1

para

replicas: 3

Faça o commit no GitHub.

O ArgoCD detectará a alteração e atualizará o cluster automaticamente, criando mais pods.

Verifique it--:

kubectl get pods -l app=frontend



