## Abstração e mini-mundo
O processo de abstração consiste em pegar uma situação do mundo real e transferir, abstrair para o mundo do banco de dados, o mais importante na abstração, é saber abstrair aquilo que realmente é importante para a sua regra de negócio, e não extrair tudo.

Por exemplo, se você vai escrever um cronograma, uma rotina para o seu dia, você vai escrever o que vai fazer em cada segundo do dia? Com certeza não, você apenas destacará os horários chave onde você deve encerrar uma atividade e terminar outra, por exemplo, você irá anotar: acordar às 8h, começar a trabalhar às 9h, reunião às 10h, e assim por diante.

A abstração é a mesma coisa, num fluxo do tipo: vendedor >vende> produto >compra> cliente. Podemos já fazer uma modelagem de dados e pegaremos apenas os dados mais importantes, como por exemplo o CPF da pessoa que compra, mas não vou precisar saber em que nível ela está no LOL por exemplo, entendeu? rsrsrs

Isso compõe o conceito de mini-mundo: representação da estrutura do negócio, espelhando recortes do mundo real. Através desta etapa, entendemos como o banco de dados será estruturado.

## Entidade forte x fraca
Uma entidade forte no banco de dados é a entidade que não depende necessáriamente de outra para existir ea entidade fraca só existe perante a existencia de uma entidade-pai que seja forte. Além disso as entidades fortes sempre têm uma chave primária enquanto as fracas não possuem chave primária, mas possuem uma chave parcial que discrimina de maneira única as entidades fracas. Ou talvez uma chave estrangeira, que é a chave primária de sua entidade forte, por exemplo, um item de compra não terá chave primária mas terá uma chave estrangeira que é a chave primária da compra a qual ele está relacionado.

## Atributos de valor atômico e multivalorado
Um atributo de uma entidade pode ter um único valor, como o CPF de uma pessoa, que só tem 1 CPF mesmo. Ou multivalorado, como telefone, posso dizer que o atributo telefone de uma entidade pessoa pode ter de 1 a 2 valores, porque a pessoa pode optar por informar o seu telefone pessoal e um comercial, por exemplo.

### Atributo composto
No universo dos atributos multivalorados, podem existir atributos compostos que podem ser divididos em partes menores que representam outros atributos, como o endereço de alguém. Ele pode ser subdividido em atributos menores, como: cidade, estado, rua, CEP.

## Atributos armazenados e derivados
Um atributo armazenado é aquele que você simplesmente capta e registra no banco, como um CPF de uma pessoa. Agora um atributo derivado é aquele que você obtém a partir de operações envolvendo outros atributos armazenados, por exemplo, se eu tenho a data de nascimento de um cliente e a data de hoje, posso gerar um atributo derivado que seria a idade do cliente, fazendo uma operação matemática de subtração da data atual com a que ele nasceu.