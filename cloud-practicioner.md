## Tipos de Instância EC2:
* **Instância sob demanda** - padrão EC2, instância normal que vc paga conforme usa.
* **Instância Reservada** - Vc reserva uma instância sob demanda porém respeitando alguns critérios pre estabelecidos, com um "contrato" de no mínimo 1 ano, para ter um desconto no pagamento.
* **Instância spot** - Você pode usar infraestrutura que sobrou na nuvem, e não está sendo utilizada, assim vai pagar um valor mais em conta. Porém, é recomendada apenas para aplicações que não tem problema ficar fora do ar por alguns períodos. Não é recomendado em aplicações críticas.
* **Host dedicado** - Quase como um VPS, ao invés de ser uma instância virtual, é um servidor físico, dedicado apenas para você, com isolamento das outras instancias de outros clientes.

## Hipervisor no EC2
Um hipervisor é a camada de software (ou firmware) que cria e gerencia máquinas virtuais (VMs) sobre um servidor físico. Ele faz a “ponte” entre o hardware (CPU, memória, disco, rede) e os sistemas operacionais das VMs, permitindo que várias VMs rodem no mesmo host físico de forma isolada.

Quando você usa Amazon EC2, suas instâncias rodam como VMs em servidores da AWS.

A AWS é responsável por corrigir/atualizar o hipervisor (parte da segurança “da nuvem”, no modelo de responsabilidade compartilhada).

Você (cliente) normalmente é responsável por atualizar o sistema operacional dentro da instância (ex.: Linux/Windows), a aplicação, etc. (segurança “na nuvem”).

## Diferença entre Amazon Connect e Amazon Direct Connect
**Amazon Connect** = Central de atendimento

**Amazon Direct Connect** = Conexão privada entre on-premises e nuvem AWS

## Serviços de Armazenamento
* **Amazon EFS** - Sistema de arquivos compartilhados serverless
* **Amazon EBS** - Serviço de armazenamento em blocos para montar arquivos do OS e da aplicação de uma instância EC2

## Banco de Dados
**Amazon RDS (Relational Database Service)**: é um serviço gerenciado de banco de dados relacional. Ele suporta motores como MySQL, PostgreSQL, MariaDB, Oracle e SQL Server, facilitando a configuração, a operação e o escalonamento de bancos de dados tradicionais na nuvem. É usado para aplicações que precisam de um banco relacional com alta disponibilidade e manutenção simplificada.

**Amazon Redshift**: é um serviço de data warehouse totalmente gerenciado e otimizado para análise de grandes volumes de dados. Ele permite executar consultas analíticas complexas rapidamente, sendo ideal para BI, análise de dados e geração de relatórios em larga escala.

**Amazon DynamoDB**: é um banco de dados NoSQL gerenciado, projetado para oferecer alta performance e baixa latência em qualquer escala. É apropriado para aplicações que precisam de armazenamento rápido e flexível de dados, como aplicativos web, mobile e IoT.

**Amazon Neptune**: é um serviço de banco de dados de grafos gerenciado, projetado para armazenar e consultar eficientemente relacionamentos altamente conectados. Serve para casos como redes sociais, recomendações, grafos de conhecimento e detecção de fraudes.

**Amazon Aurora**: é um banco de dados relacional compatível com MySQL e PostgreSQL, desenvolvido para oferecer alta performance, resiliência e disponibilidade superior em relação aos bancos tradicionais. Serve para cargas de trabalho críticas que exigem escalabilidade, segurança e recuperação rápida.

**Amazon ElastiCache**: é um serviço gerenciado da AWS que facilita a implantação, operação e escalabilidade de caches na memória usando Redis ou Memcached. Ele é utilizado para acelerar aplicações web e de negócios, proporcionando acesso rápido a dados frequentemente consultados, reduzindo a latência e a carga em bancos de dados, pois armazena dados temporários em memória para respostas mais rápidas e alto desempenho.

