# Chatbot Telegram - Consulta de Clima (Projeto Pós-Graduação IA)

Este projeto consiste em um chatbot para o Telegram integrado com a API do **OpenWeatherMap**, desenvolvido utilizando a plataforma de automação **n8n**. O bot recebe o nome de uma cidade enviado pelo usuário, consulta as informações climáticas atuais e retorna uma resposta formatada.

## 🚀 Funcionalidades
* Integração nativa com a API do Telegram.
* Consulta de dados meteorológicos em tempo real via OpenWeatherMap.
* Tratamento e formatação de dados para melhor legibilidade do usuário.

## 🔄 Fluxo da Automação

O workflow executa os seguintes passos:

```text
Telegram Trigger
      ↓
Recebe mensagem do usuário
      ↓
Formata a cidade recebida
      ↓
Consulta API OpenWeatherMap
      ↓
Valida resposta
      ↓
Formata mensagem
      ↓
Envia resposta ao Telegram
```

### Exemplo de Entrada

```text
Rio de Janeiro
```

### Exemplo de Saída

```text
🌤️ A temperatura em Rio de Janeiro é de 25°C.
```

### Exemplo de Erro

```text
❌ Cidade não encontrada. Utilize um nome de cidade válido.
```


---

## 🛠️ Pré-requisitos & Variáveis de Ambiente

O projeto foi estruturado utilizando Docker Compose em arquitetura de filas para ambiente de desenvolvimento. Para garantir a segurança, as credenciais não estão expostas no fluxo e são lidas diretamente das variáveis de ambiente do sistema.

As seguintes variáveis de ambiente são esperadas nos containers do n8n:
* `TELEGRAM_BOT_TOKEN`: Token do seu bot gerado pelo `@BotFather`.
* `OPENWEATHER_API_KEY`: Chave de API gerada na plataforma OpenWeatherMap.

---

## 🐳 Configuração do Docker Compose

Este projeto utiliza Docker Compose para executar todos os serviços necessários (n8n, PostgreSQL, Redis e Ngrok).

Após clonar o repositório, abra o arquivo `docker-compose.yml` e substitua os valores indicados pelos seus próprios dados.

### Variáveis que devem ser preenchidas

#### OpenWeatherMap

Substitua:

```yaml
OPENWEATHER_API_KEY=[SUA_OPENWEATHER_API_KEY]
```

Pela sua chave obtida na plataforma OpenWeatherMap.

Exemplo:

```yaml
OPENWEATHER_API_KEY=sua_chave_aqui
```

---

#### Telegram Bot

Substitua:

```yaml
TELEGRAM_BOT_TOKEN=[SEU_TOKEN_TELEGRAM]
```

Pelo token gerado através do BotFather.

Exemplo:

```yaml
TELEGRAM_BOT_TOKEN=123456789:AAxxxxxxxxxxxxxxxxxxxxxxxx
```

---

#### PostgreSQL

Substitua os seguintes valores:

```yaml
DB_POSTGRESDB_DATABASE=[NOME_DO_BANCO]
DB_POSTGRESDB_USER=[USUARIO_POSTGRES]
DB_POSTGRESDB_PASSWORD=[SENHA_POSTGRES]

POSTGRES_DB=[NOME_DO_BANCO]
POSTGRES_USER=[USUARIO_POSTGRES]
POSTGRES_PASSWORD=[SENHA_POSTGRES]
```

Exemplo:

```yaml
DB_POSTGRESDB_DATABASE=postgres
DB_POSTGRESDB_USER=postgres
DB_POSTGRESDB_PASSWORD=postgres

POSTGRES_DB=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
```

---

#### Ngrok

Substitua:

```yaml
WEBHOOK_URL=[SUA_URL_NGROK]
```

Pela URL pública fornecida pelo Ngrok.

Exemplo:

```yaml
WEBHOOK_URL=https://seu-dominio.ngrok-free.app
```

Também substitua:

```yaml
NGROK_AUTHTOKEN=[SEU_NGROK_AUTHTOKEN]
```

Pelo token disponível na sua conta Ngrok.

E ajuste o domínio utilizado pelo serviço:

```yaml
command: http --domain=[SEU_DOMINIO_NGROK] n8n-editor-ftr:5678
```

Exemplo:

```yaml
command: http --domain=meu-bot-clima.ngrok-free.app n8n-editor-ftr:5678
```

---

