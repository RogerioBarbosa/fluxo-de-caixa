# fluxo-de-caixa
Sistemas para controle de fluxo de caixa, para fins didáticos.

# Contexto
Controle de fluxo de caixa diário com lançamentos (débitos e créditos), e relatório contendo saldo diário consolidado.

Considerando a natureza didática será apresentada uma solução que extrapola o limite da simplicidade do problema, visando a demonstração e aplicabilidade de tecnologias diversas para a resolução do problema proposto, desta forma a solução apresentará soluções que por vezes serão mais complexas que o necessário, entretanto tais escolhas foram definidas de forma proposital apenas para exemplificar as possibilidades técnicas disponíveis.

# Arquitetura proposta
A arquitetura apresentada pode ser dividida em duas partes sendo a primeira focada na solução do problema do ponto de vista de componentes estruturais e de tecnologia tendo como proposta principal a criação de uma base tecnologica sólida permitindo contemplar requisitos não funionais como: 
- escalabilidade, permite oas serviços escalarem de forma horizontal e vertical afim de suportar aumento de carga significativo dos serviços 
- observabilidade, restrear e monitorar a atividade de todo ecossistema funcional da solução
- reseliência, suportar varições de utilização sem prejudicar o funcionamento dos serviços

# Modelo proposto

![SolucaoSaldo-Arquitetura da Solução drawio](https://user-images.githubusercontent.com/46346047/164729009-19a7a1bc-772e-461f-ab30-ed6a2b5d55e7.png)

# Padrões arquitetônicos utilizados
# 1 - Gateway

# 2 - Arquitetura orientada a eventos

# 3 - Cache Aside

# 4 - Federated Identity
