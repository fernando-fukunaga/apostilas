# Apostila de Python
Nessa apostila, deixarei minhas anotações de estudo referente a linguagem Python, como a POO se aplica na lang, termos reservados ou exclusividades, coisas do tipo.

## Programação Orientada a Objetos
O Python pode não obrigar o dev a declarar classe no começo de qualquer código como a linguagem Java faz por exemplo, e por isso muitas pessoas não enxergam, mas o Python é praticamente todo feito em cima de OO. Tudo que você usa, até datatypes são objetos de classes. E com certeza a lang te dá todo o suporte para desenvolver com esse paradigma.

## Os pilares da POO
A maior convenção é que a POO tem quatro pilares:
* Abstração
* Encapsulamento
* Herança
* Polimorfismo

Algumas pessoa consideram apenas 3, pois afirmam que a abstração já está dentro do encapsulamento. Mas isso realmente faz sentido, até porque, no fim das contas, todos esses pilares conversam entre sí. O encapsulamento envolve abstração, que pode envolver herança e a herança que permite que aconteca o polimorfismo e por aí vai. No final um pode depender do outro, e todos os pilares conversam entre sí de forma harmoniosa. Segue os 4 pilares da POO e sua aplicabilidade em Python.

## Abstração
Existem duas vertentes do conceito de abstação na POO, o primeiro é simplesmente o ato de trazer objetos do mundo real para a programação, aquele velho exemplo da classe animal, objeto cachorro e objeto gato e por aí vai.

Agora um outro conceito que envolve a abstração consiste em criar uma classe abstrata, com métodos abstratos, mas o que isso significa? Uma classe abstrata não deve ser instanciada, assim como seus métodos não devem ser chamados diretamente. A classe abstrata serve literalmente apenas para ser usada como modelo para as suas subclasses, que aí sim, podem ter objetos. Os métodos abstratos devem ser obrigatóriamente escritos nas subclasses.

Isso acontece quando subclasses diferentes de uma classe mãe, querem utilizar o mesmo método mas com algumas diferenças. Por exemplo, todos os animais fazem algum barulho, mas dependendo do animal esse barulho é diferente. Então considere uma classe abstrata Animal que tem o método abstrato make_sound. O método abstrado deve SEMPRE ser definido sem implementação alguma, apenas um pass. As subclasses de Animal devem obrigatóriamente ter todos os métodos abstratos, mas agora eles podem ter implementações diferentes de acordo com sua necessicade.

As outras langs de POO geralmente possuem aquele termo chamado interface para aplicar esse conceito, como Java e PHP. Mas no Python não há o termo interface, então para criar uma interface usamos a lib ABC.

A lib ABC é uma lib built-in. E dessa lib, você usará os módulos ABC e abstractmethod. Segue exemplo de uso:
```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def make_sound(self):
        pass

class Dog(Animal):
    def make_sound(self):
        print("Woof!")
```
Não se esqueça de fazer a classe abstrata herdar de ABC. Para criar um método abstrato é só usar o decorator @abstractmethod e no processamento deixar só um pass, pois cada subclasse poderá usar esse método de um jeito um pouco diferente. o método abstrato DEVE estar nas subclasses mas como você viu, não precisa do decorator nelas também.

## Encapsulamento
Encapsular a sua classe significa proteger a mesma, principalmente os seus atributos. A prática que mais simboliza o encapsulamento na POO é a questão de private e public, igual vemos no Java. Os atributos de uma classe devem sempre ser todos privados, para não serem acessados diretamente, e aí você cria os métodos públicos getter e setters para poder acessar.

Você não pode enfiar a mão no bolso de alguém e ver o seu documento, você deve perguntar o nome da pessoa, é isso que os getters e os setters fazem, e em Python, não temos os termos public e private. Tudo por padrão é público no Python, e para dizer que é privado você coloca dois underscores na frente, e.g., para definir que o atributo saldo será privado você muda seu nome para __saldo.

Você também deve sempre simplificar o código criando funções para executar mais de uma função, para simplificar a chamada, encapsular a lógica, exemplo: função transfere é composta pela função saca de conta 1 seguida por deposita em conta 2.

