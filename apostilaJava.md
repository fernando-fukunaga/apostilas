## Operador ternário
Quando quiser usar um if e else rapidamente na mesma linha para verficar uma condição, use ? e :.

Por exemplo, estamos desenvolvendo um tamaguchi, e ele está dormindo, ele vai ganhando energia de 1 ponto a cada repetição do laço mas o máximo de energia é 10 e ele deve acordar quando chegar no 10:
```java
energia = energia + 1 > 10 ? 10 : energia + 1
```
Ou seja, na repetição atual, existe uma condição, se for verdadeira, usa o que estiver depois do ? e se for false, o programa usa o que estiver depois do :. Então ele verifica se a energia + 1 ponto é maior que 10, se for maior, energia recebe 10, se ainda não for mais, é adicionado mais um ponto ao valor de energia. E então o laço é quebrado quando energia == 10.

## Alta coesão
Cada classe faz apenas uma função, para poder reutilizar código, uma classe de Teste pode ser criada para aí sim fazer testes realmente.

## Classe public e private
Uma classe ode ter várias sub-classes porém apenas uma classe por arquivo pode ser public.

## Sobrecarga e Sobrescrita
São duas formas de implementar o polimorfismo, a sobrecarga é quando você faz um outro comportamento para um método mudando os atributos, a sobrescrita ou override, é quando os atributos se mantêm iguais.

## Métodos static e non-static
Um método estático é um método da classe, que não necessita de nenhuma intância de objeto para ser chamado. Já o método não estático, só pode ser chamado quando se tem instâncias daquele objeto, como por exemplo os getter e setters, não dá para manipular atributos de objeto se nenhum objeto foi instanciado ainda.
