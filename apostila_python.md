## Encapsulamento
Encapsular os atributos;

Criar atributos privados com dois underlines, ex: __saldo, para que os atributos só possam ser acessados ou alterados por meio do método correspondente, e não diretamente;

Você não pode enfiar a mão no bolso de alguém e ver o seu documento, você deve perguntar o nome da pessoa;

Simplificar o código criando funções para executar mias de uma função, para simplificar a chamada, encapsular a lógica, exemplo: função transfere é composta pela função saca de conta 1 seguida por deposita em conta 2.

### Getters e setters
Getter e setter são prefixos nas funções para especificar metodos de get ou set, exemplo: set_nome(), get_idade();

### @property
Usando o @property antes de um getter, você faz parecer que está acessando diretamente um atributo privado usando objeto.atributo sem parenteses, mas na verdade, por debaixo dos panos você está chamando um objeto.get_atributo();

### @atributo.setter
Usando esse decorator você faz o mesmo efeito do property mas para um setter, no lugarde atributo escreve o atibuto que você quer modificar, exemplo: limite ou saldo.

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
Métodos estáticos são métodos que podem ser chamados dentro de uma classe sem precisar ter instanciado algum objeto, geralmente é usado para atributos que serão de mesmo valor para todos os objetos de uma classe, exemplo:
```python
@staticmethod
def get_codigo_banco():
    return "001"
```
Em uma classe Conta, você pode chamar esse método sem ter instanciado alguma classe, e ele retornará o código do banco, pois será o mesmo para todas as contas que você criar, você nem precisa do self. Tome cuidado para não sair tacando staticmethod em todos os métodos da classe pois isso vai quebrar a orientação a objetos, saiba quando usar!

### Convenção dos Pythoners
Tornar um atributo ou método privado utilizando dois underscores é uma boa prática para o encapsulamento e segurança do app. Porém, existe uma prática mais popular que é de colocar apenas 1 underscore, como em self._nome por exemplo. Essa discussão começou pois ao colocar dois underscores, acaba de fato atrapalhando algumas funcionalidades, é um pouquinho burocrático para usar o atributo, vamos dizer. E como isso aqui é Python e não Java (rsrs), precisamos cortar o máximo de burocracia possível. Existe uma polêmica no ar quanto a essa questão, alguns ainda preferem e acham a melhor prática colocar dois unerlines mas a maioria dos Pythoners hoje colocam apenas um, isso não muda em nada na funcionalidade das paradas e é uma maneira de avisar à pessoa que está lendo o código que trata-se de algo privado, e por uma convenção entre os coders, ninguém mexe naquilo. É uma maneira de avisar que, mermão, se mexer pode quebrar a aplicação.

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

## Abstração
A abstração consiste em criar uma classe abstrata, com métodos abstratos, mas o que isso significa? Uma classe abstrata não deve ser instanciada, assim como seus métodos não devem ser chamados diretamente. A classe abstrata serve literalmente apenas para ser usada como modelo para as suas subclasses, que aí sim, podem ter objetos. Os métodos abstratos devem ser obrigatóriamente escritos nas subclasses.

Para criar uma classe abstrata, é necessário importar a lib abc, que é built-in. E dessa lib, você usará as classes ABC e staticmethod. Segue exemplo de uso:
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
Não se esqueça de fazer a classe abstrata herdar de ABC. Para criar um método estático é só usar o decorator @staticmethod e no processamento deixar só um pass, pois cada subclasse poderá usar esse método de um jeito um pouco diferente. o método estático DEVE estar nas subclasses mas como você viu, não precisa do decorator nelas também.

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

## Função Assíncrona
Uma função assíncrona, ou "async function", é uma função especial em Python que permite que o código seja executado de forma assíncrona. Isso significa que, em vez de bloquear a execução do programa até que uma operação seja concluída, a função assíncrona permite que o programa continue a ser executado enquanto a operação está em andamento.

A função assíncrona é definida usando a palavra-chave "async" antes da definição da função. Ela é executada em um objeto asyncio.EventLoop, que é responsável por gerenciar a execução assíncrona.

Dentro de uma função assíncrona, você pode usar a palavra-chave "await" antes de uma chamada a outra função assíncrona ou a uma operação de E/S (como uma chamada a uma API ou a um banco de dados), para indicar que você deseja esperar até que a operação seja concluída antes de continuar com a execução do código.

As funções assíncronas são muito úteis para lidar com operações de E/S demoradas ou bloqueantes, como requisições de rede ou acesso a banco de dados, sem bloquear a execução do programa. Elas permitem que o código seja executado de forma mais eficiente, melhorando o desempenho e a escalabilidade do programa.

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

## Annotated
No momento, nosso entendimento do uso do Annotated é: passar mais de uma característica para o parâmetro de uma função no FastAPI. Então por exemplo, se uma função recebe como parâmetro um token que deve ser uma string mas também trabalha com dependência do Depends, usamos:
```python
from typing import Annotated

def funcao(token: Annotated[str, Depends(dependency)]):
    pass
```
Até o momento, eu sei que isso não passa de uma boa prática e não usar pode causar erros, mas ainda precisamos pesquisar melhor o motivo de usar essa feature.

## Criptografia Assimétrica
```terminal
openssl req -new -x509 -nodes -newkey ec:<(openssl ecparam -name secp384r1)         -keyout private.key -out public.crt         -days 1095 -subj "/CN=teste_fanna"
```
Use esse comando para gerar chave privada e pública e trabalhar com chaves assimétricas.

## Definindo tipagem e usando o typing
No Python, você pode definir o tipo de dado que uma variável deve receber, por exemplo, vou criar uma variável nome_cliente e eu PRECISO que seja do tipo string, então:
```python
nome_cliente : str = "Fernando"
```
Isso irá mostrar que a variável deve receber uma string. Um outro exemplo interessante é definir os tipos de dados que os parâmetros de uma função deve receber:
```python
def somar(num1: int, num2: int):
    return num1 + num2
```
Aqui vemos uma simples função de soma mas eu especifiquei que esse método só pode ser chamado com 2 números inteiros, então não dá para chamar com uma string. Isso aumenta muito a qualidade e segurança do código, além de deixar o código mais legível para outros devs.

Agora uma outra coisa bem legal em definição de tipagem, é a famosa setinha ->. Para que ela serve? Bom, essa setinha é utilizada geralmente depois da definição de um método, para dizer que tipo de dado esse método deve retornar, exemplo:
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
Olha só que legal, assim podemos podemos criar uma função que retorna um objeto, wow. Isso é muito utilizado em frameworks de desenvolvimento web, para instanciar a aplicação em sí, chamando as rotas que podem estar em outro arquivo do projeto!

Agora, quando quiser usar essa funcionalidade em casos mais complexos como com lista, por exemplo, você deve importar a biblioteca typing, que possui features que ajudam com esses tipos mais complexos de dados. Por exemplo, ao instanciar uma lista, posso usar a classe List da lib typing e definir para a lista receber somente determinado tipo de dado:
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