### Getters e setters
Getter e setter são prefixos nas funções para especificar metodos de get ou set, exemplo: set_nome(), get_idade(). Esse é o jeito indicado para acessar atributos de uma classe, seguindo o encapsulamento.

### @property
Usando o @property antes de um getter, você faz parecer que está acessando diretamente um atributo privado usando objeto.atributo sem parenteses, mas na verdade, por debaixo dos panos você está chamando um objeto.get_atributo();

### @atributo.setter
Usando esse decorator você faz o mesmo efeito do property mas para um setter, no lugar de "atributo" escreva o atibuto que você quer modificar, exemplo: limite ou saldo.

### Métodos privados
Use o double underscore antes de um método para torná-lo privado e o mesmo só pode ser utilizado dentro da classe dele. Muito usado para verificações por exemplo:
```python
def __pode_sacar(self, valor_saque):
    limite_de_saque = self.limite
    return valor_saque <= limite_de_saque

def saca(self, valor):
    if self.__pode_sacar(valor):
        self.__saldo -= valor
    else:
        print("Ultrapassou o limite amigão!")
```
Neste exemplo, uma classe ContaBancaria tem o método saca() que vai subtrair um valor do saldo da conta e possui um método privado __pode_sacar() que valida se há limite de saque suficiente para a transação, esse método de pode sacar é privado, só poderá ser usado dentro de outros métodos da classe, e não ser chamado por fora igual o saca(), entendeu?

### Métodos estáticos
Métodos estáticos são métodos que podem ser chamados sem precisar ter instanciado algum objeto de sua classe, geralmente é usado para atributos que serão de mesmo valor para todos os objetos de uma classe, exemplo:
```python
@staticmethod
def get_codigo_banco():
    return "001"
```
Em um código, você pode chamar esse método sem ter instanciado algum objeto de sua classe, e ele retornará o código do banco, pois será o mesmo para todas as contas que você criar, você nem precisa do self. Tome cuidado para não sair tacando staticmethod em todos os métodos da classe pois isso vai quebrar a orientação a objetos, saiba quando usar!

### Convenção dos Pythonistas
Tornar um atributo ou método privado utilizando dois underscores é uma boa prática para o encapsulamento e segurança do app. Porém, existe uma prática mais popular que é de colocar apenas 1 underscore, como em self._nome por exemplo. Essa discussão começou pois ao colocar dois underscores, acaba de fato atrapalhando algumas funcionalidades, é um pouquinho burocrático para usar o atributo, vamos dizer. E como isso aqui é Python e não Java (rsrs), precisamos cortar o máximo de burocracia possível. Existe uma polêmica no ar quanto a essa questão, alguns ainda preferem e acham a melhor prática colocar dois unerlines mas a maioria dos devs hoje colocam apenas um, isso não muda em nada na funcionalidade das paradas e é uma maneira de avisar à pessoa que está lendo o código que trata-se de algo privado, e por uma convenção entre os devs, ninguém mexe naquilo diretamente. É uma maneira de avisar que, mermão, se mexer pode quebrar a aplicação.

## Herança
Use herança para herdar atributos de uma classe mãe para as classes filhas, onde as filhas podem ter atributos específicos, mas também atributos iguais a todos os seus irmãos que herdam de uma mesma mãe.

### Método super()
Ao herdar uma classe, utilize o método super() para puxar o init da classe mãe, assim você importa os atributos e métodos da classe mãe que você quer na sua classe filha, e pode também adicionar coisas a mais:
```python
class Programa:
    def __init__(self, nome, ano):
        self.nome = nome
        self.ano = ano

class Filme(Programa):
    def __init__(self, nome, ano, duracao):
        super().__init__(nome, ano)
        self.duracao = duracao
```
Neste exemplo, Filme herda de Programa, a diferença é que a classe Filme tem o atributo duracao, que não teria numa série por exemplo. Então use o método super() para puxar os atributos nome e ano da classe mae e o self.duracao para pegar a duracao apenas do objeto de Filme. Após isso, também, você pode usar os métodos da classe Programa, como os getters e setters de nome e ano.

## Polimorfismo
O polimorfismo é uma vantagem que vem junto com a herança, trata-se de que não importa qual a classe sendo usada, contanto que essa classe herde de uma superclasse específica. Um código que espera uma superclasse, pode receber qualquer classe filha, reduzindo a quantidade de ifs as vezes. Por exemplo, você pode percorrer uma lista de filmes e series usando o laço for e o programa não vai se importar que Filme e Serie são classes diferentes, pois as duas herdam de programa.

