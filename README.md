# Fluxo de caixa
Sistemas para controle de fluxo de caixa, para fins didáticos.

# Contexto
Controle de fluxo de caixa diário com lançamentos (débitos e créditos), e relatório contendo saldo diário consolidado.

Considerando a natureza didática será apresentada uma solução que extrapola o limite da simplicidade do problema, visando a demonstração e aplicabilidade de tecnologias diversas para a resolução do problema proposto, desta forma a solução apresentará soluções que por vezes serão mais complexas que o necessário, entretanto tais escolhas foram definidas de forma proposital apenas para exemplificar as possibilidades técnicas disponíveis.

# Arquitetura proposta
A arquitetura apresentada pode ser dividida em duas partes sendo a primeira focada na solução do problema do ponto de vista de componentes estruturais e de tecnologia tendo como proposta principal a criação de uma base tecnologica sólida permitindo contemplar requisitos não funionais como: 
- escalabilidade, permite oas serviços escalarem de forma horizontal e vertical afim de suportar aumento de carga significativo dos serviços 
- observabilidade, restrear e monitorar a atividade de todo ecossistema funcional da solução
- reseliência, suportar varições de utilização sem prejudicar o funcionamento dos serviços

## Modelo de solução proposto

![SolucaoSaldo-Arquitetura da Solução drawio](https://user-images.githubusercontent.com/46346047/164729009-19a7a1bc-772e-461f-ab30-ed6a2b5d55e7.png)

## Padrões arquitetônicos utilizados
#### 1 - Gateway
Agregar várias solicitações individuais em uma única solicitação. Esse padrão é útil quando um cliente deve fazer várias chamadas para diferentes sistemas de back-end ao executar uma operação.

#### 2 - Arquitetura orientada a eventos
Em um sistema orientado a eventos, os componentes de captura, comunicação, processamento e persistência de eventos formam a estrutura básica da solução. Isso é diferente do modelo tradicional orientado a solicitações.
Utilizado nos pontos de captura da entrada da transação e finalização fornecendo assim uma estrutura robusta e tolerante a falhas, uma vez que mesmo os consumidores não atendendo as requisições as mensagens não são perdidas e os produtores continuam ativos sem impactos em suas funções.

#### 3 - Cache-Aside
Carregar dados sob demanda em um cache de um armazenamento de dados para melhorar o desempenho e também ajudar a manter a consistência entre dados mantidos no cache e os dados no armazenamento de dados subjacente.
Para acelerar o processo de gravação da transação na operação, é necessário acessar o saldo corrente e realizar o débito ou crédito no valor, assim para evitar uma busca no banco de transações foi implementado o saldo no cache em memória, sendo que a conclusão da transação deve fronecer o novo saldo em cache.

#### 4 - Federated Identity
Delegar autenticação a um provedor de identidade externa. Isso pode simplificar o desenvolvimento, minimizar a necessidade de administração de usuário

# Arquitetura de software
Para implementação das api's utilizaremos o conceito da **Clean Arquiteture** porposto por Robert Cecil Martin (“Uncle Bob”) promovida em seu livro Clean Architecture: A Craftsman’s Guide to Software Structure.
O Objetivo central dessa abordagem é forneceruma metodologia a ser usada na codificação, a fim de facilitar o desenvolvimento códigos, permitir uma melhor manutenção, atualização e menos dependências, em outras palavras, é fornecer aos desenvolvedores uma maneira de organizar o código de forma que encapsule a lógica de negócios, mas mantenha-o separado do mecanismo de entrega.

![image](https://user-images.githubusercontent.com/46346047/164737767-fb443512-9c02-457d-ad1b-5f8ba045b975.png)

Algumas vantagens de utilizar uma arquitetura em camadas:

 - Testável. As regras de negócios podem ser testadas sem a interface do usuário, banco de dados, servidor ou qualquer outro elemento externo.
 
 - Independente da interface do usuário. A interface do usuário pode mudar facilmente, sem alterar o restante do sistema. Uma UI da Web pode ser substituída por uma UI do console, por exemplo, sem alterar as regras de negócios.

 - Independente de banco de dados. Você pode trocar o Oracle ou SQL Server, por Mongo, BigTable, CouchDB ou qualquer outro. Suas regras de negócios não estão vinculadas ao banco de dados.

 - Independente de qualquer agente externo. Na verdade, suas regras de negócios simplesmente não sabem nada sobre o mundo exterior, não estão ligadas a nenhum Framework.

 A separação de camadas poupará o desenvolvedor de muitos problemas futuros com a manutenção do software, a regra de dependência bem aplicada deixará o sistema completamente testável.

Continua....

 # Componentes e Aplicações
 Para suportar todo o ambinete poroposta do modelo conceitual porposto foram definidos os segunites componetes:
  - **Docker**, ambiente de conteiner para facilitar a instalação e testes locais das aplicações
  - **Rabbit MQ**, Message broker utilizado na captura dos eventos do ecossistema
  - **Postgres**, Banco de dados transacional usado na aplicação
  - **Cassandra**, Banco de dados utilizado para modelos analíticos
  - **REDIS**, Banco de dados em memoria para cache do saldo corrente
  - **NodeJS(16)**, micro serviço para entrada das transações (gateway) - fluxo-de-caixa-gatway
  - **Java**, Back end voltado a atulização do saldo e armazenamento das transações no banco Postgres (... TODO)
  - **Python**, Geração de relatório analítico dasa transações diárias ralizadas (...TODO)

  ## Criação do ambiente
  Para criação do ambiente é necessário possuir o Docker e Docker Compose instalados e executar o comnado:
   -  docker-compose -f docker-compose-fluxo-de-caixa.yml up -d
   O comando irá baixar e configurar todo o ambiente para execussão das aplicações (**neste momento só estará disponível a captura da transação no gateway, demais serviçoes em construção**)

   --Existe um bug na subida dos serviços, onde mesmo colocando a relação de depnedência entre a aplicação e o broker, a aplicação ainda poderá subir antes do broker causano um erro de conexão, neste momnento trabalheremos com workaround de, caso o serviço não inicie, realizar um start manual usando docker start (CONTAINER) ou executando o mesmo comando inicial.
