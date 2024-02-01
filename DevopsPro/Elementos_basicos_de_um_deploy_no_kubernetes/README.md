<div align="center">
  <div>
    <img height = "150" width = "150" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kubernetes/kubernetes-plain-wordmark.svg" />
  </div>
</div>

<h1>Pod</h1>

<p style="text-align: justify;">Um Pod é a menor unidade de implantação no Kubernetes e representa um ou mais contêineres que compartilham o mesmo contexto de rede e armazenamento. Os contêineres em um Pod são sempre implantados juntos e compartilham o mesmo ciclo de vida, o que facilita a comunicação e a colaboração entre eles.</p>

<p style="text-align: justify;">Os Pods são gerenciados pelo Kubernetes e podem ser escalados, atualizados e distribuídos automaticamente para garantir a disponibilidade e o desempenho dos aplicativos.</p>

<p style="text-align: justify;">Veja um exemplo de pod a seguir.</p>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
```

<ul>
  <li style="text-align: justify;"><b>apiVersion</b>: Especifica a versão da API do Kubernetes que será usada para interpretar o arquivo YAML. No exemplo fornecido, a versão é v1, indicando a API principal do Kubernetes;</li>
  <li style="text-align: justify;"><b>kind</b>: Define o tipo de recurso Kubernetes que está sendo criado. No exemplo, o recurso é um "Pod", que é a menor unidade de implantação no Kubernetes;</li>
  <li style="text-align: justify;"><b>metadata</b>: Contém metadados associados ao recurso, como o nome do Pod. No exemplo, o nome do Pod é definido como "nginx";</li>
  <li style="text-align: justify;"><b>spec</b>: Descreve a especificação do recurso, ou seja, as configurações e características específicas do Pod;</li>
  <li style="text-align: justify;"><b>containers</b>: Lista os contêineres que devem ser executados no Pod;</li>
  <li style="text-align: justify;"><b>name</b>: Especifica o nome do contêiner dentro do Pod. Neste caso, o nome do contêiner é "nginx";</li>
  <li style="text-align: justify;"><b>image</b>: Indica a imagem do Docker que será usada para criar o contêiner. No exemplo, está usando a imagem oficial do Nginx.</li>
</ul>

<p style="text-align: justify;">Para realizar o deploy do pod no servidor kubernetes utilize o comando <i>kubectl apply</i>.<p>

```bash
kubectl apply -f pod.yaml
```

<p style="text-align: justify;">Apos executar o comando verifique se o pod foi iniciado utilizando o comando <i>kubectl get pod</i>.<p>

```bash
kubectl get pod
```

<p style="text-align: justify;">Para deletar o pod criado basta usar o comando <i>kubectl delete pod</i> e passar como parametro o nome do pod.<p>

```bash
kubectl delete pod ngnix
```

<h1>Labels e Selectors</h1>

<p style="text-align: justify;">Labels e selectors são conceitos fundamentais que permitem identificar, organizar e agrupar recursos dentro do cluster. Vamos entender cada um desses conceitos:<p>

<ol>
  <li><b>Labels</b></li>
  <ul>
    <li style="text-align: justify;"><b>Definição</b>: Labels são pares chave-valor associados a recursos no Kubernetes. Eles são metadados que podem ser anexados a objetos, como Pods, Services e Deployments.</li>
    <li style="text-align: justify;"><b>Proposito</b>: Labels são usados para identificar, categorizar e filtrar recursos. Eles fornecem uma maneira flexível de associar informações adicionais aos objetos.</li>
    <li style="text-align: justify;"><b>Exemplo</b>:  No contexto de um Pod, pode-se adicionar labels como <i>app: frontend</i> e <i>env: production</i> para identificar a aplicação e o ambiente ao qual o Pod pertence.</li>
  </ul>

  <li><b>Selectors</b></li>
  <ul>
    <li style="text-align: justify;"><b>Definição</b>: Selectors são usados para filtrar recursos com base nos labels associados a eles. Eles definem critérios de correspondência para selecionar um conjunto específico de recursos.</li>
    <li style="text-align: justify;"><b>Proposito</b>: Seletores são usados em diversos contextos, como na definição de Services, ReplicaSets e outros controladores, para escolher quais Pods ou recursos devem ser afetados por uma determinada configuração.</li>
    <li style="text-align: justify;"><b>Exemplo</b>: Se um Service no Kubernetes utiliza um seletor como <i>app: frontend</i>, ele redirecionará o tráfego para todos os Pods que possuem o label <i>app: frontend</i>.</li>
  </ul>
</ol>

<p style="text-align: justify;">Veja a seguir um exemnplo de pod utilizando labels.</p>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
    versao: beta
spec:
  containers:
  - name: nginx
    image: nginx
```

<p style="text-align: justify;">Utilizando labels em seu pod voce podera realizar consultas personalizadas</p>

```bash
kubectl get pod -l app=nginx
ou
kubectl get pod -l versao=beta
```

<h1>ReplicaSet</h1>

<p style="text-align: justify;">ReplicaSet é um controlador que garante que um número especificado de réplicas (cópias) de um conjunto de Pods esteja sempre em execução no cluster. Aqui está um resumo dos principais conceitos relacionados ao ReplicaSet:<p>