Então você pode usar for programa in programas, se não fosse o polimorfismo você não poderia usar o for pois precisaria especificar a classe do programa como: Filme.vingadores ou Serie.breakingbad

## Agregação
Em programação orientada a objetos, a agregação é um conceito que descreve a relação entre duas classes, onde uma classe é composta de objetos de outra classe como parte de sua estrutura. Em Python, a agregação pode ser implementada através da criação de instâncias de objetos de uma classe dentro de outra classe.

Em outras palavras, a agregação é uma forma de composição, onde uma classe é composta por uma ou mais instâncias de outra classe, mas as instâncias da classe contida podem existir independentemente da classe que as contém.

Por exemplo, considere duas classes: "Livro" e "Autor". Cada livro é escrito por um ou mais autores. Nesse caso, a classe "Livro" pode conter uma ou mais instâncias da classe "Autor" como atributos.
```python
Copy code
class Autor:
    def __init__(self, nome):
        self.nome = nome

class Livro:
    def __init__(self, titulo, autores):
        self.titulo = titulo
        self.autores = autores

autor1 = Autor("José")
autor2 = Autor("Maria")
livro1 = Livro("Meu Livro", [autor1, autor2])
```
No exemplo acima, a classe "Livro" possui um atributo "autores", que é uma lista de instâncias da classe "Autor". Dessa forma, um livro pode ser escrito por um ou mais autores, e um autor pode ter escrito um ou mais livros.

A agregação é uma forma útil de modelar relacionamentos entre objetos em um programa orientado a objetos, permitindo que as classes sejam compostas de objetos de outras classes de maneira flexível e reutilizável.

## Atributos de classe vs atributos de instância
Uma classe pode ter dois tipos de atributos: atributos de classe ou instância. Os atributos de classe são atibutos definidos dentro do corpo da classe mas fora de qualquer método como o construtor da classe, portanto eles serão constantes e terão o mesmo valor para qualquer instância da classe, e não estarão atrelados diretamente a estas instâncias. Já os atributos de instâncias são os atributos normais que são definidos geralmente em um método construtor e terão valores diferentes para cada instância.

Imagine uma classe Baralho, que representa um baralho tradicional de cartas. Uma carta tem como atributos seu valor e seu naipe, mas, para ficar mais fácil de fazer comparações de qual carta é mais forte, esses atributos serão representados por números int, ao invés de str. Porém quero que exista uma forma de representar os objetos em forma de str legível para o usuário final, então criarei dois atributos de classe que são dict e associarei uma string para cada "código" de naipe e valor, se liga em como fica o código:
```python
class Baralho:
    def __init__(self, naipe, valor):
        self.naipe = naipe
        self.valor = valor

    naipe_names = {
        1: "Ouros",
        2: "Espadas",
        3: "Copas",
        4: "Paus"
    }

    valor_names = {
        1: "Ás",
        2: "2",
        3: "3",
        4: "4",
        5: "5",
        6: "6",
        7: "7",
        8: "8",
        9: "9",
        10: "10",
        11: "Valet",
        12: "Dama",
        13: "Rei",
    }

    def __str__(self):
        return f"{Baralho.valor_names.get(self.valor)} de {Baralho.naipe_names.get(self.naipe)}"


carta = Baralho(naipe=4, valor=13)
print(carta)
# a saída do console será:
# Rei de Paus
```
Repare que, no método "dunder str", utilizamos sintáxes diferentes para acessar valores, para acessar o atributo de **classe** valor_names, utilizamos a sintáxe NomeDaClasse.valor_names, e para acessar o atributo de instância valor, por exemplo, usamos a sintáxe objeto.valor. Portando, o atributo valor_names está atrelado a classe e o atributo valor está atrelado à instância de Baralho, ou seja, para chamar os dois seria algo como:
```python
Baralho.valor_names # retorna os nomes dos valores
carta.valor # retorna o valor em int da carta
```

