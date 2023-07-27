# Apostila de Desenvolvimento Web com Python
Essa apostila irá concentrar anotações mais focadas na parte backend e web. Conceitos, libs, boas práticas, padrões, etc. Usando a linguagem Python

## Super definição resumida do que é REST
Em pouquíssimas palavras, dar um valor semântico às requisições, de acordo com o path e o método HTTP.

## A diferença entre WSGI e ASGI

## Path Parameters e Query Parameters
Quando precisar escolher entre path parameters ou query parameters na chamada de um endpoint, pense sempre que os path parameters são destinados a organização e modularização, por exemplo para acessar partes específicas da sua estrutura, quando o dado que quer filtrar é um dado estático, sem atributos a serem qualificados, como um id por exemplo, ou sub páginas num site.

Já os query parameters, são mais usados com a intenção de qualificar os parâmetros, quando esses parâmetros tem atributos com valores editáveis, por exemplo, para explicar o número máximo de resultados que você quer que o endpoint retorne, para fazer uma busca igual uma busca do google, onde você tem várias maneiras de escrever o que quer achar, e coisas desse tipo.

## Annotated
No momento, nosso entendimento do uso do Annotated é: passar mais de uma característica para o parâmetro de uma função no FastAPI. Então por exemplo, se uma função recebe como parâmetro um token que deve ser uma string mas também trabalha com dependência do Depends, usamos:
```python
from typing import Annotated

def funcao(token: Annotated[str, Depends(dependency)]):
    pass
```
Até o momento, eu sei que isso não passa de uma boa prática e não usar pode causar erros, mas ainda precisamos pesquisar melhor o motivo de usar essa feature.

## CORS (Cross-Origin Resource Sharing)
Se você quiser integrar um front na sua aplicação, ele pode estar rodando fora do seu servidor back, por exemplo, os dois podem estar no localhost mas em portas diferentes, e quando isso acontece, o FastAPI irá barrar qualquer requisição que venha de, digamos, "fontes desconhecidas". Para isso, devemos configurar o CORS no FastAPI para especificar de quais origens nossa aplicação backend pode receber requisições.

O CORS é um conceito do javascript, quando um front em JS precisa se comunicar com seu serviço REST, é importante que o seu backend tenha configurado o CORS, ele define quais origens podem acessar o seu serviço, quais headers serão permitidos, quais métodos HTTP serão permitidos e coisas do tipo.

## BaseModel e pydantic
Os modelos para um projeto FastAPI podem ser divididos em dois tipos. Temos os modelos do negócio, que seriam os modelos que refletem as entidades do nosso modelo conceitual do banco de dados mesmo, nossas tabelas, são as entidades que compões o negócio por trás da nossa aplicação.

Mas temos também os modelos de serviço, ou podemos até chamá-los de controladores, são modelos que usaremos para receber ou cuspir informações, realizar tarefas ou tranferências de dados dentro do nosso backend e coisas do gênero.

## Arquitetura model/schema
A arquitetura que aprendi e achei muito legal para transporte de dados é a de models e schemas. Basicamente, o cliente faz requisições e recebe respostas das rotas usando schemas, o que seria tipo o JSON. E os repositórios mandam e buscam dados ao banco de dados usando os models, que são representações das tabelas do próprio banco relacional, usando da orienteção a objetos.

Os nossos repositórios têm a função de pegar os schemas das requests e responses e convertê-las em models, para que se faça a persistência e as consultas no banco. E também fazer o inverso, tranformando models que voltam do banco em shcmeas para cuspir JSONs de response ao cliente. Irei implantar essa arquitetura em meus projetos pessoais.

Dica: pense em um repositório como o DAO dos seus projetos Java with Maven :D

