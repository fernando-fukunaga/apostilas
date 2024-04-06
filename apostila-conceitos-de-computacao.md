## Estrutura de Dados
Em computação, normalmente utilizamos os dados de forma conjunta. A forma como estes dados serão agregados e organizados depende muito de como serão utilizados e processados, levando-se em consideração, por exemplo, a eficiência para buscas, o volume dos dados trabalhados, a complexidade da implementação e a forma como os dados se relacionam. Estas diversas formas de organização são as chamadas estruturas de dados. 

Podemos afirmar que um programa é composto de algoritmos e estruturas de dados, que juntos fazem com que o programa funcione como deve.

Cada estrutura de dados tem um conjunto de métodos próprios para realizar operações como:
* Inserir ou excluir elementos;
* Buscar e localizar elementos;
* Ordenar (classificar) elementos de acordo com alguma ordem especificada.

As estruturas de dados podem ser:
* lineares (ex. arrays) ou não lineares (ex. grafos);
* homogêneas (todos os dados que compõe a estrutura são do mesmo tipo) ou heterogêneas (podem conter dados de vários tipos);
* estáticas (têm tamanho/capacidade de memória fixa) ou dinâmicas (podem expandir).

Basicamente estruturas de dados são as famosas variáveis que guardam vários valores diferentes, por exemplo arrays. No caso do Python, listas, tuplas e dict são estruturas de dados.

## Linguagem estaticamente tipada vs dinamicamente tipada
Uma linguagem de tipagem estática ou estaticamente tipada, faz a verificação de todos os tipos de dados das variáveis antes da execução do programa, então toda declaração de variável deve indicar obrigatoriamente o seu tipo, além de não ser permitido trocar o tipo de uma variável em tempo de execução. Exemplos de linguagens assim são: C, C++ e Java.

Já uma linguagem de tipagem dinâmica, ou dinamicamente tipada, não exige que você diga o tipo de variável ao declará-la, ele irá entender em tempo de execução qual tipo do dado que foi atribuído à variável, até por isso você pode alterar o tipo de uma variável várias vezes em tempo de execução:
```python
variavel = 1 # essa variável será do tipo int

variavel = "python" # agora a mesma variável mudou para o tipo str, e isso não gerará nenhum erro
```
Exemplos de linguagem assim são: Python, JavaScript e Ruby.

## Tipagem forte vs tipagem fraca
Em uma linguagem de tipagem forte, você não pode fazer operações com tipos de dados diferentes, o compilador ou interpretador não irá entender isso, por exemplo, o seguinte código em Python, que é uma linguagem de tipagem forte, irá dar um erro:
```python
variavel_1 = 5
variavel_2 = "python"

print(variavel_1 + variavel_2)
# a saída no console será: 
# TypeError: cannot concatenate ‘str’ and ‘int’ objects
```
Se eu tentar fazer a mesma coisa no JavaScript, que é uma linguagem de tipagem fraca, ele entenderá e fará a concatenação, mesmo se tratando de duas variáveis com tipos diferentes:
```javascript
const variavel1 = 5;
const variavel2 = "javascript";

console.log(variavel1 + variavel2);
// a saída no console será:
// 5javascript
```