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