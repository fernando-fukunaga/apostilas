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