## Função Assíncrona e await
Uma função assíncrona, ou "async function", é uma função especial em Python que permite que o código seja executado de forma assíncrona. Isso significa que, em vez de bloquear a execução do programa até que uma operação seja concluída, a função assíncrona permite que o programa continue a ser executado enquanto a operação está em andamento.

A função assíncrona é definida usando a palavra-chave "async" antes da definição da função. Ela é executada em um objeto asyncio.EventLoop, que é responsável por gerenciar a execução assíncrona.

Dentro de uma função assíncrona, você pode usar a palavra-chave "await" antes de uma chamada a outra função assíncrona ou a uma operação de E/S (como uma chamada a uma API ou a um banco de dados), para indicar que você deseja esperar até que a operação seja concluída antes de continuar com a execução da função. As vezes você precisa se algum valor ou dado para continuar com a função então você pede para ela "esperar" um pouco.

As funções assíncronas são muito úteis para lidar com operações de E/S demoradas ou bloqueantes, como requisições de rede ou acesso a banco de dados, sem bloquear a execução do programa. Elas permitem que o código seja executado de forma mais eficiente, melhorando o desempenho e a escalabilidade do programa.

Dentro de funções assíncronas, podemos usar tanto o await quando outros dois termos: async for e async with. Todas elas só podem ser usadas dentro de um async def (definição de função assíncrona). Todas essas atividades também podem ser chamadas de coroutines.

## Instanciando um objeto passando um dict como atributo
Considere a seguinte classe herdeira de BaseModel:
```python
from pydantic import BaseModel

class User(BaseModel):
    username: str
    password: str
    email: str
```
Ao instanciar algum usuário, neste caso, há uma maneira de passar uma variável que contém um dict inteiro ao invés de passar chave e valor, um por um. É só usar dois asteriscos ** que você passa apenas o nome de uma variável com um dicionário inteiro:
```python
dict = {
    "username": "usuario33"
    "password": "senha123"
    "email": "usuario123@email.com"
}

objeto_usuario = User(**dict)
```
Ao fazer isso, você pode digitar apenas **dict ao invés de escrever como parâmetro o dicionário inteiro, isso pode ser muito útil caso você queira usar uma mesma estrutura de dicionário para vários objetos diferentes, é só usar variáveis para os valores no dicionário dict.

## Criptografia Assimétrica
```terminal
openssl req -new -x509 -nodes -newkey ec:<(openssl ecparam -name secp384r1)         -keyout private.key -out public.crt         -days 1095 -subj "/CN=teste"
```
Use esse comando para gerar chave privada e pública e trabalhar com chaves assimétricas.

## Definindo tipagem com type hints e usando a lib typing
No Python, você pode definir o tipo de dado que uma variável deve receber, por exemplo, vou criar uma variável nome_cliente e eu PRECISO que seja do tipo string, então:
```python
nome_cliente : str = "Fernando"
```
Isso irá mostrar que a variável deve receber uma string. Um outro exemplo interessante é definir os tipos de dados que os parâmetros de uma função deve receber:
```python
def somar(num1: int, num2: int):
    return num1 + num2
```
Aqui vemos uma simples função de soma mas eu especifiquei que esse método só pode ser chamado com 2 números inteiros, então não dá para chamar com uma string.

O nome dessa prática é type hinting, onde você usa anotações de tipagem como : str para dar "dicas" de que tipo de dado será trabalhado em certa variável, classe ou função. Isso aumenta muito a qualidade e segurança do código, além de deixar o código mais legível para outros devs.

Agora uma outra coisa bem legal em type hinting, é a famosa setinha ->. Para que ela serve? Bom, essa setinha é utilizada geralmente depois da definição de um método, para dizer que tipo de dado esse método deve retornar, exemplo:
```python
def hello_world(nome: str) -> str:
    return f'Olá, mundo! Olá, {nome}!'
```
Com isso, você crava que a função hello_world() só pode retornar um dado do tipo str, isso é muito útil.

Outra forma que gostei muito de usar a setinha, é para dizer que uma função deve retornar uma instância de classe, ou seja, você demanda que uma função deve retornar um objeto de uma classe, da seguinte maneira:
```python
class Cliente:
    def __init__(nome: str, idade: int):
        self.nome = nome
        self.idade = idade

def cria_cliente(nome: str, idade: int) -> Cliente:
    # A setinha que vc viu aí em cima apontando para a classe cliente, diz que essa função DEVE retornar uma intância/objeto dessa classe
    cliente = Cliente(nome, idade)
    return cliente
```
Olha só que legal, assim podemos podemos criar uma função que retorna um objeto, wow. Isso é muito utilizado em projetos profissionais que usam POO!