## Conexões com o banco
Em toda rota que for precisar se conectar com o banco de dados, você deve passar como parâmetro uma sessão do banco, pois ela é precisa para se instanciar a classe de repositorio que faz a conversa com o banco. Exemplo:
```python
# Importando os módulos necessários
from fastapi import FastAPI, Depends
from sqlalchemy.orm import Session
from src.infra.sqlalchemy.config.database import criar_banco_de_dados, get_banco_de_dados
from src.schemas.schemas import Usuario
from src.infra.sqlalchemy.repositorios.usuario import RepositorioUsuario

criar_banco_de_dados()
app = FastAPI()

@app.post("/cadastro")
# Definindo a rota, passando como um dos parâmetros o banco de dados que é so tipo Session e depende do get_banco que obtem o banco em sí
def cadastar(usuario: Usuario, banco_de_dados: Session = Depends(get_banco_de_dados)):
    # Instanciando um objeto da classe do repositório responsável, para poder chamar a função de criar um usuário, essa classe tem a sessão do banco no construtor, por isso precisamos dela, então devemos importar e usar como parâmetro da rota sempre.
    usuario_cadastrado = RepositorioUsuario(banco_de_dados).criar(usuario)
    return usuario_cadastrado
```

## Config de schemas
Nos schemas da aplicação, sempre que criar um BaseModel, crie uma classe Config dentro desse BaseModel para permitir que um model seja convertido em schema da maneira correta, melhorando a comunicação entre esses dois elementos:
```python
class Usuario(BaseModel):
    id: Optional[int] = None
    nome: str
    email: str
    username: str
    senha: str

    # Essa config aqui:
    class Config:
        orm_mode = True
```

## SQL Migrations com Alembic
A lib Alembic tem uma função muito legal trabalhando juntamente ao SQLAlchemy. Ele trata das SQL Migrations, mas o que é isso? Pense que o Alembic será como um "git" do seu banco de dados, ou seja, ele vai ajudar você a fazer alterações na estrutura do banco, como adicionar tabelas e colunas, alterar o nome de alguma coluna e coisas desse tipo, e deixará todas essas alterações versionadas dentro do seu projeto.

É como se você pudesse ver toda a trajetória do seu banco, ele fica maleável e mutável, isso lembra um pouco o git não? Afinal, cada versão do seu banco seria como se fosse um commit diferente. Então quando você quer adicionar uma nova tabela ou coluna, por exemplo, ao invés de fazer aquele trampo de dropar o banco de dados e criá-lo novamente, perdendo assim os registros já existentes. Você pode apenas pedir para o Alembic detectar as mudanças que você fez no seu arquivo models.py do sqlalchemy por exemplo e aplicar essas mudanças. O Alembic vai atualizar seu banco, sem excluir registros que já haviam lá antes. Mas é claro que se por exemplo você mudar o data type de alguma coluna, você pode anular o valor dela em todos os registros, isso é normal. E pelo SQLite eu não consigo alterar coisas como constraints das colunas, acho até que faz sentido mas ainda podemos confirmar se em outros SGDB's acontece a mesma coisa.

Para usar o Alembic, você deve instalar o mesmo no seu ambiente usando o pip, e inicializar ele com o comando:
```terminal
alembic init alembic
```

Depois, configure o arquivo alembic > env.py, você deve importar o seu Base e os seus models, e no mesmo arquivo, atribuir à variável target_metadata, a função metadata do seu Base:
```python
# Fazendo as importações
from src.infra.sqlalchemy.config.database import Base
from src.infra.sqlalchemy.models.models import *

# Configura metadata
target_metadata = Base.metadata
```

Depois disso, procuro o arquivo alembic.ini na raíz do projeto, abra o arquivo e em sqlalchemy.url coloque a url do seu banco de dados, sem aspas nenhuma.

Pronto, você já pode usar o alembic, digite o comando abaixo no terminal, na raíz do projeto para pedir para o alembic detectar mudanças na config do seu banco e criar uma versão, entre aspas, digite uma breve mensagem explicando o que você alterou, tipo uma mensagem de commit, considere esse comando como se fosse um git add . para facilitar o entendimento:
```terminal
alembic revision --autogenerate -m "alterando coluna x da tabela y"
```

