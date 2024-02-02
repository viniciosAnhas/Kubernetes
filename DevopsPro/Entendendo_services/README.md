<div align="center">
  <div>
    <img height = "150" width = "150" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kubernetes/kubernetes-plain-wordmark.svg" />
  </div>
</div>

<h1>Services</h1>

<p style="text-align: justify;">No Kubernetes, Services são uma abstração que permite expor aplicativos em execução em um conjunto de Pods como um serviço de rede consistente. Aqui está um resumo dos principais conceitos relacionados a Services:</p>

<ol>
  <li style="text-align: justify;"><b>Definição</b></li>
  <ul>
    <li style="text-align: justify;">Um Service é um recurso que define um conjunto lógico de Pods e uma política de acesso a esses Pods.</li>
    <li style="text-align: justify;">Ele fornece uma maneira de expor serviços e aplicações, permitindo que outros Pods ou serviços os acessem de maneira confiável.</li>
  </ul>
  <li style="text-align: justify;"><b>Tipos de Services</b></li>
  <ul>
    <li style="text-align: justify;">Existem diferentes tipos de Services no Kubernetes, incluindo:</li>
    <ul>
      <li style="text-align: justify;"><b>ClusterIP</b>: Expõe o Service apenas dentro do cluster Kubernetes.</li>
      <li style="text-align: justify;"><b>NodePort</b>: Expõe o Service em um determinado porto em cada nó do cluster. Pode ser acessado externamente usando o endereço IP do nó e a porta atribuída.</li>
      <li style="text-align: justify;"><b>LoadBalancer</b>: Cria um balanceador de carga externo para rotear o tráfego para o Service. Geralmente, é associado a um provedor de nuvem que oferece um serviço de balanceador de carga.</li>
      <li style="text-align: justify;"><b>ExternalName</b>: Mapeia o Service para um nome de host externo (fora do cluster).</li>
    </ul>
  </ul>
  <li style="text-align: justify;"><b>Seletores e Labels</b></li>
  <ul>
    <li style="text-align: justify;">Services usam seletores baseados em labels para associar-se a conjuntos específicos de Pods. Os Pods correspondentes ao seletor são automaticamente incluídos no conjunto de endpoints do Service.</li>
  </ul>
  <li style="text-align: justify;"><b>Descoberta de Serviços</b></li>
  <ul>
    <li style="text-align: justify;">Um Service permite a descoberta dinâmica de outros recursos no cluster, já que o DNS do Kubernetes pode ser usado para encontrar Services e seus Pods associados.</li>
  </ul>
  <li style="text-align: justify;"><b>Balanceamento de Carga</b></li>
  <ul>
    <li style="text-align: justify;">Services oferecem balanceamento de carga para distribuir o tráfego entre os Pods correspondentes, proporcionando alta disponibilidade e escalabilidade.</li>
  </ul>
</ol>

<h1>ClusterIP</h1>

<p style="text-align: justify;">O tipo de Service ClusterIP no Kubernetes é usado para expor um conjunto de Pods como um serviço dentro do cluster. Aqui está um resumo das características principais do Service ClusterIP:</p>

<ol>
  <li style="text-align: justify;"><b>Escopo de Acesso</b></li>
  <ul>
    <li style="text-align: justify;">O ClusterIP expõe o Service apenas dentro do cluster Kubernetes. Isso significa que outros Pods ou Services dentro do mesmo cluster podem acessá-lo.</li>
  </ul>
  <li style="text-align: justify;"><b>Endereço IP Fixo</b></li>
  <ul>
    <li style="text-align: justify;">O ClusterIP é atribuído um endereço IP interno fixo, que serve como ponto de acesso ao Service. Esse endereço IP é usado para acessar o conjunto de Pods associados ao Service.</li>
  </ul>
  <li style="text-align: justify;"><b>Seletores e Labels</b></li>
  <ul>
    <li style="text-align: justify;">O Service ClusterIP utiliza seletores baseados em labels para associar-se a um conjunto específico de Pods. Os Pods correspondentes ao seletor são incluídos automaticamente no conjunto de endpoints do Service.</li>
  </ul>
  <li style="text-align: justify;"><b>Descoberta de Serviços</b></li>
  <ul>
    <li style="text-align: justify;">O DNS do Kubernetes pode ser usado para descobrir e acessar Services do tipo ClusterIP. O Service é mapeado para um nome DNS dentro do cluster, permitindo que outros Pods o acessem pelo nome.</li>
  </ul>
  <li style="text-align: justify;"><b>Balanceamento de Carga</b></li>
  <ul>
    <li style="text-align: justify;">O Service ClusterIP fornece balanceamento de carga interno entre os Pods correspondentes. Quando há múltiplos Pods associados ao Service, o tráfego é distribuído entre eles para proporcionar alta disponibilidade e escalabilidade.</li>
  </ul>
</ol>

<p style="text-align: justify;">Um exemplo básico de um Service ClusterIP YAML incluiria a definição do tipo de Service, o seletor de Pods associado e detalhes sobre as portas expostas</p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: front
spec:
  replicas: 2
  selector:
    matchLabels:
      app: front
  template:
    metadata:
      labels:
        app: front
    spec:
      containers:
      - name: front
        image: nginx

---

apiVersion: v1
kind: Service
metadata:
  name: front-service
spec:
  selector:
    app: front
  type: ClusterIP
  ports:
  - port: 80
    targetPort: 80
```

<p style="text-align: justify;">Para verificar se o service foi criado com sucesso execute o comando <i>kubectl get services</i>.</p>

```bash
kubectl get services
```

<p style="text-align: justify;">Iremos ver na pratica como o ClusterIP funciona, para isso iremos precisar acessar o Pod no modo interativo e executar o comando curl contendo o endereço do service. Para acessar o Pod primeiro precisamos consultar o nome do Pod, para isso basta utilizar o comando <i>kubectl get pod</i>, copie o nome de um dos Pods e em seguida utilize o comando <i>kubectl exec -it nomeDoPod /bin/bash</i>, com esse comando voce tera acesso ao terminal do Pod escolhido.</p>

```bash
kubectl exec -it nomeDoPod /bin/bash
```

<p style="text-align: justify;">Em seguida utilize o comando <i>apt update</i> para atualziar os pacotes do Pod e em seguida utilize o comando <i>apt install curl -y</i> para realizar a instalação do curl no Pod.</p>

```bash
apt update
apt install curl -y
```

<p style="text-align: justify;">Apos utilize o comando <i>curl http://front-service<i>, isso ira retornar o HTML da pagina inicial do nginx no terminal.</p>

```bash
curl http://front-service
```

<p style="text-align: justify;">Em resumo, o Service ClusterIP é uma maneira eficiente de disponibilizar serviços dentro do cluster Kubernetes, proporcionando uma comunicação interna entre os componentes de um aplicativo distribuído.</p>