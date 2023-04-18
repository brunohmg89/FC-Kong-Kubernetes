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
    

