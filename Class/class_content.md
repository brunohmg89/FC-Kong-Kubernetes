# Kong API Gateway e Kubernetes

## Conceitos Básicos

- Aula 1: Principais conceitos
    - Principais objetos de manipulação do Kong
        - Rotas
        - Services
        - Plugins
        - Consumers
        - Upstreams
        - Targets
    ![kong-kubernetes](img/kong-kubernetes.png)

- Aula 2: Kubernetes ingress kong
    - Kubernetes Ingress
    É a maneira de realizar a exposição de rotas HTTP e HTTPS para fora do cluster. Este roteamento de tráfego é controlado por regras definidas dentro do recurso Ingress do Kubernetes
    ![kong-kubernetes-2](img/kong-kubernetes-2.png)
    - Kong & Kubernetes Ingress
    ![kong-kubernetes-3](img/kong-kubernetes-3.png)
    - Tradução K8s -> Kong
    ![kong-kubernetes-4](img/kong-kubernetes-4.png)

- Aula 3: Modelos deployment kong kubernetes
    - Kong API Gateway - Banco de Dados K8s
    ![kong-kubernetes-5](img/kong-kubernetes-5.png)
    - Kong API Gateway DB-Less K8s
    ![kong-kubernetes-6](img/kong-kubernetes-6.png)

- Aula 4: Instalando Kong
    - Criando um cluster de K8s
        - Ferramentas que serão utilizadas
            - Kind || minikube || microk8s
            - kubectl
            - Helm v3
    ```
    kind create cluster --name kong-fc --config clusterconfig.yaml
    ```
    ```
    kubectl create ns kong
    ```
    ```
    choco install kubernetes-helm
    ```
    ```
    helm repo add kong https://charts.konghq.com
    ```
    ```
    helm repo update
    ```
    ```
    helm install kong kong/kong -f kong-conf.yaml --set proxy.type=NodePort,proxy.http.nodePort=30000,proxy.tls.nodePort=30003 --set ingressController.installCRDs=false --set serviceMonitor.enabled=true --set serviceMonitor.labels.release=promstack --namespace kong
    ```
    ```
    kubectl logs NOMEDOPOD proxy -n kong
    ```

- Aula 5: Ferramentas adicionais
    - Instalando ferramentas
    ```
    kubectl create ns monitoring
    ```
    ```
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    ```
    ```
    helm repo update
    ```
    ```
    helm install prometheus-stack prometheus-community/kube-prometheus-stack -f prometheus.yaml --namespace monitoring
    ```
    ```
    kubectl create ns iam
    ```
    ```
    helm repo add bitnami https://charts.bitnami.com/bitnami
    ```
    ```
    helm repo update
    ```
    ```
    helm install keycloak bitnami/keycloak --set auth.adminUser=keycloak,auth.adminPassword=keycloak --namespace iam
    ```
    - Criação da pasta "apps" e diversos arquivos yaml
    ```
    kubectl create ns bets
    ```
    ```
    kubectl apply -f .\apps\ --recursive -n bets
    ```

## Kong & Kubernetes

- Aula 6: Crd plugins
    - Kong Custom Resource Definitions <https://docs.konghq.com/kubernetes-ingress-controller/latest/concepts/custom-resources/>
    - Criado os primeiros manifestos:
    ```
    kubectl apply -f .\krate-limit.yaml -n bets
    ```
    ```
    kubectl apply -f .\kprometheus.yaml
    ```

- Aula 7: Kong ingress
    - Kong ingress
    - Criado mais alguns manifestos
    ```
    kubectl apply -f .\bets-api.yaml -n bets
    ```
    ```
    kubectl apply -f .\kingress.yaml -n bets
    ```

- Aula 8: Open id provider
    - Kong + OpenID Connect
    ```
    kubectl port-forward svc/keycloak 8080:80 -n iam
    ```
    - Criando usuários, realm e client pelo Keycloak
    ```
    kubectl port-forward svc/keycloak 8080:80 -n iam
    ```
    ```
    http://localhost:8080
    ```

- Aula 9: Kong openid plugin
    ```
    kubectl apply -f .\kopenid.yaml -n bets
    ```
    ```
    kubectl apply -f .\pod.yaml
    ```
    ```
    kubectl exec -it testcurl -- sh
    ```
    ```
    curl --location --request POST 'http://keycloak.iam/realms/bets/protocol/openid-connect/token' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --data-urlencode 'client_id=kong' \
    --data-urlencode 'grant_type=password' \
    --data-urlencode 'username=maria' \
    --data-urlencode 'password=maria' \
    --data-urlencode 'client_secret=DounaAGzGKoWNkx70DqAYF8szsGOadhM' \
    --data-urlencode 'scope=openid'
    ```

## APIOps

- Aula 10: APIOps
    - É um conceito que aplica os solidos e testados principios de DevOps e GitOps para o ciclo de vida das APIs e microsserviços.
    ![kong-kubernetes-7](img/kong-kubernetes-7.png)
    ![kong-kubernetes-8](img/kong-kubernetes-8.png)
    ![kong-kubernetes-9](img/kong-kubernetes-9.png)

- Aula 11: GitOps
    - É uma prática que usa infraestrutura como código, que implica que um determinado ambiente (dev, homologação, prod) esteja criado em uma maneira declarativa através de manifestos, e que através de automação seja possível recriar o ambiente de maneira repetitiva.
    Os manifestos devem estar armazenados dentro de um repositório git.
    ![kong-kubernetes-10](img/kong-kubernetes-10.png)

- Aula 12: Ferramentas necessárias
    - Github Actions
    - Spectral <https://stoplight.io/open-source/spectral>
    - Postman <https://learning.postman.com/docs/writing-scripts/test-scripts/>
    - ArgoCD <https://argoproj.github.io/cd/>

- Aula 13: Validando openapi lint
    - Criando arquivos de contrato (arquivos encontravam-se dentro do repositório pessoal do Claudio <https://github.com/claudioed/bets-app>)

- Aula 14: Checando contratos
    - Mostrou na prática na checagem dos contratos

- Aula 15: Instalando ArgoCD