Agora, quando quiser usar essa funcionalidade em casos mais complexos como com listas, por exemplo, você deve importar a biblioteca typing, uma biblioteca built-in do Python que contém vários auxiliares denominados notações que irão te ajudar a trabalhar com situações mais específicas. Por exemplo, ao instanciar uma lista, posso importar a notação List da lib typing e definir para a lista receber somente determinado tipo de dado:
```python
from typing import List

class Cliente:
    def __init__(nome: str, idade: int):
        self.nome = nome
        self.idade = idade

cliente_1 = Cliente("Fernando", 20)
cliente_2 = Cliente("João", 34)
cliente_3 = Cliente("Pedro", 25)

# Aqui usarei a feature para dizer que lista_1 só pode receber strings
lista_1: List[str] = ["Fernando", "João", "Pedro"]
# Aqui, usarei para dizer que a lista só pode receber objetos da classe Cliente, sacou??
lista_2: List[Cliente] = [cliente_1, cliente_2, cliente_3]
```
Apesar de, se você vasculhar o código da lib typing, perceber que as notações são declaradas com def, é importante frizar que as notações do módulo typing NÃO são funções em sí, a semelhança entre notações do typing e funções comuns do python termina do termo def, mas de resto, até a sintaxe é diferente. Por exemplo, logo de cara você já percebe que elas começam com letras maiúsculas, e todos sabemos que funções comuns do Python são escritas em snake_case, sem letra maiúscula. Outra coisa é que os "parâmetros" dessas notações (que também podem ser chamadas de construções) são passados dentro de colchetes e não parênteses como esperamos.

### Union e Optional: valores opicionais (ou não?)
Falando um pouco mais da lib typing, outras notações interessantes são Union e Optional. O Union, serve para permitir que uma determinada variável receba dados de mais de um tipo diferente. Exemplo, considere que id, possa receber tanto um número int quanto uma string:
```python
from typing import Union

def imprime_id(id: Union[int, str]):
    print(id)
```
Assim, podemos permitir mais de um tipo de dado em uma variável. Outra maneira mais simples de fazer isso, disponível a partir do Python 3.10 é simplesmente fazer isso:
```python
def imprime_id(id: int | str):
    print(id)
```
É bem mais simples e o bom é que nem precisa importar nada :D

A outra notação desse tópico é a Optional, que é muuuito parecida com a Union mas com a diferença que um dos tipos é fixado no None, ou seja, você usa o Optional quando quer dizer que uma variável pode receber um int ou str por exemplo, ou valor nulo, segue a sintaxe do Optional:
```python
from typing import Optional

def imprime_id(id: Optional[int]):
    print(id)

# nesse caso, Optional[int] seria a mesma coisa que Union[int, None] que é a mesma coisa que int | none, tendeu?
```
Porém, **ATENÇÃO!!!** Apesar do nome sugestivo, não se engane: a notação Optional NÃO quer dizer que o atributo é opcional. Muito menos a notação Union. O que define que o parâmetro de uma função ou atributo de um construtor é opcional, é o fato de ele ter valor padrão ou não, simplemente. Então no exemplo a seguir:
```python
def imprime_id(id: int = 0):
    print(id)
```
Nesse caso, aí sim podemos dizer que o parâmetro id é opcional, pois ele pode receber qualquer valor int, mas se você quiser chamar a função sem nenhum parâmetro, a variável id vai receber o valor padrão de 0, tornando esse parâmetro de fato opcional! A notação Optional apenas diz que a variável pode ser nula, mas ela continua sendo **obrigatória**.

## with / as
with é uma declaração em Python que é usada para trabalhar com recursos externos que precisam ser abertos antes do uso e, em seguida, fechados após o uso. O bloco with garante que o recurso será fechado ao final, mesmo que ocorra uma exceção.

