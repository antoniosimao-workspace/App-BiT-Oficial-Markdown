# Recomendações Estratégicas para a Equipe
Como se trata de um MVP com potencial para apresentação em um Shark Tank, o foco deve estar na integração rápida e em demonstrar que o núcleo da solução (Inteligência Artificial e utilização de dados) realmente funciona.
## UX/UI — O coração da aplicação é a empatia
Pessoas pertencentes a grupos sub-representados enfrentam baixa autoestima e exclusão social.
Por isso, a interface não pode ser fria nem parecer apenas um formulário de cadastro.
O processo de onboarding deve ser amigável, acolhedor e intuitivo, enquanto o check-in diário de saúde mental, baseado em emojis, deve transmitir acolhimento e proximidade.
## Engenharia de Software / Backend — Contrato de APIs Imediato
Existem dois endpoints obrigatórios e bem definidos:
- `/orientar`
- `/saude`
A equipe de Backend deve disponibilizar esses endpoints inicialmente com dados simulados (*mockados*) para que a equipe de Frontend possa realizar a integração imediatamente, sem precisar esperar a implementação completa da lógica da IA.
## Dados & IA (FastAPI / Python) — Engenharia de Prompt Isolada
O agente de Inteligência Artificial deve ser testado em um ambiente controlado (como um script Python ou um Jupyter Notebook) antes de ser integrado ao backend.
A lógica do **"Gap dos 30%"** e o encaminhamento ao **CVV** (quando a nota for inferior a 4) são regras rígidas (*hardcoded*) que devem envolver o modelo de IA, garantindo que não ocorram alucinações ou respostas inadequadas em momentos de crise.
## Frontend — Abordagem Mobile-First (PWA)
Como a aplicação será utilizada em regiões com conectividade variável (conforme o dataset da Anatel), o design deve ser otimizado para celulares de baixo custo e telas responsivas.
# Divisão das Seções do Projeto (Estrutura Base)
Estrutura sugerida para o documento principal do projeto, dividida conforme as responsabilidades da equipe.
# 1. Definição do Produto e Abordagem Humana (Ecossistema 360°)
- Declaração do problema e sua relevância cultural, apresentando um diagnóstico das barreiras enfrentadas pelo público-alvo (com base nas referências do desafio).
- Proposta de valor do BiT, mostrando como as cinco dimensões (**Formação, Empregabilidade, Experiências, Mentorias e Saúde Mental**) trabalham de forma integrada para superar o assistencialismo tradicional.
# 2. Design da Experiência do Usuário (UX/UI)
## Onboarding empático
Fluxo de coleta de dados pessoais, profissionais e de geolocalização sem gerar atrito ou frustração.
## Dashboard e interface dos serviços
Desenvolvimento da tela inicial adaptativa e visualização do **Gap Percentual** das vagas disponíveis.
### Módulo de Bem-Estar Humano
Desenvolvimento do check-in diário utilizando emojis e da apresentação de sugestões relacionadas à saúde mental.
# 3. Arquitetura de Software e Modelo de Dados (Engenharia de Software)
## Arquitetura da PWA (Progressive Web App)
Estratégia para suportar ambientes com baixa conectividade, utilizando **Service Workers** e armazenamento local para disponibilizar recursos offline em áreas críticas.
## Modelo de Dados Entidade-Relacionamento
Estrutura de banco de dados para armazenar:
- Usuários;
- Vagas de emprego;
- Cursos;
- Histórico de humor (saúde mental);
- Registros (logs) de geolocalização.
## Integração com o Dataset Vísent CDRView
Lógica responsável por cruzar as coordenadas das antenas da Anatel (latitude e longitude) com a área de cobertura do usuário, permitindo priorizar conteúdos offline ou recomendar eventos próximos.
# 4. Desenvolvimento do Backend e Endpoints do MVP (Backend)
## Endpoint `/orientar`
Implementação da lógica de negócio responsável por calcular a diferença entre os requisitos das vagas e o perfil do usuário (**Regra 70/30**), retornando uma trilha de aprendizagem recomendada.
## Endpoint `/saude`
Processamento do estado emocional do usuário.
### Regra Crítica de Negócio
Ativar automaticamente o indicador:
```text
derivar_cvv = true
```
quando:
```text
nota_semanal < 4
```
## Segurança e Variáveis de Ambiente
Definição do protocolo para impedir que chaves de API e credenciais sejam enviadas para o repositório do GitHub, utilizando arquivos `.env` para armazenamento seguro.
# 5. Integração do Agente de Inteligência Artificial
*(Engenheiro de IA / Backend)*
## Engenharia de Prompt para Saúde Mental
Definição das diretrizes para que o agente responda seguindo a filosofia dos Alcoólicos Anônimos, baseada em:
- Escutar sem julgar;
- Oferecer sugestões de ações conforme o contexto regional do usuário.
## Pipeline de Recomendação de Cursos
Cruzamento automático do perfil do usuário com programas como:
- GEAR (Google);
- ONE (Oracle/Alura);
utilizando Inteligência Artificial para recomendar as formações mais adequadas.
# 6. Implantação e Plano de Lançamento
*(DevOps / QA)*
## Estratégia de Deploy
Configuração do ambiente de produção utilizando plataformas como:
- Railway;
- Render.
## Matriz de Testes de Sensibilidade
Plano completo de testes para o endpoint `/saude`, simulando cenários de crise para garantir que o encaminhamento ao CVV nunca falhe.
## Estrutura do README
# Guia para Execução Local do Projeto e Contratos de API
Guia contendo instruções para execução local do projeto e documentação dos contratos de **Request/Response**, visando facilitar a apresentação do MVP para investidores.
# Seção 1: Definição do Produto e Abordagem Humana (Ecossistema 360°)
O núcleo do **App BiT** é compreender que a falta de emprego entre comunidades sub-representadas não é um problema isolado; ela é consequência de um ciclo acumulativo de exclusão.
Se uma pessoa está com fome, possui baixa autoestima ou vive em um bairro com baixa conectividade, dificilmente conseguirá manter a concentração em um curso de programação de quatro horas.
Esta seção define como o BiT rompe esse ciclo vicioso, utilizando uma abordagem holística que atua simultaneamente sobre diferentes dimensões da vida do usuário.
## 1.1 Diagnóstico das Barreiras e Dores do Usuário
Para desenvolver uma solução baseada na empatia, a equipe deve compreender profundamente as seis principais dificuldades identificadas no MVP.
A documentação do produto deve abordar cada uma delas, apresentando uma resposta clara em termos de design e desenvolvimento.
| Dor do Usuário | Manifestação Real | Resposta da App BiT |
|---------------|-------------------|---------------------|
| **Baixa autoestima e sentimento de inferioridade** | O usuário não se candidata às vagas porque acredita que não possui o perfil adequado. | **Gap dos 30%:** desmistifica a ideia de elegibilidade. A plataforma comunica ao usuário: *"Você já possui 70% das competências necessárias. Você já é altamente valioso; falta apenas uma pequena parte para alcançar a vaga."* |
| **Ciclo de exclusão** | A falta de recursos prejudica a saúde mental, dificultando os estudos e perpetuando a dificuldade de conseguir emprego. | **Ecossistema 360°:** atua simultaneamente nas cinco dimensões da plataforma. Se o check-in de saúde mental indicar baixa pontuação, o aplicativo reduz o ritmo das notificações relacionadas aos estudos. |
| **Desvantagens socioeconômicas** | Falta de internet estável, aparelhos celulares simples e ambientes inadequados para estudar. | **Integração com o Vísent CDRView:** o sistema detecta baixa qualidade do sinal e oferece automaticamente conteúdos disponíveis offline para que o usuário possa continuar estudando sem consumir dados móveis. |
### Continuidade
O usuário pode continuar estudando sem consumir dados móveis.
| Dor do Usuário | Manifestação Real | Resposta da App BiT |
|---------------|-------------------|---------------------|
| **Isolamento no mercado de trabalho** | Não possui familiares ou amigos que trabalhem na área de tecnologia, resultando em uma rede de contatos extremamente limitada. | **Mentorias Humanizadas:** substitui a entrevista tradicional por um convite mais próximo e acolhedor, como: *"Você gostaria de fazer uma prática comigo?"* |
| **Falta de sentimento de pertencimento** | Acredita que o mercado de tecnologia é destinado a pessoas com uma realidade diferente da sua. | **Experiências Transformadoras:** vídeos e depoimentos de CEOs, líderes e profissionais que vieram dos mesmos contextos sociais e regionais do usuário, demonstrando que o sucesso é possível. |
# 1.2 O Ecossistema 360°: Integração Funcional das Cinco Dimensões
Um erro comum durante o desenvolvimento seria implementar o App BiT como cinco módulos independentes.
O verdadeiro diferencial da plataforma está na integração automática entre todos os seus serviços.
O fluxo de funcionamento é o seguinte:
```text
[Check-in de Saúde Mental]
           │
           ▼
Influência o ritmo das Formações
           │
Cruza informações com
           ▼
[Geolocalização / Vísent CDRView]
           │
           ▼
Filtra oportunidades de Empregabilidade
           │
Cria conexões com
           ▼
[Mentorias e Experiências]
```
## Saúde Mental como Porta de Entrada
O check-in diário utilizando emojis não é apenas um recurso complementar.
Ele altera diretamente o comportamento do aplicativo.
Se um usuário registrar o estado **"Sobrecarregado"** durante três dias consecutivos, o agente de IA passará a priorizar conteúdos de **Experiências Transformadoras**.
Continuação da seção anterior:
O agente de IA passará a priorizar conteúdos de **Experiências Transformadoras** relacionados à resiliência e reduzirá a pressão sobre metas de empregabilidade e formação.
## A Regra do 70/30 (Empregabilidade + Formação)
O mecanismo de correspondência (*match*) do backend compara as competências do usuário com os requisitos das vagas disponíveis.
Em vez de simplesmente rejeitar o candidato, o sistema utiliza os **30% de competências ausentes** como entrada dinâmica para o módulo de **Formações**, criando automaticamente uma trilha personalizada de aprendizagem utilizando programas da Google Cloud, Oracle ou Alura.
Os módulos de Formação geram uma trilha de aprendizagem personalizada, utilizando programas da:
- Google Cloud;
- Oracle;
- Alura.
O objetivo é suprir exatamente as competências que faltam ao usuário.
## Mentoria como Rede de Apoio
Quando o usuário encontra dificuldades para superar os 30% restantes de sua formação, o sistema ativa automaticamente o módulo de **Mentorias**, conectando-o a um profissional que possa atuar como facilitador do aprendizado e reduzir a sensação de que:
> "Sempre falta alguma coisa para conseguir uma oportunidade."
# 1.3 Filosofia de Design: Da Assistência à Autonomia
É fundamental deixar claro aos investidores que o **BiT** não é uma plataforma assistencialista.
O assistencialismo gera dependência.
O objetivo do BiT é promover:
- Protagonismo;
- Autonomia;
- Emancipação do usuário.
## Princípio da Identidade
Inspirado no filme:
- *O Menino que Descobriu o Vento* (*The Boy Who Harnessed the Wind*)
o aplicativo parte do princípio de que o talento e a determinação já existem dentro da comunidade.
A plataforma apenas oferece:
- Infraestrutura;
- Conexões;
- Oportunidades;
que as barreiras sistêmicas historicamente impediram essas pessoas de acessar.
## Filosofia do Agente de Inteligência Artificial
O tom utilizado pelo agente de Inteligência Artificial (implementado pela equipe de Backend através de prompts específicos) deve seguir a filosofia dos **Alcoólicos Anônimos**:
> "Escutar sem julgar."
Continuação da seção anterior:
O agente não deve dar ordens nem fazer sermões.
Seu papel é validar o estado emocional do usuário, utilizando mensagens como:
- "Está tudo bem sentir-se cansado hoje."
- "Você não precisa resolver tudo de uma vez."
Além disso, deve propor microações simples, viáveis e adaptadas ao contexto regional, aproveitando as informações de conectividade e geolocalização fornecidas pelo dataset.
# Seção 2: Pesquisa e Design UX/UI
O objetivo da equipe de design é transformar barreiras psicológicas e socioeconômicas em uma interface intuitiva, acessível e extremamente acolhedora.
Será adotada uma abordagem **Mobile First (PWA)**, otimizada para funcionar em celulares com diferentes tamanhos de tela e conexões de internet instáveis.
## 2.1 Arquétipos de Usuário (Personas) com Abordagem Humana
Para projetar uma experiência baseada na empatia, a equipe deve mapear arquétipos de usuários considerando não apenas o que eles fazem, mas principalmente como se sentem.
### Persona: Thiago, 24 anos
#### Dores
- Ansiedade;
- Sensação constante de que "falta aprender tudo".
#### Objetivo do UX
Gamificação positiva e visualização clara do **Gap de Competências**, incentivando o progresso.
### Persona: Aline, 31 anos
#### Dores
- Falta de tempo;
- Baixa conectividade com a internet.
#### Objetivo do UX
Disponibilização de conteúdos offline e estratégias de **microlearning**, permitindo estudar em pequenos períodos ao longo do dia.
## O Graduado Bloqueado
Jovem de um bairro periférico que concluiu seus cursos, mas não consegue ser aprovado nas entrevistas de emprego.
Sofre intensamente com a síndrome do impostor e praticamente não possui rede de contatos profissionais.
### Foco do UX
A aplicação deve destacar suas competências e conquistas reais, fazendo com que o módulo de **Mentorias** seja percebido como uma conversa acolhedora entre colegas, e não como uma avaliação ou exame.
## O Trabalhador em Transição de Carreira
Pessoa que trabalha longas jornadas no setor informal e estuda durante o trajeto no transporte público ou à noite.
Possui pacote limitado de dados móveis e utiliza um celular de baixo desempenho.
### Foco do UX
- Interface extremamente leve;
- Botões grandes para facilitar o uso durante deslocamentos;
- Modo escuro nativo para reduzir o cansaço visual;
- Alertas claros indicando conteúdos disponíveis para download offline.
# 2.2 Fluxos Críticos do Usuário (User Flows)
A equipe de design deve mapear três fluxos fundamentais para o MVP, sempre reduzindo a carga cognitiva do usuário e evitando excesso de perguntas.
## Fluxo A: Onboarding e Diagnóstico Inicial (Zero Frustração)
O processo tradicional de cadastro, que exige currículo logo no início, prejudica a experiência de quem acredita não ter experiência suficiente.
Por isso, o fluxo proposto é:
### 1. Boas-vindas Inspiradoras
Tela inicial com uma estética inspirada em filmes como:
- *Rainha de Katwe*;
- *O Menino que Descobriu o Vento*.
Utilizando frases que despertem:
- Potencial;
- Esperança;
- Autonomia.
### 2. Dados Básicos Simplificados
Coleta apenas das informações essenciais:
### Dados Básicos Simplificados
Coleta apenas das informações essenciais:
- Nome;
- E-mail;
- Gênero;
- Localização.
Tudo apresentado de forma limpa e intuitiva.
### 3. Pesquisa Adaptativa de Objetivos
Em vez de perguntar diretamente sobre experiência profissional, o aplicativo pergunta:
> "O que você gostaria de conquistar neste momento?"
#### Opções
- Estudar;
- Definir minha carreira;
- Procurar emprego;
- Mudar de área profissional.
### 4. Localização e Conectividade (Vísent CDRView)
O sistema solicita permissão de localização de maneira transparente:
> "Utilizamos sua localização para mostrar eventos próximos e otimizar o funcionamento do aplicativo caso sua conexão de internet seja limitada."
## Fluxo B: Check-in de Saúde Mental e Ações
Esse fluxo acontece sempre que o usuário abre o aplicativo pela primeira vez no dia.
### Fluxo Simplificado
```text
Tela Inicial:
"Como você está hoje?"
          │
          ▼
 Seleção do Emoji
          │
          ▼
 Pontuação Semanal
          │
    ┌─────┴─────┐
    │           │
    ▼           ▼
 Nota < 4    Nota ≥ 4
    │           │
    ▼           ▼
```
## Fluxo B: Check-in de Saúde Mental e Ações
```text
Botão CVV em Destaque        Sugestão Humanizada
         +                   Feed Personalizado
```
Esse fluxo garante que usuários em situação de vulnerabilidade recebam prioridade no acolhimento, enquanto os demais recebem recomendações personalizadas para promover seu bem-estar e desenvolvimento.
### 1. Tela Inicial
Ao abrir o aplicativo pela primeira vez no dia, é exibida uma janela amigável e impossível de ignorar com a pergunta:
> "Como você está se sentindo hoje, [Nome]?"
O objetivo é criar um momento de acolhimento antes de qualquer outra interação com a plataforma.
### 2. Carrossel de Emojis
A interface apresenta ícones grandes, expressivos e culturalmente diversos representando os principais estados emocionais:
- Feliz
- Cansado
- Triste
- Ansioso
- Sobrecarregado
O usuário seleciona o emoji que melhor representa como está se sentindo naquele momento.
### 3. Avaliação Silenciosa
Internamente, o aplicativo calcula uma pontuação baseada no humor informado.
#### Caso a pontuação seja crítica (nota inferior a 4)
A interface muda discretamente para tons mais suaves e tranquilos.
É exibida uma mensagem extremamente empática juntamente com um botão de ação rápida, visível porém delicado, permitindo contato imediato com o **CVV (Centro de Valorização da Vida)**.
#### Caso a pontuação seja estável (nota igual ou superior a 4)
O agente de Inteligência Artificial apresenta uma sugestão personalizada de bem-estar, por exemplo:
- "Hoje parece um ótimo dia para caminhar descalço sobre a grama."
- "Que tal ouvir um pequeno podcast produzido na sua região?"
## Fluxo C: Visualização do "Gap 70/30" nas Vagas
O objetivo é substituir a tradicional mensagem negativa **"Você não atende aos requisitos"** por uma abordagem construtiva e motivadora.
### 1. Cartão da Vaga
Cada vaga apresenta:
- Nome do cargo;
- Um gráfico circular de progresso, onde aproximadamente **70%** aparece destacado em verde ou azul suave, indicando as competências que o usuário já possui.
### 2. Mensagem Motivacional
O aplicativo exibe uma mensagem como:
> "Você está muito perto! Já atende a 70% do que esta empresa procura. Veja tudo o que você já conquistou."
Logo abaixo, é apresentada uma lista das competências já dominadas pelo usuário, identificadas com um ícone de confirmação (✔).
### 3. A Ponte para os 30% Restantes
As competências que ainda faltam aparecem representadas por um cadeado aberto, acompanhado da mensagem:
> "Falta apenas isto, e nós vamos resolver isso juntos."
Na sequência, é exibido um botão de ação direta, por exemplo:
```text
[Iniciar Curso Rápido na Google Cloud / Alura]
```
permitindo que o usuário comece imediatamente uma formação voltada para preencher essa lacuna.
## 2.3 Sistema de Design do BiT: Estética de Pertencimento
A equipe de UX/UI deve criar um guia de identidade visual que evite a aparência corporativa tradicional inspirada no "Vale do Silício", caracterizada por tons frios de azul e tipografias rígidas.
Em seu lugar, deve adotar uma identidade visual vibrante, humana e representativa da América Latina e da África, transmitindo proximidade, diversidade e sentimento de pertencimento.
## Paleta de Cores (Inspirada nas Referências Culturais)
### Cores de Acolhimento e Natureza (Base)
Utilizar tons terrosos, amarelos quentes (inspirados nas paisagens do filme *O Menino que Descobriu o Vento*) e verdes suaves, transmitindo sensações de:
- Crescimento;
- Esperança;
- Bem-estar emocional.
### Cor de Progresso (Ações)
Empregar um tom de laranja vibrante ou violeta profundo, simbolizando:
- Protagonismo;
- Inovação;
- Tecnologia.
### Acessibilidade (WCAG 2.1)
É obrigatório manter alto contraste entre textos e fundos.
Muitos usuários utilizarão o aplicativo sob luz solar intensa ou em pontos de transporte público; por isso, a interface deve permanecer perfeitamente legível sem exigir brilho máximo da tela, reduzindo também o consumo de bateria.
## Tipografia e Iconografia
### Tipografia
Utilizar fontes arredondadas, modernas e altamente legíveis, como:
- Plus Jakarta Sans;
- Inter.
Essas fontes reduzem a rigidez visual e tornam a leitura mais agradável.
### Iconografia Realista e Diversificada
Os avatares e ícones utilizados nas mentorias devem representar pessoas reais pertencentes a grupos sub-representados, rompendo com os tradicionais vetores genéricos utilizados em plataformas corporativas.
## Componentes Obrigatórios da Interface para o MVP
### Widget de Estado Emocional
A tela inicial deve conter um grande seletor horizontal de emojis para que o usuário informe facilmente seu estado emocional diário.
### Barra Dinâmica de Progresso ("Gap Meter")
### Barra Dinâmica de Progresso ("Gap Meter")
Componente visual responsável por mostrar, de forma intuitiva, como cada curso concluído aproxima o usuário do fechamento dos **30% restantes** necessários para alcançar determinada oportunidade de emprego.
# Seção 3: Arquitetura de Software e Banco de Dados
O projeto arquitetônico do **BiT** é guiado pelo princípio da **resiliência tecnológica**.
Como o público-alvo enfrenta limitações socioeconômicas, o sistema deve consumir poucos dados móveis e continuar funcionando, mesmo que de forma limitada, em cenários offline (sem conexão com a internet).
## 3.1 Arquitetura da PWA (Progressive Web App) e Estratégia Offline
Para atender ao requisito de uma Aplicação Web Responsiva (**PWA**) que funcione como um aplicativo nativo em dispositivos móveis, será adotada uma arquitetura baseada em:
- Service Workers;
- IndexedDB;
- Cache Storage.
## Mecanismos de Resiliência para Redes Instáveis
### Estratégia de Cache "Stale-While-Revalidate"
Para os elementos da interface (estilos, ícones, fontes e telas da aplicação), o Service Worker entregará imediatamente os arquivos armazenados em cache local enquanto verifica atualizações em segundo plano.
Essa estratégia garante que o aplicativo carregue em menos de **2 segundos**, mesmo em conexões **3G**.
### Sincronização em Segundo Plano (Background Sync)
Se o usuário realizar seu check-in diário de saúde mental ou tentar se inscrever em uma mentoria enquanto estiver sem cobertura de internet, a PWA armazenará automaticamente a requisição HTTP em uma fila no **IndexedDB**.
Assim que o Service Worker detectar que o dispositivo recuperou a conexão (utilizando a API de conectividade do navegador), os dados acumulados serão enviados automaticamente ao backend, sem necessidade de qualquer ação adicional por parte do usuário.
### Módulo de Recursos Offline
Quando o sistema detectar que o usuário está entrando em uma região com cobertura crítica de internet (com base no dataset da Anatel), a interface disponibilizará um botão:
```text
Baixar trilha para a memória local
```
Essa funcionalidade permitirá armazenar no dispositivo:
- Conteúdos textuais dos cursos (GEAR/Alura);
- Micro podcasts de bem-estar;
- Informações de contato para emergências.
Tudo em formato leve, permitindo acesso mesmo sem conexão com a internet.
## 3.2 Integração do Dataset Vísent CDRView
O dataset, construído com coordenadas reais da infraestrutura de telecomunicações do Brasil (Anatel), será utilizado como o mecanismo inteligente de geolocalização do aplicativo.
## Fluxo de Dados e Utilização do Dataset
### 1. Captura das Coordenadas
Durante o uso da aplicação, o Frontend envia a latitude (`lat`) e longitude (`lng`) do usuário através dos principais endpoints:
- `/orientar`
- `/saude`
### 2. Processamento Geoespacial no Backend (FastAPI / Node.js)
O backend mantém armazenado o dataset das antenas de telecomunicações (**ERBs – Estações Rádio Base**).
Ao receber as coordenadas do usuário, realiza um cálculo de proximidade (utilizando a fórmula de **Haversine** ou consultas espaciais através de extensões como **PostGIS**) para determinar:
- A distância entre o usuário e as antenas mais próximas;
- A tecnologia predominante disponível na região (5G, 4G ou 3G).
### 3. Regras de Negócio Derivadas
#### Priorização de Conteúdo
Caso a área onde o usuário esteja localizada apresente predominância de **3G** ou baixa densidade de antenas (indicando sinal fraco), a API retornará automaticamente o indicador:
```json
{
  "baixa_conectividade": true
}
```
Esse indicador fará com que o Frontend ative automaticamente o modo de economia extrema de dados, adotando medidas como:
- Desativar a reprodução automática de vídeos das Experiências Transformadoras;
- Priorizar transcrições em texto;
- Recomendar downloads de conteúdos para acesso offline.
#### Filtro de Eventos Próximos
Ao cruzar a localização do usuário com a base de dados de eventos presenciais e mentorias práticas, o sistema retornará apenas oportunidades localizadas dentro de um raio de deslocamento acessível ao usuário.
Essa estratégia busca reduzir o isolamento geográfico e aumentar as possibilidades reais de participação.
## 3.3 Modelo de Dados Entidade-Relacionamento (Banco de Dados)
Para o MVP, recomenda-se um modelo de banco de dados relacional (**PostgreSQL**) ou híbrido, garantindo a integridade das transações críticas, como:
- Histórico de saúde mental;
- Correspondência automática entre usuários e vagas de emprego.
As principais entidades e seus relacionamentos são apresentados a seguir:
```text
[USUÁRIOS] 1 ─────────── N [HISTÓRICO_SAÚDE]
```
> A continuidade desse diagrama é apresentada na página seguinte.
```text
[USUÁRIOS] 1 ─────────── N [HISTÓRICO_SAÚDE]
      │
      ├────────────── N [CANDIDATURAS_VAGAS] ─── N [VAGAS]
      │
      └────────────── N [INSCRIÇÕES_CURSOS] ─── N [CURSOS]
```
### 1. Tabela: Usuários
Armazena o perfil 360° coletado durante o processo de onboarding.
#### Campos
- `id` (UUID, Chave Primária)
- `nome`
- `email`
- `data_nascimento`
- `gênero`
- `escolaridade` (VARCHAR)
- `continente`
- `país`
- `estado`
- `cidade` (VARCHAR)
- `whatsapp` (VARCHAR)
- `nivel_tecnico` (ENUM: Trainee, Júnior, Pleno)
- `area_interesse` (VARCHAR: Frontend, Backend, QA, etc.)
- `objetivo_atual` (ENUM: Estudar, Definir Carreira, Procurar Emprego, Mudar de Emprego)
- `ultima_latitude`
- `ultima_longitude` (DECIMAL)
### 2. Tabela: Histórico_Saúde
Responsável por monitorar diariamente o estado emocional do usuário.
Essa tabela é fundamental para calcular a nota semanal e disparar alertas de crise quando necessário.
#### Campos
- `id` (UUID, Chave Primária)
- `usuario_id` (UUID, Chave Estrangeira → Usuarios.id)
- `data` (Timestamp, utilizando por padrão a data e hora atuais)
- `humor` (VARCHAR / ENUM: feliz, cansado, triste, ansioso, sobrecarregado)
- `nota_diaria` (Inteiro, escala de 1 a 5)
- `contexto_regional` (TEXT, armazenando um resumo das condições do ambiente ou da rede no momento do registro)
### 3. Tabela: Vagas e Tabela Intermediária Candidaturas_Vagas
Contém a base de oportunidades profissionais utilizadas pelo sistema para calcular automaticamente a compatibilidade entre usuário e vaga.
#### Campos
- `id` (UUID, Chave Primária)
- `empresa`
- `cargo`
- `descricao` (TEXT)
#### Campos da Tabela de Vagas
- `requisitos_tecnicos` (JSONB / Array contendo as habilidades exigidas para a vaga)
- `latitude` (DECIMAL)
- `longitude` (DECIMAL)
Utilizadas para calcular a proximidade entre a vaga e o usuário.
### Tabela Intermediária
### Tabela Intermediária: Candidaturas_Vagas
Essa tabela registra o percentual de compatibilidade (`gap_percentual`) calculado pelo backend para cada usuário em relação a cada vaga disponível.
### 4. Tabela: Cursos
Catálogo contendo todas as formações disponíveis na plataforma, incluindo programas como:
- Google Cloud GEAR;
- Oracle ONE;
- Alura.
#### Campos
- `id` (UUID, Chave Primária)
- `provedor` (VARCHAR: Google, Oracle, Alura)
- `titulo` (TEXT)
- `url_acesso` (TEXT)
- `habilidades_que_cobre` (JSONB / Array contendo as tecnologias ensinadas pelo curso)
Esse campo é essencial para que a Inteligência Artificial consiga relacionar automaticamente os **30% de competências faltantes** do usuário com o curso mais adequado.
- `suporta_offline` (BOOLEAN)
Indica se o conteúdo pode ser utilizado sem conexão com a internet.
## 3.4 Stack Tecnológico Recomendado para a Equipe
Considerando que a equipe possui Engenheiros de Software e desenvolvedores Full Stack, a seguinte combinação tecnológica maximiza a velocidade de desenvolvimento do MVP:
### Frontend
- React
- Tailwind CSS (para construção rápida de interfaces responsivas)
- Vite (para configuração nativa e eficiente da PWA através de plugins de Service Workers)
### Backend
- FastAPI (Python)
**ou**
- Node.js com Express
#### Recomendação
Caso o processamento do dataset **Vísent CDRView** e os algoritmos de recomendação baseados em IA sejam implementados em Python, o **FastAPI** é a opção mais indicada, devido ao suporte nativo à programação assíncrona e à geração automática da documentação dos endpoints (**Swagger**).
### Banco de Dados
#### PostgreSQL
Seu suporte ao tipo de dado **JSONB** permite armazenar listas de habilidades tanto do perfil do usuário quanto das vagas de forma flexível, sem perder a robustez do modelo relacional necessário para controlar informações críticas, como o histórico de saúde mental.
Com essa arquitetura devidamente documentada, o Engenheiro de Software pode iniciar imediatamente a criação do banco de dados e a configuração do repositório no GitHub, incluindo as estratégias de cache, enquanto a equipe de Backend prepara a estrutura de dados que alimentará toda a aplicação.
# Seção 4: Desenvolvimento do Backend e da API
Esta seção estabelece as regras de funcionamento do servidor e o comportamento dos endpoints obrigatórios do MVP.
Para garantir a segurança do sistema, toda a lógica crítica deve ser executada no lado do servidor, expondo contratos de integração claros e padronizados para consumo pelo Frontend.
## 4.1 Endpoint `/orientar` (Algoritmo e Lógica do 70/30)
Este endpoint é responsável por calcular a lacuna de competências entre o perfil do usuário e as exigências do mercado de trabalho.
Não se trata apenas de uma consulta simples, mas de uma regra ativa de negócio.
### Método
```http
POST
```
### Rota
```http
/api/v1/orientar
```
## Estratégia do Algoritmo (Backend)
1. O backend recebe as habilidades atuais e os interesses do usuário (perfil e nível).
2. Realiza uma consulta na tabela de **Vagas**, filtrando pela área de interesse e pela região geográfica, utilizando um raio baseado nas coordenadas latitude (`lat`) e longitude (`lng`).
3. Para cada vaga compatível, calcula o percentual de correspondência entre as habilidades do usuário e as competências exigidas, utilizando a interseção entre os conjuntos de habilidades.
### Continuação da Estratégia do Algoritmo (Backend)
4. **Regra Crítica de Negócio**
O sistema procura vagas cujo nível de compatibilidade esteja próximo de **70%**.
Quando o usuário atende aproximadamente **70% dos requisitos**, os **30% restantes** são identificados explicitamente e retornados no campo `gap_items`.
5. Em seguida, o backend consulta a tabela **Cursos** (programas GEAR da Google e ONE da Oracle/Alura), procurando módulos capazes de suprir exatamente as competências faltantes (`gap_items`) e monta automaticamente a trilha de aprendizagem sugerida (`trajetoria_sugerida`).
## Contrato de Integração
### JSON — REQUEST
```json
{
  "usuario_id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
  "perfil": ["HTML", "CSS", "JavaScript", "Git"],
  "nivel": "Junior",
  "regiao": "SP",
  "idioma": "PT",
  "lat": -23.550520,
  "lng": -46.633308
}
```
### JSON — RESPONSE
```json
{
  "gap_percentual": 70.0,
  "gap_items": [
    "React",
    "Tailwind CSS"
  ],
  "trajetoria_sugerida": [
    {
      "curso_id": "c102",
      "provedor": "Oracle & Alura (ONE)"
    }
  ],
  "vagas_compativeis": [
    {
      "vaga_id": "v99",
      "empresa": "Tech Solutions BR",
      "cargo": "Desenvolvedor Frontend Júnior"
    }
  ],
  "confianca": 0.95
}
```
### Exemplo de Curso Recomendado
```json
{
  "titulo": "Formação Frontend: Imersão em React",
  "url": "https://alura.com.br/one/react"
}
```
A resposta também retorna as vagas compatíveis com o perfil do usuário:
```json
"vagas_compativeis": [
  {
    "vaga_id": "v99",
    "empresa": "Tech Solutions BR",
    "cargo": "Desenvolvedor Frontend Júnior"
  }
],
"confianca": 0.95
```
# 4.2 Endpoint `/saude`
## Monitoramento de Crises e Regras Rígidas
A área de saúde mental exige uma abordagem híbrida composta por:
### Camada Controladora Determinística
Código tradicional responsável por aplicar regras rígidas e inquebráveis.
### Camada Generativa
Inteligência Artificial responsável por gerar mensagens empáticas e sugestões de bem-estar.
> A IA nunca decide sozinha se um usuário deve ser encaminhado para atendimento em situação de crise.
Essa decisão é tomada exclusivamente pelo código do backend, com base na pontuação registrada.
## Método
```http
POST
```
## Rota
```http
/api/v1/saude
```
## Lógica de Controle de Crise
### 1. Recebimento dos Dados
O backend recebe o emoji correspondente ao humor do usuário e calcula o impacto sobre a nota semanal, consultando os registros dos últimos sete dias armazenados no banco de dados.
### 2. Filtro de Segurança (Hardcoded)
Se a nota semanal ou a nota do dia atual for inferior a **4** (em uma escala de 1 a 5, onde valores abaixo de 4 representam situação de alta vulnerabilidade emocional ou crise), o backend ativa imediatamente:
```text
derivar_cvv = true
alerta = true
```
### 3. Encaminhamento ao CVV
Quando o encaminhamento é ativado, o sistema interrompe o fluxo normal de sugestões de lazer e bem-estar.
Em vez disso, adiciona automaticamente ao JSON de resposta os dados de contato direto do **CVV (Centro de Valorização da Vida)**.
## Contrato de Integração
### JSON — REQUEST
```json
{
  "usuario_id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
  "humor": "sobrecarregado"
}
```
```json
{
  "usuario_id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
  "humor": "sobrecarregado",
  "nota_semanal": 3.5,
  "contexto": "Baixo sinal de rede na região, fim do mês"
}
```
## JSON — RESPONSE (Situação de Crise Detectada)
```json
{
  "mensagem": "Olá. Percebemos que as coisas têm sido muito difíceis ultimamente e queremos dizer que você não está sozinho. O seu bem-estar é a nossa maior prioridade. Existem pessoas preparadas para ouvi-lo com total respeito e confidencialidade neste momento.",
  "acao_sugerida": "Converse gratuitamente e de forma anônima com um atendente do CVV clicando no botão abaixo. Você não precisa enfrentar isso sozinho.",
  "derivar_cvv": true,
  "nota_atual": 3.5,
  "alerta": true,
  "contato_emergencia": {
    "organizacao": "Centro de Valorização da Vida (CVV)",
    "telefone": "188",
    "site": "https://www.cvv.org.br/"
  }
}
```
# Seção 5: Integração do Agente de Inteligência Artificial
O Agente de IA será implementado utilizando um **Modelo de Linguagem de Grande Escala (LLM)**, como um modelo fundacional do Google ou equivalente, consumido por meio de API e orquestrado pelo Backend através de técnicas rigorosas de **Prompt Engineering**.
O agente deverá atuar inspirado na filosofia dos Alcoólicos Anônimos:
- Ouvir sem julgar;
- Validar as emoções do usuário;
- Propor pequenas ações humanas e acessíveis para promover o bem-estar.
# 5.1 Prompt do Sistema para o Agente de Saúde Mental (Módulo Empático)
Este é o prompt de sistema que a equipe de Backend deverá enviar permanentemente para o modelo de IA, definindo sua personalidade e seus limites de atuação.
## CONTEXTO DO SISTEMA
Você atua como um acompanhante humano, empático e culturalmente relevante para comunidades sub-representadas no setor de tecnologia.
Sua filosofia é baseada em:
- Escutar sem julgar;
- Validar as emoções do usuário;
- Promover autonomia;
- Incentivar o sentimento de pertencimento.
Você não é um psicólogo clínico.
Seu papel é atuar como um facilitador de bem-estar, oferecendo acolhimento e orientação inicial de maneira humana e respeitosa.
## REGRAS DE TOM E ESTILO
### 1. Seja verdadeiramente humano, acolhedor e próximo
Utilize uma linguagem compreensiva, empática e acessível, evitando termos médicos complexos ou um tom corporativo frio.
O usuário deve sentir que está conversando com alguém que realmente o compreende.
### 2. Nunca julgue ou minimize o sofrimento do usuário
Jamais critique, diminua ou desconsidere a dor, a frustração ou o cansaço da pessoa.
Em vez disso, valide seus sentimentos utilizando expressões como:
- "É completamente compreensível sentir-se assim."
- "Está tudo bem precisar de uma pausa."
- "Você não precisa enfrentar tudo sozinho."
O acolhimento deve sempre preceder qualquer sugestão.
### 3. As recomendações devem ser simples, humanas e possíveis
As sugestões oferecidas pela IA devem ser ações concretas, leves e facilmente realizáveis no cotidiano, priorizando atividades fora das telas sempre que possível.
O objetivo é incentivar hábitos saudáveis de lazer e bem-estar, e não apenas indicar conteúdos digitais.
## REGRA DE CONTEXTO REGIONAL E CONECTIVIDADE
As recomendações devem sempre considerar o contexto social e tecnológico do usuário.
Se os metadados indicarem baixa conectividade ou desvantagens socioeconômicas, o agente não deve sugerir atividades que dependam de internet rápida ou gastos financeiros, como assistir séries em streaming 4K ou comprar produtos.
Em vez disso, deve recomendar alternativas gratuitas e acessíveis, por exemplo:
- Caminhar descalço na grama;
- Ouvir um áudio previamente salvo no celular;
- Escrever pensamentos em um caderno ou folha de papel;
- Observar a chuva ou a natureza;
- Ler um livro físico;
- Conversar com alguém de confiança.
## LIMITES ABSOLUTOS DE SEGURANÇA
Caso o usuário demonstre pensamentos de automutilação, desesperança extrema ou sinais claros de crise emocional grave, o agente deve responder de forma breve, extremamente acolhedora e respeitosa.
Deve reforçar que a vida do usuário possui valor inestimável, evitando oferecer sugestões comuns do cotidiano.
Nesse cenário, o backend será responsável por anexar automaticamente as informações oficiais de apoio (**CVV**).
A mensagem produzida pela IA deve apenas preparar emocionalmente o usuário para aceitar essa ajuda com confiança e segurança.
# 5.2 Fluxo de Execução do Agente no Código (FastAPI / Node.js)
## Fluxo de Execução do Agente no Código (FastAPI / Node.js)
Para facilitar a implementação pelo Engenheiro de Software no endpoint `/saude`, o fluxo lógico deve seguir uma arquitetura baseada em três camadas de proteção:
```text
[Requisição /saude]
           │
           ▼
1. Verificação da Nota Crítica (< 4)
           │
     ┌─────┴─────┐
     │           │
    SIM         NÃO
     │           │
     ▼           ▼
Responder      Enviar para IA
com Template   (Prompt + Humor)
CVV
                 │
                 ▼
        Sanitizar Resposta
                 │
                 ▼
        Enviar ao Frontend
```
## Camada 1: Avaliação Pré-IA (Filtro Determinístico)
O backend analisa os dados recebidos no JSON.
Se:
```text
nota_semanal < 4
```
o sistema pode optar por **não chamar a API da Inteligência Artificial**, economizando tempo de processamento e custos computacionais.
Nesse caso, responde imediatamente utilizando uma mensagem de crise previamente aprovada por profissionais da área de saúde, garantindo:
- Rapidez;
- Segurança;
- Consistência no atendimento.
## Camada 2: Consulta Controlada à Inteligência Artificial
Se o estado emocional do usuário estiver baixo, porém ainda estável (por exemplo, nota igual a **4** e humor **"cansado"**), o sistema enviará ao modelo de Inteligência Artificial:
- O prompt do sistema previamente definido;
- O humor selecionado pelo usuário;
- Os dados de conectividade e geolocalização provenientes do dataset **Vísent CDRView**.
Com essas informações, a IA poderá gerar uma resposta:
- Personalizada;
- Contextualizada;
- Culturalmente relevante.
## Camada 3: Pós-processamento (Sanitização)
Após receber o texto produzido pela Inteligência Artificial, o backend realiza uma etapa de validação antes de enviá-lo ao usuário.
Esse processo verifica, por meio de expressões regulares ou filtros de palavras-chave, se a resposta contém:
- Termos proibidos;
- Promessas médicas;
- Diagnósticos inadequados;
- Qualquer conteúdo que viole as diretrizes de segurança da plataforma.
Somente após essa validação o backend monta a resposta definitiva, que será exibida pelo Frontend de forma amigável e acolhedora.
# Conclusão
Com os contratos de API claramente definidos e o prompt do sistema devidamente estruturado, a equipe de Backend e o Engenheiro de Inteligência Artificial podem trabalhar de maneira independente, porém totalmente coordenada, desde o primeiro dia do desenvolvimento do projeto.
Essa arquitetura facilita a integração entre os componentes da aplicação, reduz riscos durante a implementação e garante que o comportamento do agente de IA permaneça:
- Seguro;
- Previsível;
- Alinhado aos objetivos humanos do App BiT.
Além disso, a separação entre as regras determinísticas do backend e os componentes generativos da Inteligência Artificial assegura maior confiabilidade em cenários críticos, especialmente aqueles relacionados à saúde mental e ao acolhimento de usuários em situação de vulnerabilidade.
Dessa forma, o App BiT estabelece uma base sólida para evoluir de um MVP para uma plataforma escalável, resiliente e socialmente relevante, capaz de promover inclusão, desenvolvimento profissional e bem-estar de forma integrada.
