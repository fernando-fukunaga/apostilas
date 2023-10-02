# Apostila Docker
Aqui deixarei anotações importantes sobre meus estudos no Docker, para containerização de aplicações.

## Container
Um container, diferente de uma máquina virtual, roda em um sistema operacional como um *processo*, por esse motivos eles são mais leves que as VMs, e eles conseguem isolar as características e dependências do host através de namespaces que agem em várias camadas, a mais importante delas sendo a UTS, que é capaz de "pegar um pedacinho" do kernel do SO do host apenas com as partes necessárias para a aplicação e rodar dentro do container.
Afinal, uma aplicação não precisa necessariamente de um SO completo para rodar, o namespace da camada UTS vai pegar apenas o que é necessário diretamente do kernel do SO do host, que na maioria das vezes é o Linux Kernel. Por esse motivo que os containers são mais leves e é assim que ele provê isolamento entre ele mesmo, os outros containers rodando na mesma máquina e o SO da máquina em sí.

## Comandos básicos
* docker pull nome-da-imagem: comando para baixar uma imagem do DockerHub para minha máquina.
* docker run nome-da-imagem: comando para subir o container.
* docker ps: mostra os container rodando no momento, com a flag -a ele mostra todos os containers da minha máquina, inclusive os que já foram parados.
* docker stop nome-ou-id-do-container: parar container (reinicia todos os processos do container quando vc sobe de novo).
* docker start nome-ou-id-do-container: voltar a executar container que estava parado.
* docker pause nome-ou-id-do-container: pausar container (não reinicia todos os processos, quando você subir de novo o container os processos voltarão de onde estavam e também a contagem de há quanto tempo o container está de pé não é interrompida).
* docker unpause nome-ou-id-do-container: voltar a executar container que estava pausado.
* docker rm nome-ou-id-do-container: remover o container, isso vai fazer tudo ser excluído dentro dele, é uma ação irreversível! Por padrão, essa operação demora 10 segundos, por questões de segurança. Mas se você quiser algo mais rápido, adicione a flag --force que a operação será instantânea.

Algumas flags importantes no comando docker run:
* -d: essa flag é para o docker não travar seu terminal após subir um container e você não precisar abrir outro terminal.
* -p portadocontainer:portadamaquina: essa flag é para mapear uma porta do container (que está isolada e inacessível) para uma porta do nosso host, da nossa máquina, por exemplo 80:8080, vc está direcionando a porta 80 do container para a porta 8080 da nossa localhost.
* --name nome-do-container: definir um nome personalizado pro container, se você não definir isso, por padrão o docker coloca um nome "fofinho" como funny_beaver ou coisa do tipo.