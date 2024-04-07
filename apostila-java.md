## Operador ternário
Quando quiser usar um if e else rapidamente na mesma linha para verficar uma condição, use ? e :.

Por exemplo, estamos desenvolvendo um tamaguchi, e ele está dormindo, ele vai ganhando energia de 1 ponto a cada repetição do laço mas o máximo de energia é 10 e ele deve acordar quando chegar no 10:
```java
energia = energia + 1 > 10 ? 10 : energia + 1
```
Ou seja, na repetição atual, existe uma condição, se for verdadeira, usa o que estiver depois do ? e se for false, o programa usa o que estiver depois do :. Então ele verifica se a energia + 1 ponto é maior que 10, se for maior, energia recebe 10, se ainda não for mais, é adicionado mais um ponto ao valor de energia. E então o laço é quebrado quando energia == 10.

## Operador de pré-incremento vs pós-incremento
Existem dois tipos de operadores de incremento: o operador de pré-incremento (++variavel) e o operador de pós-incremento (variavel++).

O operador de pré-incremento (++variavel) aumenta o valor da variável em 1 antes de usar a variável em uma expressão. Aqui está um exemplo:
```java
int num = 5;
int resultado = ++num; //num é incrementado para 6 e depois atribuído a resultado
System.out.println(num); // imprime 6
System.out.println(resultado); // imprime 6
```
Já o operador de pós-incremento (variavel++) aumenta o valor da variável em 1 depois de usar a variável em uma expressão. Aqui está um exemplo:
```java
int num = 5;
int resultado = num++; //num é atribuído primeiramente à variável resultado e depois incrementado para 6
System.out.println(num); // imprime 6
System.out.println(resultado); // imprime 5

```

## Alta coesão
Cada classe faz apenas uma função, para poder reutilizar código, uma classe de Teste pode ser criada para aí sim fazer testes realmente.

## Classe public e private
Uma classe pode ter várias sub-classes porém apenas uma classe por arquivo pode ser public.

## Sobrecarga e Sobrescrita
São duas formas de implementar o polimorfismo, a sobrecarga é quando você faz um outro comportamento para um método mudando os atributos, a sobrescrita ou override, é quando os atributos se mantêm iguais.

## Métodos static e non-static
Um método estático é um método da classe, que não necessita de nenhuma intância de objeto para ser chamado. Já o método não estático, só pode ser chamado quando se tem instâncias daquele objeto, como por exemplo os getter e setters, não dá para manipular atributos de objeto se nenhum objeto foi instanciado ainda.

## Annotation Override
Quando um método Java possui a annotation @Override em cima, significa que você está implementando um método de uma classe mãe ou interface, e você quer garantir que a classe filha está implementando todos os métodos que deve implementar. Por exemplo, se na ClasseMae você tem um método imprime() e na ClasseFilha também, caso voce altere o nome em ClasseMae para imprimir(), caso o método da classe filha não tenha o @Override, o Java vai entender que são métodos diferentes e não vai reclamar, agora caso na ClasseFilha haja o @Override, o Java vai perceber que nenhuma classe mãe de ClasseFilha possui um método chamado imprime() e irá lançar a exceção para você.

## Default Methods
Antes, uma interface do Java só podia ter métodos abstratos, ou seja, métodos sem implementação, que funcionavam apenas como uma base para que as classes filhas fizessem a sua própria implementação. Porém, a partir do Java 8, se você escrever "default" antes de definir um método, você pode implementá-lo ali mesmo, caso você tenha certeza que ele irá ter o mesmo comportamento sempre. Isso permite que você adicione um método em uma interface antiga do seu programa sem quebrar todas as implementações dessa interface por existir a obrigatoriedade desse método ser implementado em todas as implementações.

## Classe Abstrata vs Interface
Uma classe abstrata pode ser atributos declarados e estado, uma interface não, apenas métodos, comportamentos concretos, não pode ter estado.

## Modificadores de acesso
Private - Só a própria classe pode ver e manipular.

Public - Qualquer classe pode ver ou manipular.

Default (sem definir se é public ou private) - Qualquer classe do mesmo **pacote** pode ver ou manipular, se quiser acessar um atributo default de uma classe de outro pacote, por exemplo, tem que ir lá que colocar um public se não, não irá conseguir puxar a informação.

Protected - Em breve.