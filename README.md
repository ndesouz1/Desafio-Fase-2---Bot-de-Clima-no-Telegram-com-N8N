# 🌤️ Chatbot de Previsão do Tempo (Telegram + n8n)

Este repositório contém um workflow automatizado para o **n8n** que cria um chatbot no Telegram. O bot recebe o nome de uma cidade, consulta a API da OpenWeatherMap e retorna a temperatura atual formatada.

## 🚀 Funcionalidades

* **Consulta em Tempo Real:** Integração direta com a API OpenWeather.
* **Tratamento de Erros:** Identifica quando uma cidade não é encontrada e retorna uma mensagem amigável ao usuário, sem travar o fluxo.
* **Formatação Inteligente:** Limpa o texto de entrada e arredonda valores de temperatura.
* **Segurança:** O workflow está configurado para não expor chaves de API diretamente no código (uso de variáveis de ambiente).

## 🛠️ Pré-requisitos

Para rodar este projeto, você precisará de:

1.  Uma instância do **n8n** (Desktop ou Cloud).
2.  Uma conta no Telegram e um bot criado via **@BotFather** (para obter o Token).
3.  Uma conta na **OpenWeatherMap** (para obter a API Key).

## ⚙️ Instalação e Configuração

### 1. Importar o Workflow
1.  Baixe o arquivo `workflow-chatbot-telegram.json` deste repositório.
2.  No seu n8n, clique no menu no canto superior direito > **Import from File**.
3.  Selecione o arquivo baixado.

### 2. Configurar Credenciais do Telegram
1.  Abra o nó **Telegram Trigger** ou **Send a text message**.
2.  Em "Credential to connect with", selecione **Create New**.
3.  Escolha "Telegram API" e insira o Token fornecido pelo BotFather.

### 3. Configurar a API Key da OpenWeather
⚠️ **Atenção:** Este workflow foi exportado utilizando uma variável de ambiente para maior segurança. Você tem duas opções para configurar a chave:

* **Opção A (Recomendada - Variável de Ambiente):**
    Configure uma variável de ambiente no seu n8n chamada `OPENWEATHER_API_KEY` com o valor da sua chave.

* **Opção B (Manual):**
    1. Abra o nó **HTTP Request**.
    2. No parâmetro `appid`, apague a expressão `{{ $env.OPENWEATHER_API_KEY }}`.
    3. Cole sua API Key diretamente no campo "Value".

## 🧪 Como Usar

1.  Certifique-se de que o workflow está **Ativo** (chave "Active" verde no topo do n8n).
2.  Abra o seu bot no Telegram.
3.  Envie o nome de uma cidade (ex: `Curitiba` ou `Rio de Janeiro`).
4.  O bot responderá com a temperatura atual (ex: "🌤️ A temperatura em Curitiba é de 17°C.").

## 📂 Estrutura do Workflow

1.  **Telegram Trigger:** Recebe a mensagem.
2.  **Edit Fields (Limpeza):** Normaliza o texto (remove espaços).
3.  **HTTP Request:** Consulta a API da OpenWeather.
    * *Configurado com "Continue On Fail" para tratar erros 404.*
4.  **If:** Verifica se a cidade foi encontrada.
5.  **Edit Fields (Resposta):** Prepara a mensagem de sucesso ou de erro.
6.  **Telegram Response:** Envia a resposta final ao usuário, garantindo o ID correto do chat.

---
Desenvolvido como parte de um desafio prático de automação com n8n.
