# 🌤️ Chatbot de Previsão do Tempo via Telegram (com n8n)

Este repositório contém o workflow de um chatbot para Telegram desenvolvido em **n8n**. O bot tem como objetivo informar a temperatura atual de qualquer cidade brasileira, consultando a API da OpenWeatherMap.

O projeto foi desenvolvido com foco em robustez (tratamento de erros) e segurança (não exposição de credenciais no código).

---

## 🚀 Funcionalidades

* **Consulta em Tempo Real:** O usuário envia o nome de uma cidade e recebe a temperatura atualizada.
* **Tratamento de Erros:** Se a cidade digitada não existir ou houver erro na API, o bot retorna uma mensagem amigável orientando o usuário, em vez de travar.
* **Formatação de Dados:** A temperatura é arredondada para um número inteiro e o texto de entrada é normalizado para melhorar a busca.
* **Segurança:** O workflow foi configurado para utilizar variáveis de ambiente para a chave da API OpenWeather, evitando que segredos fiquem expostos no arquivo JSON.

## 📋 Pré-requisitos

Para utilizar este workflow, você precisará de:

1.  Uma instância do **n8n** rodando (localmente ou em nuvem).
2.  Um **Token de Bot do Telegram** (obtido através do @BotFather).
3.  Uma **API Key da OpenWeather** (gratuita).

## ⚙️ Como Importar e Configurar

Siga os passos abaixo para colocar o bot em funcionamento na sua instância do n8n.

### Passo 1: Importar o Workflow

1.  Faça o download do arquivo `workflow-chatbot-telegram.json` presente neste repositório.
2.  No painel do seu n8n, clique no menu (canto superior direito) e selecione **"Import from File"**.
3.  Selecione o arquivo `.json` baixado.

### Passo 2: Configurar Credenciais do Telegram

O workflow utiliza nós do Telegram que precisam ser autenticados.

1.  Abra o nó **"Telegram Trigger1"** (o primeiro nó).
2.  Em "Credential to connect with", clique para selecionar e escolha **"Create New Credential"**.
3.  Dê um nome para a credencial e cole o **Token** que você recebeu do @BotFather.
4.  Clique em "Save".

### Passo 3: Configurar a Chave da OpenWeather (Atenção ⚠️)

Por questões de segurança, o nó **"HTTP Request"** neste workflow foi configurado para buscar a chave da API em uma variável de ambiente do sistema, usando a expressão `{{ $env.OPENWEATHER_API_KEY }}`.

Você tem duas opções para configurar sua chave:

* **Opção A (Recomendada - Mais Segura):**
    Configure uma variável de ambiente no servidor onde seu n8n está rodando com o nome `OPENWEATHER_API_KEY` e o valor da sua chave.

* **Opção B (Manual - Mais Simples):**
    1. Abra o nó **"HTTP Request"** no workflow importado.
    2. Localize o parâmetro `appid` na lista de "Query Parameters".
    3. Apague a expressão que está lá (`{{ $env.OPENWEATHER_API_KEY }}`).
    4. Cole a sua chave de API da OpenWeather diretamente no campo "Value".

## 🧪 Como Executar e Testar

Após configurar as credenciais:

1.  Certifique-se de que o workflow está **Ativo**. Verifique a chave "Active" no canto superior direito da tela do n8n (ela deve estar verde).
2.  Abra a conversa com o seu bot no Telegram.

### Cenários de Teste:

* **Cidade Válida:**
    * *Envie:* `Curitiba`
    * *Resposta esperada:* "🌤️ A temperatura em Curitiba é de 18°C."

* **Cidade com Estado (Normalização):**
    * *Envie:* `São Paulo, SP`
    * *Resposta esperada:* "🌤️ A temperatura em São Paulo é de 22°C."

* **Cidade Inexistente (Teste de Erro):**
    * *Envie:* `CidadeQueNaoExiste123`
    * *Resposta esperada:* "❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP)."

---
Desenvolvido como parte do desafio prático de automação com n8n.
