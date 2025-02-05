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

Protected - Qualquer classe do mesmo pacote e qualquer subclasse independente do pacote pode ver e manipular.

## Atalhos do IntelliJ Idea
* Ctrl + Alt + O - Importar todas as classes/pacotes que não foram importados e remover imports não utilizados na classe atual.

* Ctrl + / - Comentar um bloco de código selecionado.

* Shift + F6 - Renomear de forma inteligente classes, métodos e atributos de forma a renomear em todos os lugares onde são usados, inclusive nome de arquivos.

## Interfaces como Adjetivos
Você pode utilizar interfaces para definir que diferentes classes de diferentes "árvores genealógicas de herança" SEJAM alguma coisa. Por exemplo uma classe Produto com uma subclasse Margarina e uma outra classe Servico com uma subclasse ConsertoDePia. Tanto a margarina que é um produto quanto o conserto de uma pia que é um serviço são coisas tributáveis, ou seja, é cobrado imposto sobre isso. Você pode criar uma interface Tributavel com o método calculoDeImposto definido, então qualquer classe seja ela filha de Servico ou Produto pode implementar essa interface Tributavel e aí cada uma vai ter uma maneira diferente de fazer o cálculo dos impostos com o método calculoDeImposto. Ou seja, se o produto for tributável, então ele implementa a interface Tributavel, isso é um exemplo de polimorfismo.

## Lombok
Geração de boilerplate(códigos repetitivos que você vai ter que escrever toda hora). Exemplo, numa classe Usuario com os atributos nome, senha e idade, ao invés de escrever todos os getters setters e o construtor, posso usar annotations para que o lombok injete isso para mim sem eu precisar escrever:
```java
import lombok.AllArgsConstructor
import lombok.Getter
import lombok.Setter

@Getter
@Setter
@AllArgsConstructor
public class User {
    private String name;
    private int age;
    private String password;
}
```

## Persistência de dados com JPA
JPA é uma especificação de ORM para o Java, ele não é uma implementação de ORM nem uma lib nem nada do tipo. Apenas define um conjunto de regras e práticas para persistência no estilo ORM mesmo. Para utilizar JPA você precisa de uma **implementação**, existe a implementação presente na API do Jakarta EE (antigo Java EE) ou você pode usar o Hibernate, um framework de ORM para Java, e provavelmente a implementação mais famosa. Outras implementações conhecidas são EclipseLink e OpenJPA.

No JPA temos algo chamado EntityManager, é a principal interface da JPA para gerenciar entidades e interagir com o banco de dados. Ele é responsável por persistir, buscar, atualizar, remover dados no banco. O EntityManager gerencia o ciclo de vida das entidades e mantém um cache de primeiro nível (as entidades recuperadas dentro da mesma transação são reutilizadas sem precisar de uma nova consulta ao banco).

Para usar o EntityManager em uma classe de DAO, por exemplo, vc deve criar um EntityManagerFactory, necessário para criar o EntityManager, iniciar uma transação, chamar um método de EntityManager como persist() para inserir um objeto no banco, e completar a transação utilizando commit() depois fechando o EntityManager. Confira um exemplo:
```java
import jakarta.persistence.*;

public class ClienteDAO {
    private EntityManagerFactory emf = Persistence.createEntityManagerFactory("meuPU");

    public void salvarCliente(Cliente cliente) {
        EntityManager em = emf.createEntityManager();
        em.getTransaction().begin();  // Inicia uma transação

        em.persist(cliente);  // Insere o cliente no banco

        em.getTransaction().commit(); // Confirma a transação
        em.close(); // Fecha o EntityManager
    }
}
```

### Hibernate
* **O que é?** O Hibernate é a implementação mais popular da JPA. Ele é um framework ORM (Object-Relational Mapping) que gerencia a persistência de objetos Java no banco de dados.
* Diferencial: Ele fornece recursos extras além da JPA, como cache de segundo nível, suporte a queries HQL, otimizações de performance, etc.
* Trabalha com SQL e HQL: Ele pode converter consultas SQL nativas ou permitir o uso de HQL (Hibernate Query Language), que é mais orientada a objetos.

Exemplo de código Hibernate puro (sem usar Spring):
```java
SessionFactory sessionFactory = new Configuration().configure().buildSessionFactory();
Session session = sessionFactory.openSession();
Transaction tx = session.beginTransaction();

Cliente cliente = new Cliente();
cliente.setNome("João");
session.save(cliente);

tx.commit();
session.close();
```

Perceba que, ao contrário do JPA, o Hibernate exige mais configurações e gerenciamento manual da sessão.

### Spring Data JPA
Spring Data JPA é um módulo do Spring Boot que simplifica o uso da JPA e do Hibernate. Ele elimina a necessidade de escrever consultas SQL ou HQL na maioria dos casos e permite criar repositórios com apenas interfaces.

Vantagens:
* Menos código boilerplate (não precisa gerenciar EntityManager manualmente)
* Criação automática de queries com findBy + nome do atributo.
* Facilidade para paginação e ordenação.

Exemplo de repositório com Spring Data JPA:
```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ClienteRepository extends JpaRepository<Cliente, Long> {
    List<Cliente> findByNome(String nome);
}
```
Aqui, JpaRepository já fornece métodos prontos como save(), findAll(), findById(), e até consultas personalizadas com findByNome(String nome) sem precisar escrever SQL.

* Mas eu só consigo usar Spring Data JPA com o Hibernate?
Não! Por padrão, quando você instala o Spring Data JPA no seu projeto, ele vem com o Hibernate, mas você pode utilizar outra implementação do JPA conforme sua preferência, basta configurar isso no application.properties ou application.yml corretamente!