<ol>
  <li><b>Definição</b></li>
  <ul>
    <li style="text-align: justify;">O ReplicaSet é um recurso no Kubernetes que faz parte do controle de replicação.</li>
    <li style="text-align: justify;">Ele define um conjunto de Pods idênticos, mantendo um número desejado de réplicas desses Pods em execução.</li>
  </ul>
  <li><b>Propósito</b></li>
  <ul>
    <li style="text-align: justify;">O ReplicaSet é usado para garantir alta disponibilidade e escalabilidade de aplicativos no cluster Kubernetes.</li>
    <li style="text-align: justify;">Mantém o número especificado de cópias de Pods, reiniciando ou substituindo Pods caso falhem ou sejam excluídos.</li>
  </ul>
  <li><b>Seleção de Pods</b></li>
  <ul>
    <li style="text-align: justify;">Usa um seletor (selector) para identificar os Pods que fazem parte do conjunto gerenciado pelo ReplicaSet. Os Pods correspondentes ao seletor são considerados membros do conjunto.</li>
  </ul>
  <li><b>Atualizações e Rollouts</b></li>
  <ul>
    <li style="text-align: justify;">Permite atualizações controladas e rollouts incrementais de novas versões de aplicativos. Isso é feito ajustando a quantidade desejada de réplicas ao longo do tempo.</li>
  </ul>
</ol>

<p style="text-align: justify;">Um exemplo básico de um ReplicaSet YAML incluiria a especificação do número desejado de réplicas e o seletor para identificar os Pods que fazem parte do conjunto.<p>

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: primeiroreplicaset
spec:
  replicas: 3 
  selector:
    matchLabels:
      app: nginx-replicaset
  template:
    metadata:
      labels:
        app: nginx-replicaset
    spec:
      containers:
      - name: nginx
        image: nginx
```

<p style="text-align: justify;">Utilizando o comando <i>kubectl get pod</i>, iremos perceber que possuimos tres pods conforme é indicado no campo <i>replicas</i> no arquivo yaml.<p>

<p style="text-align: justify;">Para listar o replicaset utilize o comando <i>kubectl get replicaset</i>.<p>

```bash
kubectl get replicaset
```

<p style="text-align: justify;">Para deletar o replicaset basta usar o comando <i>kubectl delete replicaset</i> passando como parametro o nome do replicaset.<p>

```bash
kubectl delete replicaset primeiroreplicaset
```

<p style="text-align: justify;">Em resumo, um ReplicaSet no Kubernetes é um mecanismo essencial para gerenciar e garantir o número correto de instâncias de Pods em execução, facilitando assim a escalabilidade e a confiabilidade dos aplicativos no ambiente de cluster.<p>

<h1>Deployment</h1>

<p style="text-align: justify;"> O Deployment é um recurso que facilita a atualização e o gerenciamento de aplicações ao lidar com a criação e atualização de Pods de maneira declarativa. Aqui está um resumo dos principais conceitos relacionados ao Deployment:<p>

<ol>
  <li><b>Definição</b></li>
  <ul>
    <li style="text-align: justify;">O Deployment é um controlador no Kubernetes usado para declarar como os Pods de uma aplicação devem ser criados, atualizados e escalonados.</li>
    <li style="text-align: justify;">Ele fornece uma abstração de nível superior para gerenciar aplicações, garantindo que o número desejado de réplicas esteja sempre em execução.</li>
  </ul>
  <li><b>Rolling Updates</b></li>
  <ul>
    <li style="text-align: justify;">O Deployment facilita atualizações sem tempo de inatividade usando estratégias de rolling update. Isso significa que novas versões dos Pods são gradualmente introduzidas enquanto as versões antigas são desativadas, garantindo uma transição suave.</li>
  </ul>
  <li><b>Rollback</b></li>
  <ul>
    <li style="text-align: justify;">Se uma atualização causar problemas inesperados, o Deployment permite reverter rapidamente para uma versão anterior, facilitando rollbacks seguros.</li>
  </ul>
  <li><b>ReplicaSets</b></li>
  <ul>
    <li style="text-align: justify;">Por baixo dos panos, o Deployment gerencia ReplicaSets, que são controladores responsáveis por garantir que um número específico de réplicas de Pods esteja em execução.</li>
  </ul>
</ol>

<p style="text-align: justify;"> Um exemplo básico de um Deployment YAML incluiria a definição do número inicial de réplicas, a especificação do template do Pod e detalhes sobre a estratégia de atualização.<p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: meuprimeirodeployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-deployment
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - name: nginx
        image: nginx
```

<p style="text-align: justify;">Para listar o deployment utilize o comando <i>kubectl get deployment</i>.<p>

```bash
kubectl get deployment
```

<p style="text-align: justify;">Para deletar o deployment basta usar o comando <i>kubectl delete deployment</i> passando como parametro o nome do deployment.<p>

```bash
kubectl delete deployment meuprimeirodeployment
```

<p style="text-align: justify;">Em resumo, o Deployment no Kubernetes simplifica o processo de implantação e atualização de aplicações, fornecendo uma abstração de alto nível e mecanismos para garantir consistência e segurança durante as mudanças de versão.<p>