O with as é uma extensão do with que permite atribuir o recurso aberto a uma variável para facilitar o seu uso dentro do bloco with. É comumente usado para abrir arquivos e manipulá-los dentro de um bloco with, garantindo que o arquivo seja fechado automaticamente ao final.

Por exemplo, podemos abrir um arquivo usando with as da seguinte maneira:
```python
with open('arquivo.txt', 'r') as f:
    conteudo = f.read()
    print(conteudo)
```
Nesse exemplo, o arquivo é aberto em modo de leitura e seu conteúdo é armazenado na variável conteudo. Ao final do bloco with, o arquivo é fechado automaticamente pelo Python, sem a necessidade de chamar o método close() explicitamente.

## Usando null no Python
No python, para dizer que uma variável tem valor nulo, não existe a palavra null, usamos None para esses casos:
```python
variavel = None
# essa variavel tem valor nulo :D
```

## Modularização: a pasta infra
Em um projeto profissional do Python, é comum usarmos uma pasta chamada infra para colocar códigos que envolvam bibliotecas/frameworks que tenham papéis de protagonismo em seu sistema, por exemplo, é comum na pasta infra guardar códigos que envolvam o ORM que você vai usar, bibliotecas que conversam com serviços de nuvem como a AWS e coisas desse tipo.

Agora, não é comum usar a pasta infra para o pytest, pois ele já vai ter a pasta tests na raíz do projeto, e não é comum também usar para frameworks de web dev, como Django, Flask e FastAPI, até porque eles já são os caras que estruturam o projeto em sí hehehe.

## O comando yield
O comando yield é bem parecido com o return, ambos devem ser usados dentro de uma função. A grande diferença é que, quando uma função chega em um return, ela retorna esse valor e a função ACABA, ela se encerra e se for chamada novamente começará do início. Agora o yield, retorna uma valor porém ele **pausa** a execução da função e depois da chamada, continua a execução da onde parou, sendo possível assim, retornar mais de um valor na mesma função. Mas para que serveria isso?

Uma função que usa yield é chamada de "generator function", ela é uma função que gera valores, e quando você chama ela com uma variável, essa variável será do tipo 'generator', e você pode imprimir ela como lista, por exemplo. Você pode gerar iteráveis, basicamente. Funções geradores combinam muito com laços de repetição, inclusive.

Confira esse script onde a função filtra_impares() usa do yield para imprimir todos os números ímpares qua existem antes de um número x:
```python
def filtra_impares(numeros: int):
    for numero in range(numeros):
        if numero%2 != 0:
            yield numero

numeros_impares = filtra_impares(20)

print(type(numeros_impares))
print(list(numeros_impares))
```
Como pode ver, a cada repetição do laço for, a função vai verificar se o resto da divisão do número por 2 é diferente de zero, se for verdadeiro, o número é ímpar e então ele itera o mesmo na variável numeros_impares que foi quem a chamou. Mas depois disso, ao invés de começar o laço for novamente, ele vai continuar o mesmo de onde parou, até passar por todos os 20 números. Então eu peço para ele imprimir o tipo da variável numeros_impares e depois para imprimi-la como lista, o output será o seguinte:
```terminal
<class 'generator'>
[1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
```
Se nesse caso eu usasse o return ao invés do yield, assim que o operador lógico encontrasse desse verdadeiro, a função iria se encerrar, e então a variável numeros_impares ia acabar recebendo apenas o número 1.

Mas o yield, não serve apenas para gerar iteráveis gigantes e listas. Ele pode ser usado em um caso de try/finally, onde após o retorno do dado, o programa ainda precisa voltar na função pra fazer mais uma coisnha. O melhor exemplo disso é o caso do SQLAlchemy, onde, para cada vez que eu preciso fazer uma consulta ou escrita no banco de dados, tenho que pegar uma session e fechar a mesma depois da operação. A melhor forma de fazer isso é usando yield e o try/finally:
```python
def obter_sessao():
    session = SessionLocal()

    try:
        yield session

    finally:
        session.close()
```
Ele vai retornar a sessão para ser utilizada na operação com o banco, mas após essa operação acontecer, deve voltar para realizar o session.close(). Então usamos o yield nesse caso não para criar uma lista ou algo do tipo. Mas para "salvar" o progresso de execução de uma função e poder voltar nela depois de onde paramos.