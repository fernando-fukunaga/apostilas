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

## CORS
Se você quiser integrar um front na sua aplicação, ele pode estar rodando fora do seu servidor back, por exemplo, os dois podem estar no localhost mas em portas diferentes, e quando isso acontece, o FastAPI irá barrar qualquer requisição que venha de, digamos, "fontes desconhecidas". Para isso, devemos configurar o CORS no FastAPI para especificar de quais origens nossa aplicação backend pode receber requisições.

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

## Dicas e boas práticas
* É uma boa prática colocar os nomes das rotas com palavras no plural, exemplo: "/buscar-contratos", isso faz parte da definição de REST, uma API RESTful tem que seguir esses tipos de padrão.