## Sobre o Amazon VPC
* **Conexões de peering**: Permitem rotear o tráfego entre duas VPCs usando endereços IPv4 ou IPv6, não é possível usá-las para bloquear tráfegos específicos.
* **ACL de rede**: Atua como um firewall, você pode definir regras de entrada e saída para controlar quem pode ou não acessar, pode bloquear trafegos específicos.

## Tranferência de dados e arquivos
* **AWS Snowball Edge** - é um dispositivo de hardware usado para transporte de dados em grande escala. Você pode usar o Snowball Edge para transferir com segurança grandes quantidades de dados para a nuvem AWS em alta velocidade. Suporta terabytes de transferência de dados.
* **Amazon S3 Transfer Acceleration** - pode acelerar as transferências de e para o Amazon S3 para transferências de longa distância ou arquivos grandes, adequado para trabalhos recorrentes de transferência de dados, e não para uma migração única.

## Serviços Observadores
* **Truted Advisor** - Fornece CONSELHOS para seguir as melhores práticas de gerenciamento de nuvem AWS
* **CloudWatch** - é usado para monitorar recursos e aplicações. No entanto, o CloudWatch, por si só, não fornece um registro das atividades realizadas em uma conta.
* **CloudTrail** - fornece auditoria operacional e de risco, governança e conformidade de sua conta da AWS. Você pode usar o CloudTrail para identificar ações do usuário em serviços AWS como excluir uma instância EC2, por exemplo.
* **Amazon Inspector** - é um serviço de gerenciamento de vulnerabilidades que escaneia continuamente suas cargas de trabalho da AWS em busca de vulnerabilidades de software e exposição não intencional à rede.

## Diferenças entre Site-to-Site VPN, Direct Connect e Client VPN
* **AWS Direct Connect** - vincula sua rede interna a um local do Direct Connect por meio de um cabo de fibra óptica Ethernet padrão. Uma extremidade do cabo se conecta ao seu roteador. A outra extremidade do cabo se conecta a um roteador do Direct Connect. O Direct Connect fornece uma conexão a nível de rede com a AWS a partir de data centers on-premises, não de laptops.
* **AWS Site-to-Site VPN** - cria um caminho de rede criptografado entre sua rede on-premises e sua rede na nuvem AWS. Essa conexão usa a Internet. O Site-to-Site VPN fornece uma conexão a nível de rede com a AWS a partir de data centers on-premises, não de laptops.
* **AWS Client VPN** - é um serviço gerenciado de VPN baseado no cliente que permite acessar de forma segura seus recursos da AWS e os recursos na sua rede on-premises. Com o Client VPN, você pode acessar seus recursos de qualquer local usando um cliente de VPN com base no OpenVPN.

## Diferença entre AWS Direct Connect e AWS VPN
A AWS fornece o **AWS VPN** como um serviço gerenciado. O AWS VPN estabelece uma conexão criptografada segura entre a AWS e suas redes on-premises ou de filiais.

Você pode usar o **Direct Connect** para conectar de forma privada sua rede on-premises à nuvem AWS. No entanto, a comunicação via Direct Connect não é criptografada.

## Tipos de Gateway
* **Internet Gateway**: Você pode usar um gateway de internet para estabelecer conexões públicas de internet em um Amazon VPC.
* **NAT Gateway**: É possível usar gateway NAT para estabelecer uma conexão para instâncias em uma sub-rede privada dentro de um VPC. Os gateways NAT permitem que as instâncias se conectem a recursos ou serviços fora do VPC.
* **Virtual Private Gateway**: É possível usar um gateway privado virtual para estabelecer conexões com a nuvem AWS para sites on-premises por meio de túneis Site-to-Site VPN.
* **Gateway do AWS Direct Connect**: Um gateway do Direct Connect pode estabelecer uma conexão de sites on-premises à nuvem AWS. Um gateway Direct Connect usa links dedicados que são isolados da Internet.
* **Amazon Transit Gateway**: O Amazon VPC Transit Gateways é um hub de trânsito de rede usado para interconectar VPCs e redes on-premises.

