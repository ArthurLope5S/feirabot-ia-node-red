
# FeiraBot — Chatbot com Inteligência Artificial

### Assistente virtual do projeto FeiraCircular

Projeto acadêmico desenvolvido durante a Global Solution 2026/2 da FIAP, utilizando inteligência artificial, processamento de linguagem natural e automação de fluxos com Node-RED.

## Sobre o Projeto

O **FeiraBot** é um chatbot desenvolvido para o FeiraCircular, uma proposta de plataforma digital voltada à redução do desperdício de alimentos nas feiras livres de São Paulo.

O assistente foi projetado para esclarecer dúvidas sobre ofertas de alimentos, reservas, participação de feirantes, doações e localização de feiras.

A solução utiliza modelos de linguagem e regras de identificação de intenções para interpretar mensagens e gerar respostas contextualizadas.

O projeto é um protótipo acadêmico. As informações sobre ofertas, reservas e feiras são demonstrativas, sem integração com uma base de dados operacional.

## Objetivos

- Facilitar o acesso às informações do FeiraCircular.
- Automatizar o atendimento aos usuários.
- Interpretar perguntas em linguagem natural.
- Reconhecer intenções e entidades nas mensagens.
- Manter o contexto das conversas.
- Disponibilizar atendimento pelo Webchat e Telegram.
- Permitir interação por texto e áudio no Telegram.

## Tecnologias Utilizadas

| Tecnologia | Aplicação |
|---|---|
| Node-RED | Orquestração dos fluxos e integrações |
| JavaScript | Processamento de mensagens e regras de negócio |
| Groq API | Geração de respostas com LLM |
| GPT-OSS 20B | Modelo de linguagem utilizado |
| Groq Whisper | Transcrição de mensagens de áudio |
| Gemini API | Conversão de respostas em áudio |
| Telegram Bot API | Comunicação com usuários pelo Telegram |
| HTML e CSS | Interface do Webchat |
| FFmpeg | Conversão de formatos de áudio |

## Funcionalidades

### 1. Reconhecimento de Intenções

O chatbot possui cinco intenções cadastradas, cada uma com 12 exemplos de frases.

| Intenção | Finalidade |
|---|---|
| consultar_ofertas | Esclarecer dúvidas sobre alimentos disponíveis |
| reservar_alimento | Explicar o funcionamento das reservas |
| cadastrar_feirante | Orientar sobre a participação de feirantes |
| doar_alimentos | Explicar os processos de doação |
| localizar_feira | Fornecer orientações sobre localização de feiras |

O reconhecimento utiliza correspondência de frases, expressões regulares e regras de pontuação implementadas em JavaScript.

### 2. Identificação de Entidades

Foram implementadas cinco categorias de entidades com sinônimos:

- **categoria_alimento:** frutas, verduras, legumes e cestas.
- **perfil_usuario:** feirantes, consumidores e organizações sociais.
- **acao_plataforma:** comprar, reservar, doar e anunciar.
- **condicao_alimento:** excedentes, descontos e doações.
- **localidade:** São Paulo, bairros e feiras.

Essas entidades auxiliam na interpretação de diferentes formas de expressão utilizadas pelos usuários.

### 3. Memória de Contexto

O sistema utiliza o contexto de fluxo do Node-RED para armazenar informações de cada conversa.

Entre os dados registrados estão:

- Última intenção identificada.
- Entidades reconhecidas.
- Última pergunta enviada.
- Preferência de resposta por texto ou áudio.

O contexto é separado por usuário e canal de atendimento.

### 4. Integração com Telegram

O FeiraBot possui integração com o Telegram por meio do pacote `node-red-contrib-telegrambot`.

A estrutura contempla:

- Recebimento de mensagens de texto.
- Recebimento de mensagens de voz.
- Transcrição de áudio utilizando Groq Whisper.
- Geração de respostas com inteligência artificial.
- Síntese de voz utilizando Gemini.
- Envio de respostas em texto ou áudio.

Comandos disponíveis:

- `/start` — apresenta o assistente.
- `/texto` — seleciona respostas em texto.
- `/audio` — seleciona respostas em áudio.

### 5. Webchat

O projeto também possui uma interface Webchat desenvolvida com HTML, CSS e JavaScript.

O Webchat permite enviar perguntas e receber respostas em texto, utilizando uma API HTTP criada no Node-RED.

Endereço local:

`http://localhost:1880/feirabot`

A interface utiliza sessões individuais para manter o contexto das conversas.

## Arquitetura da Solução

### Processamento de mensagens

