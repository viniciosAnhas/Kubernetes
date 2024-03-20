<div align="center">
  <div>
    <img height = "150" width = "150" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kubernetes/kubernetes-plain-wordmark.svg" />
  </div>
</div>

<p style="text-align: justify;">No contexto do Kubernetes, "self-healing" (autocura ou auto-recuperação) refere-se à capacidade do sistema de detectar e corrigir automaticamente problemas que possam surgir nos aplicativos em execução no cluster. Essa capacidade é fundamental para garantir a confiabilidade e a disponibilidade contínua dos serviços. Vermos xemplos utilizando o <b>Startup Probes</b>, <b>Readiness Probes</b> e <b>Liveness Probes</b>.</p>

<h1>Liveness Probe</h1>

<p style="text-align: justify;">O "liveness probe" no Kubernetes é um mecanismo usado para determinar se um contêiner dentro de um Pod está em execução corretamente. Essa sonda é fundamental para garantir a saúde e a resiliência dos aplicativos no ambiente de contêineres. Aqui está um resumo sobre a "liveness probe":</p>

<ol>
  <li style="text-align: justify;">Definição</li>
  <ul>
    <li style="text-align: justify;">A liveness probe é uma sonda que verifica se um contêiner está em execução e saudável. Se a sonda detectar que o contêiner não está respondendo conforme esperado, o Kubernetes tomará medidas, como reiniciar o Pod, para tentar recuperar o contêiner.</li>
  </ul>
  <li style="text-align: justify;">Configuração no YAML do Pod</li>
  <ul>
    <li style="text-align: justify;">A liveness probe é configurada no arquivo YAML do Pod, especificando o tipo de sonda (por exemplo, HTTP, TCP, execução de comando), o caminho ou a porta a ser verificado e parâmetros adicionais, como atraso inicial e intervalo entre sondas.</li>
  </ul>
  <li style="text-align: justify;">Tipos de Probes</li>
  <ul>
    <li style="text-align: justify;">A liveness probe suporta diferentes tipos, incluindo:</li>
    <ul>
      <li style="text-align: justify;"><b>HTTP GET</b>: Faz uma requisição HTTP e verifica o código de resposta.</li>
      <li style="text-align: justify;"><b>TCP Socke</b>: Verifica se uma porta específica está abertas.</li>
      <li style="text-align: justify;"><b>Execução de Comando</b>: Executa um comando no contêiner e verifica o código de saída.</li>
    </ul>
  </ul>
  <li style="text-align: justify;">Finalidade</li>
  <ul>
    <li style="text-align: justify;">A principal finalidade da liveness probe é garantir que um contêiner em execução não esteja preso ou em um estado de falha. Se o contêiner não responder à sonda, o Kubernetes considerará o contêiner com falha e tomará medidas para tentar corrigir isso, como reiniciar o Pod.</li>
  </ul>
  <li style="text-align: justify;">Recuperação Automática</li>
  <ul>
    <li style="text-align: justify;">A liveness probe contribui para a recuperação automática em caso de falhas temporárias nos aplicativos. Se um contêiner entrar em um estado não responsivo, o Kubernetes tentará automaticamente reiniciar o Pod, esperando que o problema seja corrigido na próxima tentativa.</li>
  </ul>
</ol>

<p style="text-align: justify;">Vamos ver um exemplo de um pod utilizando o liveness probe.</p>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
    livenessProbe:
      httpGet:
        path: /produtos
        port: 80
        scheme: HTTP
      initialDelaySeconds: 10
      periodSeconds: 5
```

<p style="text-align: justify;">Neste exemplo o liveness probe é configurada para fazer uma requisição HTTP GET para o caminho /healthz na porta 80 do contêiner.</p>