Ao executar esse comando, você perceberá que no caminho alembic > versions estão sendo criados arquivos .py com as suas versões e alterações. A própria lib te aconselha a fazer alterações nesse arquivo pois, como a geração do mesmo é automática, ele pode não ter "pego" tudo que você alterou exatamente igual, já percebi que ele não pega tamanho máximo de strings por exemplo, então faça alterações no arquivo sempre que necessário e se achar que está tudo certo, execute o comando abaixo para atualizar de fato o banco, considere esse comando como se fosse um git commit para facilitar o entendimento:
```terminal
alembic upgrade head
```

Pronto, você usou o alembic para atualizar seu banco sem precisar excluir ele a criar de novo. Lembre-se de sempre instalar o alembic e inicializar ele antes mesmo de rodar o projeto pela primeira vez, pois assim, você terá um histórico completo do ciclo de vida do seu banco de dados desde a sua criação.

E lembre-se também que ter muita certeza das constraints de suas colunas na primeira criação, pois achei burocrático alterar isso depois, vejo o alembic como uma ferramenta mais para adicionar tabelas e colunas que permitem null, alterar constraints já é um pouco complicado.

Uma coisa que vi que dá para fazer é se caso queira fazer alterações em chaves estrangeiras, é só colocar além da coluna chave desejada dentro do ForeignKey(), inserir também um name, isso permitirá "forçar" a adição de uma nova coluna de chave estrangeira, permitindo ela ter valor nulo nos registros já existentes, mas acho que isso não soa correto, pelo menos tente atualizar os registros existentes para que eles deixem de ter valor nulo nessa chave. O problema é se você já tem 500 registros lol, claro que você poderia automatizar um processo aí mas seria complicado pois você está alterando todo o negócio na aplicação que pode já estar em "produção". Essa é a importância de estruturar muito bem o banco de dados e os requisitos funcionais do projeto antes de iniciar o desenvolvimento.

De qualquer forma, a definição de name para ForeignKey ficará algo como:
```python
produto_id - Column(Integer, ForeignKey('produto.id', name='fk_pedido_produto'))
```
Fazendo isso, você pode pedir para o alembic alterar o seu banco adicionando chaves estrangeiras que ficarão com valor nulo nos registros já existentes.

## Background Tasks
Às vezes em nossa aplicação, vamos precisar realizar uma tarefa que pode se tornar demorada e afetar no desempenho da nossa aplicação. E quando isso acontece, podemos usar background tasks para já dar um retorno ao cliente e deixar essas tarefas para serem realizadas a parte, haja visto que são coisas que, se derem erro, podem ser comunicadas depois.

Isso geralmente é usado para tarefas como enviar um e-mail ou processar um pagamento, por exemplo. Repare que quando fazemos uma compra em um e-commerce, ao mandar os dados do nosso cartão de crédito e finalizar a compra já recebemos a confirmação da compra imediatamente. Por que isso acontece? Pois o site opta por já te dar uma confirmação enquanto executa por meio de uma background task a sua função de processar pagamento, já que esta pode demorar um pouco para ser finalizada.

A grande motivação por trás disso, na minha visão, está relacionada a UX, pois se para processos demorados como esse a gente obrigasse o cliente a aguardar todo o processamento antes de dar um retorno, daria a impressão que o site é lento, isso não é bom. Ficaria naquele loading eterno do navegador, usuário nenhum gosta disso.

## Middlewares
Middlewares são processos que você pode colocar no meio do fluxo entre a requisição feita pelo cliente e a rota no servidor. Você pode pedir para que sempre antes do processamento de uma rota X, seja executada uma função qualquer. Então você libera a rota para ser executada, e quando ela tiver a response, você pode, ainda, fazer mais alguma outra coisa (tipo uma chamada de função mesmo) antes de liberar a response para viajar até o cliente.

É como se fosse um pedágio das requests e responses antes de chegar no servidor ou depois de sair dele, você pode usar o middleware para o que for necessário dentro da sua regra de negócio.

## Dicas e boas práticas
* É uma boa prática colocar os nomes das rotas com palavras no plural, exemplo: "/buscar-contratos", isso faz parte da definição de REST, uma API RESTful tem que seguir esses tipos de padrão.