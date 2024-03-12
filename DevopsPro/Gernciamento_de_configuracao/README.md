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

<h1>Secret</h1>

<p style="text-align: justify;">
No Kubernetes, o recurso Secret é usado para armazenar e gerenciar dados sensíveis, como senhas, chaves de API e certificados. Aqui está um resumo dos principais aspectos relacionados aos Secrets.</p>

<ol>
  <li style="text-align: justify;">Definição</li>
  <ul>
    <li style="text-align: justify;">Um Secret é um recurso no Kubernetes projetado para armazenar e gerenciar informações sensíveis, como senhas, chaves de API e certificados.</li>
  </ul>
  <li style="text-align: justify;">Codificação Base64</li>
  <ul>
    <li style="text-align: justify;">Os dados dentro de um Secret são codificados em base64, proporcionando uma camada mínima de segurança. No entanto, os Secrets não são destinados a fornecer segurança total e não devem ser usados para armazenar informações altamente confidenciais sem considerações adicionais.</li>
  </ul>
  <li style="text-align: justify;">Utilização em Pods</li>
  <ul>
    <li style="text-align: justify;">Secrets podem ser montados como volumes em Pods ou usados como variáveis de ambiente. Isso permite que os dados sensíveis sejam acessados pelos aplicativos dentro dos contêineres.</li>
  </ul>
</ol>

<p style="text-align: justify;">Veja um exemplo de secret a seguir, iremos usar o mesmo pod do mongodb, porem utilizando o secrete para armazenar as variavies <b>MONGO_INITDB_ROOT_USRNAME</b> e <b>MONGO_INITDB_ROOT_PASSWORD</b>.</p>

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
          - secretRef:
             name: mongodb-secret

---

apiVersion: v1
kind: Secret
metadata:
  name: mongodb-secret
type: Opaque
data:
  MONGO_INITDB_ROOT_USRNAME: bW9uZ291c2Vy
  MONGO_INITDB_ROOT_PASSWORD: bW9uZ29wd2Q=
```

<p style="text-align: justify;">Repare que no valor do usuario e senha, os valores precisam ser passados criptografados na base 64 para isso voce pode utilizar o comando no bash <i>echo -n "valor" | base64</i>.</p>

```bash
echo -n "valor" | base64
```

<p style="text-align: justify;">Para verificarmos se o secret foi criado corretamente podemos utilizar o comando <i>kubectl get secret</i>.</p>

```bash
kubectl get secret
```

<p style="text-align: justify;">Utilizando o comando <i>kubectl describe secret nomedosecret</i>, iremos verificar que os valores foram criptografados, tendo assim mais segurança para armazenar valores sensíveis.</p>

```bash
kubectl describe secret mongodb-secret
```

<p style="text-align: justify;">Em resumo, o Secret no Kubernetes é uma ferramenta valiosa para gerenciar e disponibilizar informações sensíveis aos aplicativos em execução em contêineres, garantindo a segurança e flexibilidade necessárias para trabalhar com dados confidenciais.</p>