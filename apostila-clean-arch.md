<img src="images/clean-architecture.jpg">

## Inversão de Dependência
Corresponde ao D do SOLID, preconiza que as classes do programa devem depender de abstrações e não implementações de outras classes, elas devem trabalhar em cima de interfaces ou classes abstratas, dessa forma a implementação pouco importa e eu posso mudar as libs externas e bancos de dados que a aplicação usa o quanto eu quiser sem afetar o core desta aplicação que vai sempre olhar apenas para o contrato estabelecido, vai olhar **o que** faz e não **como** faz.

## Interface Adapters
Cria interfaces, contratos para a camada de use cases utilizar, protegendo a use cases de saber o que rola nas camadas mais externas, para que a mesma saiba apenas o quê e não como.

## Camada Core da Aplicação
Aqui você vai encontrar majoritariamente interfaces, tudo é bem mais abstrato, você raramente vai encontrar lógica e algoritimos nessa parte, apenas interfaces para o Services e afins seguirem.
Você terá entities que são objetos de negócio e use cases que serão interfaces que definem O QUE a sua aplicação vai fazer, que erros podem ocorrer, definindo exceções também.

## Rascunho
### Core da aplicação (entities e use cases):
Eu tenho um use case, que lança exceções, tudo em Java puro, pois lá nas camadas extrernas o Spring vai detectar as exceções e transformar em erro HTTP.
A implementação desse use case consome métodos de uma outra interface que representa o repositório, sem implementação. A implementação desta interface de repository acontecerá na última camada (infra), lá sim pode chamar JPA, coisas do tipo. Sendo essa a única implementação da interface de repository, a impl do use case vai saber que tem que chamar a coisa certa.
### Camada de aplicação:
É basicamente a implementação das interfaces de use cases, podem também ser chamadas de Services. Você pode colocar a camada de aplicação logo dentro do core, dessa forma a implementação dos use cases já ficarão logo ao lado de suas interfaces, porém ainda sem chamar nada externo, apenas interfaces. Ou você pode deixar a camada de aplicação separada, sendo uma intermediaria entre o core a as partes mais externas, uma ponte.

Se você quiser, as implementações de use case podem já ser Services do Spring, usar Spring já nessa camada é uma escolha opcional, já que muitos acreditam que o framework web é importante demais para o funcionamento de uma aplicação para ser incluso tão fortemente no clean arch e que se você quiser mudar o framework web é mais fácil simplesmente reescrever o programa inteiro de qualquer maneira. Isso vai da interpretação de cada um sobre o clean arch.

Se a camada de aplicação estiver já fora do core, não creio que seja um problema introduzir as implementações de use cases como Services do Spring, pois o acoplamento segue muito baixo e você já vai paulatinamente criando essa ponte entre domínio da aplicação e dependências externas.

Dentro de um Service, você pode chamar os métodos do repositório por meio de uma outra interface chamada gateway (porta de entrada entre mundo interno e externo). E aí o gateway é quem vai implementar algo que chame lá o JPA ou JDBC, etc.
### Camada de Interface Adapters
Basicamente a camada de interface adaptors apenas defini interfaces que mostram para a camada de aplicação o que os serviços externos fazem, sem que ninguém saiba exatamente qual banco, ORM e libs o projeto está usando exatamente, permitindo até que estes sejam alterados a qualquer momento sem dificuldades. É comum também escrever os contollers nesta camada, seja com o framework web ou apenas uma interface para o controller do framework web realmente só implementar na camada de infra, é uma escolha arbitrária que vai depender da sua interpretação, como dito anteriormente. As interfaces de gateway mencionadas anteriormente, também são muitas vezes introduzidas nesta camada.

Essa é a ponte entre o core da aplicação e a camada de infra, então o nome adapters não é em vão, é uma camada que tem como principal função adaptar realmente os detalhes técnicos externos para utilização do core da aplicação que idealmente só vai ter Java puro, a intenção é você poder alterar qualquer coisa na camada de infra sem afetar a camada de adapters e muito menos o core. já que a camada de adapters mostra apenas abstrações e as implementações específicas de ORM, libs variadas e afins ficam na camada infra.
### Camada de infraestrutura:
E finalmente, esta camada é responsável por interagir com sistemas, serviços e estruturas externas. É onde vamos implementar a comunicação com outros serviços, como o banco de dados, AWS, libs de segurança, e até o framework web principal se for a sua ideia.

Os componentes da camada de infraestrutura devem implementar as interfaces definidas nas camadas anteriores, então essa é a camada mais externa. A implementação dos repositories costumam ficar nessa camada.

## Onde ficam os controllers?
Creio que os controllers são os componentes que mais vão aparecer em lugares diferentes das aplicações. Eles podem ter uma pasta só para eles dentro do src da app, eles podem ser implementados já na camada de Interface adapters, ou eles podem ser implementados realmente só na camada de infra, deixando apenas uma interface na camada anterior.

Tudo depende muito de uma coisa muito importante: se você quer ou não que o seu framework web (Spring, FastAPI, Express) esteja incluso nesses princípios de desacoplamento e inversão de dependência. Teoricamente, deveria estar incluso, pois querendo ou não, o Spring por exemplo **é** um componente **externo**, portanto os controllers do Spring devem ser implementados somente na camada de infra, sabemos que a maioria emagadora das aplicações Java no mercado utilizam Spring mas pode acontecer de repentinamente a sua empresa aprovar um projeto que irá migrar sua aplicação para Quarkus, e aí? Como seria essa migração se sua aplicação estiver totalmente acoplada ao Spring? Provavelmente teria que escrever tudo de novo, o que seria uma dor de cabeça.

Porém, por outro lado, há pessoas que argumentam que o framework web é algo muito base de uma aplicação backend, que dita toda a forma com a qual essa aplicação é desenvolvida, ainda mais em frameworks como o Spring onde basicamente todos os componetes funcionam em cima dele com as anotações e tal, controllers, services, repositories, ainda tem os Beans. É defendido por essas pessoas que se for ter que mudar o framework para Quarkus, por exemplo, já ia ter que reescrever a aplicação toda de qualquer forma então não faz sentido incluir o Spring no clean arch, e sim incluir apenas coisas que têm mais chances de serem alteradas, muitas vezes por cortes de gastos talvez, como por exemplo SGDBs e provedores cloud. E é um argumento válido, também.

Nesse caso, os componentes Spring como controllers, poderiam ser implementados na camada de adapters, e alguns outros componentes como os services, já poderiam ser implementados até mesmo na camada de aplicação, fazendo com que a implementação direta dos use cases já seja com o Spring. Tem gente até que já usa coisas do Spring diretamente no core da app. Enquanto na camada de infra ficariam realmente apenas as outras dependências externas: ORM, libs de segurança, SDKs de provedores cloud, drivers de bancos de dados, etc. Pois entende-se que essas coisas podem mudar com mais frequência e ditam menos o funcionamento de uma aplicação backend.

Portanto, ao desenvolver uma aplicação com clean arch, escolha aquilo que faz mais sentido para você e para o contexto da sua aplicação: framework web incluído ou não, é uma escolha sua.