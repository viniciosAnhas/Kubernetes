<div align="center">
  <div>
    <img height = "150" width = "150" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kubernetes/kubernetes-plain-wordmark.svg" />
  </div>
</div>

<p style="text-align: justify;">O gerenciamento de configuração no Kubernetes refere-se ao processo de gerenciar e organizar as configurações necessárias para os aplicativos em execução dentro de um cluster. O Kubernetes oferece várias abordagens e recursos para facilitar esse gerenciamento, iremos ver como exemplo o <b>configMap</b> e <b>secrets</b>.</p>

<ol>
  <li style="text-align: justify;">ConfigMap</li>
  <ul>
    <li style="text-align: justify;">O ConfigMap é um recurso no Kubernetes projetado para armazenar configurações, como variáveis de ambiente ou arquivos de configuração. Ele permite a separação das configurações do código da aplicação.</li>
  </ul>
  <li style="text-align: justify;">Secrets</li>
  <ul>
    <li style="text-align: justify;">As Secrets são semelhantes aos ConfigMaps, mas são usadas para armazenar dados sensíveis, como senhas e chaves de API. Elas são codificadas em base64 e decodificadas automaticamente pelos Pods.</li>
  </ul>
</ol>

<h1>ConfigMap</h1>

<p style="text-align: justify;">No Kubernetes, o ConfigMap é um recurso que permite armazenar dados de configuração como pares de chave-valor, que podem ser usados pelos Pods. Aqui está um resumo dos principais aspectos relacionados ao ConfigMap:</p>

<ol>
  <li style="text-align: justify;">Definição</li>
  <ul>
    <li style="text-align: justify;">Um ConfigMap é um recurso no Kubernetes usado para armazenar dados de configuração, como variáveis de ambiente, arquivos de configuração ou quaisquer dados não confidenciais.</li>
  </ul>
  <li style="text-align: justify;">Formato de Chave-Valor</li>
  <ul>
    <li style="text-align: justify;">Os dados dentro de um ConfigMap são organizados como pares chave-valor. Isso permite uma fácil leitura e recuperação de configurações específicas.</li>
  </ul>
  <li style="text-align: justify;">Utilização em Pods</li>
  <ul>
    <li style="text-align: justify;">Os ConfigMaps podem ser montados como volumes em Pods ou utilizados diretamente como variáveis de ambiente, fornecendo uma maneira flexível de injetar configurações em aplicativos.</li>
  </ul>
  <li style="text-align: justify;">Atualizações Dinâmicas</li>
  <ul>
    <li style="text-align: justify;">Alterações feitas em um ConfigMap são refletidas automaticamente nos Pods que o referenciam. Essa característica permite a atualização dinâmica das configurações sem reinicialização dos Pods.</li>
  </ul>
</ol>

<p style="text-align: justify;">Veja um exemplo de configmap a seguir, iremos criar um pod de mongodb passando as variavies <b>MONGO_INITDB_ROOT_USRNAME</b> e <b>MONGO_INITDB_ROOT_PASSWORD</b> para um configmap.</p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:4.2.8
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USRNAME
          value: mongouser
        - name: MONGO_INITDB_ROOT_PASSWORD
          value: mongopwd
```

<p style="text-align: justify;">Podemos utilizar o mesmo arquivo para fazer essa altração assim podemos executar apenas um unico arquivo.</p>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:4.2.8
        ports:
          - containerPort: 27017
        envFrom:
          - configMapRef:
             name: mongodb-configmap

---

apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  MONGO_INITDB_ROOT_USRNAME: mongouser
  MONGO_INITDB_ROOT_PASSWORD: mongopwd
```

<p style="text-align: justify;">Para verificar se o <b>configmap</b> corretamente, utilizee o comando <i>kubectl get configmap</i>.</p>

```bash
kubectl get configmap
```

<p style="text-align: justify;">Podemos tambem acessar os valores do configmap utilizando o comando <i>kubectl describe configmap nomedoconfigmap</i>.</p>

```bash
kubectl describe configmap mongodb-configmap
```

<p style="text-align: justify;">Em resumo, o ConfigMap é uma ferramenta valiosa no Kubernetes para gerenciar dados de configuração de forma flexível e dinâmica, facilitando a separação das configurações do código da aplicação e permitindo atualizações sem a necessidade de reiniciar os Pods.</p>