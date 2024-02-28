<div align="center">
  <div>
    <img height = "150" width = "150" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kubernetes/kubernetes-plain-wordmark.svg" />
  </div>
</div>

<p style="text-align: justify;">No Kubernetes, um Namespace é um recurso que proporciona uma forma de organizar e isolar logicamente os recursos dentro de um cluster. Aqui estão os principais pontos sobre Namespaces:</p>

<ol>
  <li style="text-align: justify;">Definição</li>
  <ul>
    <li style="text-align: justify;">Um Namespace é um espaço virtual dentro de um cluster Kubernetes que permite dividir os recursos do cluster em unidades lógicas isoladas.</li>
  </ul>
  <li style="text-align: justify;">Organização Lógica</li>
  <ul>
    <li style="text-align: justify;">Os Namespaces fornecem uma maneira de organizar e dividir os recursos do cluster com base em equipes, projetos ou outros critérios de organização.</li>
  </ul>
  <li style="text-align: justify;">Isolamento</li>
  <ul>
    <li style="text-align: justify;">Cada Namespace fornece um escopo isolado para recursos, como Pods, Services e Deployments. Isso permite que diferentes equipes ou aplicações coexistam no mesmo cluster sem interferências.</li>
  </ul>
  <li style="text-align: justify;">Recursos Compartilhados ou Isolados</li>
  <ul>
    <li style="text-align: justify;">Certos recursos, como Nodes e PersistentVolumes, são compartilhados por padrão em todos os Namespaces, enquanto outros, como Pods e Services, são isolados por padrão em um Namespace específico.</li>
  </ul>
  <li style="text-align: justify;">Recursos Compartilhados ou Isolados</li>
  <ul>
    <li style="text-align: justify;">Quando os recursos são criados sem especificar um Namespace, eles são automaticamente colocados no Namespace padrão <b>default</b>.</li>
  </ul>
</ol>

<p style="text-align: justify;">Podemos utilizar o comando <i>kubectl get namespace</i>, para listar todos os namespaces do cluster.</p>

```bash
kubectl get namespace
```

<p style="text-align: justify;">Alem do comando de listar podemos usar o comando para criar um novo namespace utilizando o comando <i>kubectl create namespace novonamespace</i>, iremos criar um namespace chamado prod como exemplo.</p>

```bash
kubectl create namespace prod
```

<p style="text-align: justify;">Ao criar um recurso no Kubernetes, como um Pod ou um Service, é possível associá-lo a um Namespace específico. Por exemplo, ao criar um Pod, pode-se incluir um campo namespace no arquivo de configuração YAML</p>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  namespace: prod
spec:
  containers:
  - name: mycontainer
    image: nginx
```