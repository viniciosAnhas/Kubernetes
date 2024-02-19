<div align="center">
  <div>
    <img height = "150" width = "150" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/kubernetes/kubernetes-plain-wordmark.svg" />
  </div>
</div>

<p style="text-align: justify;">O recurso Endpoint é usado para associar serviços a conjuntos específicos de Pods. Ele fornece uma abstração que representa um grupo de IP e portas que podem ser alcançados, geralmente correspondendo aos Pods selecionados por um Service. Aqui estão os principais pontos sobre o recurso Endpoint:</p>

<ol>
  <li style="text-align: justify;">Definição</li>
  <ul>
    <li style="text-align: justify;">Um Endpoint é um recurso no Kubernetes que define um conjunto de IP e portas associadas a um Service.</li>
    <li style="text-align: justify;">Ele representa os pontos de extremidade, ou seja, os endereços de rede, para os quais um Service encaminha o tráfego.</li>
  </ul>
  <li style="text-align: justify;">Associação com Services</li>
  <ul>
    <li style="text-align: justify;">Os Endpoints são automaticamente gerenciados pelo sistema Kubernetes e associados aos Services. Cada Service tem um conjunto correspondente de Endpoints.</li>
  </ul>
  <li style="text-align: justify;">IPs e Portas</li>
  <ul>
    <li style="text-align: justify;">Os Endpoints incluem uma lista de IPs e portas que correspondem aos Pods selecionados pelo Service. Isso permite que o Service encaminhe o tráfego para esses endereços.</li>
  </ul>
  <li style="text-align: justify;">Atualização Dinâmica</li>
  <ul>
    <li style="text-align: justify;">Os Endpoints são dinamicamente atualizados conforme os Pods associados aos Services são criados, atualizados ou removidos. Isso garante que os Endpoints reflitam o estado atual dos Pods.</li>
  </ul>
  <li style="text-align: justify;">Descoberta de Serviços</li>
  <ul>
    <li style="text-align: justify;">Os Endpoints são fundamentais para a descoberta de serviços no Kubernetes. Quando um Service é acessado, o Kubernetes redireciona o tráfego para os Endpoints associados, que, por sua vez, encaminham o tráfego para os Pods reais.</li>
  </ul>
</ol>

<p style="text-align: justify;">Embora os Endpoints sejam gerenciados automaticamente pelo Kubernetes, eles não precisam ser configurados diretamente pelos usuários. No entanto, os usuários podem visualizar os Endpoints associados a um Service usando o comando <i>kubectl get endpoints</i> <nome-do-service>.</p>

```bash
kubectl get endpoints 
```

<p style="text-align: justify;">Em resumo, os Endpoints são uma parte essencial da arquitetura de rede do Kubernetes, representando os pontos de extremidade para os quais os Services encaminham o tráfego. Eles garantem a correta descoberta e roteamento do tráfego para os Pods associados.</p>