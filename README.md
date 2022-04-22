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

# Modelo de solução proposto

![SolucaoSaldo-Arquitetura da Solução drawio](https://user-images.githubusercontent.com/46346047/164729009-19a7a1bc-772e-461f-ab30-ed6a2b5d55e7.png)

# Padrões arquitetônicos utilizados
# 1 - Gateway
Agregar várias solicitações individuais em uma única solicitação. Esse padrão é útil quando um cliente deve fazer várias chamadas para diferentes sistemas de back-end ao executar uma operação.

# 2 - Arquitetura orientada a eventos
Em um sistema orientado a eventos, os componentes de captura, comunicação, processamento e persistência de eventos formam a estrutura básica da solução. Isso é diferente do modelo tradicional orientado a solicitações.
Utilizado nos pontos de captura da entrada da transação e finalização fornecendo assim uma estrutura robusta e tolerante a falhas, uma vez que mesmo os consumidores não atendendo as requisições as mensagens não são perdidas e os produtores continuam ativos sem impactos em suas funções.

# 3 - Cache Aside
Carregar dados sob demanda em um cache de um armazenamento de dados para melhorar o desempenho e também ajudar a manter a consistência entre dados mantidos no cache e os dados no armazenamento de dados subjacente.
Para acelerar o processo de gravação da transação na operação, é necessário acessar o saldo corrente e realizar o débito ou crédito no valor, assim para evitar uma busca no banco de transações foi implementado o saldo no cache em memória, sendo que a conclusão da transação deve fronecer o novo saldo em cache.

# 4 - Federated Identity
Delegar autenticação a um provedor de identidade externa. Isso pode simplificar o desenvolvimento, minimizar a necessidade de administração de usuário

# Arquitetura de software