### Iniciando os Containers

Após configurar todas as variáveis, execute:

```bash
docker compose up -d
```

Para verificar se todos os serviços foram iniciados corretamente:

```bash
docker ps
```

Os seguintes containers devem estar em execução:

* n8n-editor
* n8n-worker
* postgres
* redis
* ngrok

Após a inicialização, acesse o n8n em:

```text
http://localhost:5678
```

e importe o workflow `workflow-chatbot-telegram.json`.


---

## 📥 Passos para Importar o Workflow no n8n

1. Certifique-se de que o seu n8n está rodando localmente (via Docker/Portainer).
2. Acesse a interface do n8n no seu navegador.
3. Crie um novo fluxo de trabalho (*Workflow*).
4. No canto superior direito da tela, clique nos **três pontinhos** e selecione **Import from File**.
5. Selecione o arquivo `workflow-chatbot-telegram.json` incluído neste repositório.
6. O fluxo será carregado automaticamente na sua tela.

---

## 🤖 Criando o Bot no Telegram (BotFather)

Para utilizar o chatbot, é necessário criar um bot no Telegram e obter o token de acesso.

### Passo 1 - Acessar o BotFather
1. Abra o Telegram.
2. Pesquise por `@BotFather`.
3. Inicie uma conversa com o BotFather.

### Passo 2 - Criar um novo bot
Envie o comando:

```text
/newbot
```

O BotFather solicitará:
1. Nome do bot (exemplo: Clima Brasil Bot)
2. Nome de usuário do bot (deve terminar com `bot`)

Exemplo:
```text
Nome: Clima Brasil Bot
Usuário: clima_brasil_bot
```

### Passo 3 - Obter o Token
Após a criação, o BotFather retornará uma mensagem semelhante a:

```text
Use this token to access the HTTP API:
123456789:AAxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Copie esse token e configure-o como variável de ambiente:
```env
TELEGRAM_BOT_TOKEN=seu_token_aqui
```
--- 

## 🌦️ Obtendo a API Key da OpenWeatherMap

O chatbot utiliza a API da OpenWeatherMap para consultar informações climáticas em tempo real.

### Passo 1 - Criar uma conta

1. Acesse o site oficial da OpenWeatherMap.
2. Crie uma conta gratuita utilizando seu e-mail.
3. Confirme o cadastro através do e-mail enviado pela plataforma.

### Passo 2 - Gerar uma API Key

1. Faça login na plataforma.
2. Acesse a área **API Keys**.
3. Crie uma nova chave ou utilize a chave padrão fornecida pela plataforma.

Exemplo:

```text
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### Passo 3 - Configurar no ambiente

Adicione a chave como variável de ambiente:

```env
OPENWEATHER_API_KEY=sua_api_key_aqui
```
---

## 🔑 Como Inserir as Credenciais no n8n

Para que as variáveis de ambiente do Docker sejam injetadas corretamente nos nós, siga as instruções abaixo:

### 1. Nó do OpenWeather (Consulta de Clima)
1. Abra o nó responsável por buscar o clima.
2. No parâmetro `appid` (ou correspondente à API Key), certifique-se de alterar o tipo de campo para **Expression** (clicando no botão `fx`).
3. Insira a expressão nativa para ler a variável de ambiente:
   ```javascript
   {{ $env["OPENWEATHER_API_KEY"] }}
   
### 2. Nó do Telegram (Envio/Recebimento)
1. Abra o trigger do Telegram e vá na seção de Credentials.
2. Selecione Create New Credential.
3. No campo Access Token, ative a opção Expression (fx).
4. Insira a expressão dinâmica para capturar o token injetado:
    ```javaScript
    {{ $env["TELEGRAM_BOT_TOKEN"] }}
    ```
5. Salve a credencial e certifique-se de ativar o fluxo clicando no botão Active no canto superior direito do n8n.

---

## 🤖 Como Executar e Testar o Chatbot
Com o fluxo ativado (`Active`) e o Webhook devidamente configurado (ex: via Ngrok):

1. Abra o aplicativo do Telegram e busque pelo nome do seu Bot.
2. Clique em Começar (`/start`).
3. Envie o nome de uma cidade de teste. Exemplo: `Rio de Janeiro`.
4. O que esperar de retorno: O chatbot processará a requisição, consultará a API e responderá em poucos segundos com uma mensagem formatada contendo o clima atual da cidade enviada.