## Finanças
**AWS Budgets**: O AWS Budgets é um serviço que permite criar orçamentos personalizados para monitorar e controlar custos e uso da AWS. Você pode definir limites, alertas, gerar relatórios para comparar gastos reais vs previstos, etc. O alerta pode apitar quando você estiver próximo de estourar seu orçamento.

**AWS Cost Explorer**: O AWS Cost Explorer é uma ferramenta de visualização e análise de custos e uso da AWS ao longo do tempo. É mais para puxar o histório, com direito a uma infinidade de filtros, para análises pós, com muita granularidade, com horários das atividades realizadas etc.

O **AWS Pricing Calculator** oferece a capacidade de criar estimativas para seus casos de uso da AWS antes de criar as aplicações. O AWS Pricing Calculator pode fornecer uma estimativa de custo.

## Serviços de IA
**Amazon Rekognition**: é um serviço da AWS que oferece análise de imagens e vídeos baseada em inteligência artificial. Ele permite identificar objetos, pessoas, textos, cenas e atividades, além de reconhecer rostos, detectar emoções e realizar moderação de conteúdo visual automaticamente, facilitando a integração de recursos avançados de visão computacional em aplicações sem necessidade de expertise em machine learning.

**Amazon Macie**: é um serviço da AWS voltado para segurança e proteção de dados, que utiliza machine learning para identificar, classificar e proteger dados confidenciais armazenados na AWS, especialmente em buckets do Amazon S3. Ele detecta automaticamente dados sensíveis, como informações pessoais, e monitora possíveis exposições ou acessos não autorizados, ajudando empresas a manter conformidade com regulamentações de privacidade e segurança.

## AWS Support Plans
**1. Basic (Gratuito)**
* ✅ Acesso 24/7 a atendimento ao cliente, documentação e whitepapers
* ✅ AWS Trusted Advisor (7 verificações básicas)
* ✅ AWS Personal Health Dashboard
❌ Sem suporte técnico

**2. Developer (~$29/mês ou 3% dos custos)**
* ✅ Tudo do Basic
* ✅ Suporte técnico em **horário comercial** via e-mail
* ✅ 1 contato primário
* ⏱️ Resposta: < 24h (geral) | < 12h (sistema comprometido)
* 🎯 Para ambientes de **desenvolvimento/teste**

**3. Business (~$100/mês ou 10-3% dos custos)**
* ✅ Tudo do Developer
* ✅ Suporte **24/7** via telefone, chat e e-mail
* ✅ Contatos ilimitados
* ✅ AWS Trusted Advisor (verificações completas)
* ✅ Orientação de arquitetura
* ⏱️ Resposta: < 1h (produção indisponível)
* 🎯 Para ambientes de **produção**

**4. Enterprise (~$15.000/mês ou 10-3% dos custos)**
* ✅ Tudo do Business
* ✅ **Technical Account Manager (TAM)** dedicado
* ✅ Revisões de arquitetura e operações
* ✅ Acesso ao Infrastructure Event Management
* ✅ Concierge Support Team
* ⏱️ Resposta: **< 15 min** (crítico para negócio)
* 🎯 Para **workloads críticos** e empresariais

## Outros Serviços/Explicações
**Amazon Quicksight** - business intelligence e vizualização de dados

**Athena** - usado para consultar informações no S3 usando SQL padrão

**AWS Shield** - Proteção contra DDos

É possível criar IAM roles e atribuir a instâncias EC2 para dar acesso à um bucket por exemplo

As **chaves de acesso do IAM** são credenciais de curto ou longo prazo. As chaves de acesso permitem que você acesse a AWS de forma programada.

Qual perspectiva do AWS Cloud Adoption Framework (AWS CAF) conecta tecnologia e negócios? *R - Pessoas*

Dentro do Well Architected Framework, Disaster Recovery está mais para confiabilidade do que para Segurança

**AWS Artifact** - Fornecimento de documentações para seguir diretrizes e politicas de segurança como ISOs

**AWS CloudHSM** - ajuda os usuários a atender aos requisitos de conformidade regulatória e contratual para segurança de dados usando dispositivos de hardware dedicados na Nuvem AWS