1. O usuário envia uma mensagem pelo Telegram ou Webchat.
2. O Node-RED recebe e prepara os dados.
3. O sistema identifica intenções e entidades.
4. O contexto da conversa é consultado e atualizado.
5. A requisição é enviada à API da Groq.
6. O modelo de linguagem gera a resposta.
7. O Node-RED encaminha a resposta ao canal correspondente.

### Processamento de áudio

**Entrada de áudio:**

Telegram → Node-RED → Groq Whisper → Transcrição → Processamento da mensagem

**Saída de áudio:**

Resposta da LLM → Gemini TTS → FFmpeg → Telegram

## Estrutura do Repositório

```text
feirabot-ia-node-red/
├── feirabot.json
└── README.md
```

O arquivo `feirabot.json` contém os nós, conexões, funções e configurações exportadas do Node-RED.

## Como Executar o Projeto

### Pré-requisitos

- Node.js
- Node-RED
- Pacote `node-red-contrib-telegrambot`
- FFmpeg
- Chave de API da Groq
- Chave de API do Gemini
- Token de um bot do Telegram

### 1. Instalar o Node-RED

Com o Node.js instalado, execute:

```bash
npm install -g --unsafe-perm node-red
```

Inicie o Node-RED:

```bash
node-red
```

Acesse:

`http://localhost:1880`

### 2. Instalar a integração com Telegram

No Node-RED:

1. Abra o menu principal.
2. Selecione **Manage palette**.
3. Acesse **Install**.
4. Procure `node-red-contrib-telegrambot`.
5. Instale o pacote.

### 3. Importar o fluxo

1. Abra o menu do Node-RED.
2. Selecione **Import**.
3. Escolha o arquivo `feirabot.json`.
4. Importe o fluxo para o ambiente.

### 4. Configurar as credenciais

O projeto utiliza variáveis de ambiente para acessar as APIs externas:

```text
GROQ_API_KEY
GEMINI_API_KEY
```

Essas variáveis devem ser configuradas no ambiente em que o Node-RED é executado.

O token do Telegram deve ser configurado no nó de configuração do bot.

**Nunca publique tokens ou chaves de API no GitHub.**

### 5. Configurar o processamento de áudio

Instale o FFmpeg e disponibilize seu executável no PATH do sistema.

Na configuração original para Windows, o projeto utiliza a pasta:

```text
C:\NodeRED_Audios
```

Caso utilize outro sistema operacional, será necessário adaptar os caminhos de armazenamento e processamento de áudio.

### 6. Executar

Após configurar as dependências e credenciais:

1. Ative o receptor do Telegram, caso esteja desativado.
2. Clique em **Deploy** no Node-RED.
3. Acesse o Webchat em `http://localhost:1880/feirabot`.
4. Envie perguntas para testar o atendimento.
5. Teste também a integração pelo Telegram.

## Segurança e Boas Práticas

O projeto foi estruturado para utilizar variáveis de ambiente em vez de armazenar chaves de API diretamente no código.

Também inclui:

- Validação básica das mensagens recebidas pelo Webchat.
- Separação de contexto por sessão.
- Tratamento de algumas falhas de integração.
- Instruções para evitar respostas com dados reais inexistentes.
- Orientação para não solicitar informações pessoais sensíveis.

## Limitações

O FeiraBot é um protótipo acadêmico e apresenta algumas limitações:

- Não consulta ofertas reais em um banco de dados.
- Não realiza reservas ou doações efetivas.
- Não processa pagamentos.
- Não possui informações atualizadas sobre todas as feiras.
- Depende de serviços externos para geração de respostas e processamento de áudio.
- Requer configuração local para executar todas as integrações.

## Relação com o FeiraCircular

O FeiraBot faz parte da proposta FeiraCircular, que busca reduzir o desperdício de alimentos nas feiras livres de São Paulo.

O chatbot representa uma possível interface de atendimento da plataforma.

A modelagem de banco de dados do FeiraCircular está disponível em outro repositório:

[FeiraCircular — Modelagem de Banco de Dados](https://github.com/ArthurLope5S/desperdicio-alimentos-sp)

## Status do Projeto

**Protótipo acadêmico desenvolvido em Node-RED.**

O fluxo contém as integrações e funcionalidades descritas neste documento. Sua execução depende da configuração das credenciais, dos serviços externos e das dependências necessárias.

## Contexto Acadêmico

**Instituição:** FIAP — Faculdade de Informática e Administração Paulista

**Curso:** Análise e Desenvolvimento de Sistemas

**Projeto:** Global Solution 2026/2 — CitizenTech

**Área:** Inteligência Artificial e Chatbots

---
Projeto acadêmico que combina inteligência artificial, automação e processamento de linguagem natural para apoiar uma solução voltada à sustentabilidade.