**AWS Outposts** - estende a infraestrutura da AWS até as instalações do cliente. Essa solução permite que você implante recursos em servidores físicos locais para atender aos requisitos de latência, conformidade e processamento local. O Outposts oferece a capacidade de usar o hardware instalado on-premises pela AWS para estender e executar serviços nativos da AWS on-premises. Se você usa o Outposts, pode executar alguns serviços da AWS localmente usando os mesmos serviços, ferramentas e APIs da AWS.

**Amazon Lightsail** - pode fornecer aos usuários da AWS uma solução simples de servidor virtual privado (VPS). O Lightsail pode fornecer aos desenvolvedores capacidades e recursos de computação, armazenamento e rede para implantar e gerenciar sites e aplicações web na nuvem.

**AWS Global Accelerator** - usa a rede global da AWS para rotear o tráfego para o endpoint regional ideal com base na integridade, na localização do cliente e em outras políticas personalizadas. O Global Accelerator ajuda a aumentar a disponibilidade e o desempenho das aplicações hospedadas na AWS.

**KMS - Key managment service**, usado para criar chaves criptográficas que controlam o uso em diversos serviços da AWS.

O **AWS Systems Manager** é usado para organizar, monitorar e automatizar tarefas de gerenciamento em recursos da AWS.

**Session Manager do AWS Systems Manager**: é um serviço de gerenciamento de nós para a nuvem ou para unidades computacionais on-premises. O Session Manager fornece uma conexão segura por meio de scripts de shell. Você pode estabelecer a conexão sem a necessidade de abrir portas. Pode ser usado para limitar o acesso ao EC2, com os usuários acessando instâncias remotamente em vez de abrir portas SSH de entrada e gerenciar chaves SSH.

**Certificate Manager** implanta certificados SSL/TLS para criptografar dados enquanto trafegam na rede.

Os **locais da borda** fazem parte da rede de entrega de conteúdo (CDN) da AWS. Eles são otimizados para entrega de conteúdo, não diretamente vinculados às Zonas de Disponibilidade. São pontos aleatorios em varios lugares do mundo que guardam em cache contedudo acessado com frequencia, diminuindo a latencia no cliente

A **AWS GovCloud (EUA)** é uma solução de nuvem criada para hospedar dados confidenciais e controlados, incluindo ativos críticos e de alto valor, atendendo requisitos de segurança. Ela oferece duas regiões isoladas (Leste e Oeste dos EUA), operadas e acessíveis apenas por cidadãos dos EUA, garantindo arquitetura segura, escalável e resiliente, com conectividade via Internet pública ou AWS Direct Connect.

O **AWS Elastic Beanstalk** é um serviço da AWS que facilita a implantação, gestão e escalabilidade de aplicações web na nuvem; ele permite que desenvolvedores façam o upload do código e o próprio serviço cuida automaticamente da infraestrutura necessária, incluindo provisionamento de servidores, balanceamento de carga, auto scaling e monitoramento, permitindo que você foque no desenvolvimento sem se preocupar com detalhes operacionais.

O **AWS Control Tower** é um serviço da AWS que facilita a criação, configuração e governança de ambientes multi-conta na nuvem. Ele ajuda empresas a estabelecer e administrar uma “aterrissagem” (landing zone) segura e bem estruturada, com boas práticas de segurança, conformidade, padronização e automação desde o início, permitindo o gerenciamento centralizado e simplificado de várias contas AWS de acordo com políticas e diretrizes corporativas.

O **AWS Config** é um serviço da AWS que permite monitorar, auditar e avaliar as configurações dos recursos na nuvem. Ele registra continuamente alterações e o histórico das configurações dos recursos da AWS, ajudando a identificar desvios, analisar conformidade com políticas, e atender requisitos de auditoria e segurança. Assim, facilita a gestão e o controle dos recursos, mantendo um rastreamento detalhado de como e quando cada configuração foi modificada.

**AWS Ground Station**: é usado para controlar as comunicações via satélite, processar dados de satélite e dimensionar as operações de satélite.