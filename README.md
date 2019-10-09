# Desafio IBM | Experimente BB

[![IBM Cloud Powered](https://img.shields.io/badge/IBM%20Cloud-powered-blue.svg)](https://ibm.biz/Bdzhws)
[![Platform Node.JS](https://img.shields.io/badge/platform-nodejs-lightgrey.svg?style=flat)](https://developer.ibm.com/node/)

## Passo a passo (leia com atenção)

* [1. Sobre o Desafio](#1-sobre-o-desafio)
* [2. Pré-requisitos](#2-pré-requisitos)
* [3. Deploy do Chatbot](#3-deploy-do-chatbot)
* [4. Treinamento do Modelo Preditivo](#4-treinamento-do-modelo-preditivo)
* [5. Desenvolvimento da Action](#5-desenvolvimento-da-action)
    * [5.1. Criar instância de Natural Language Understanding e Language Translator](#51-criar-instância-de-natural-language-understanding-e-language-translator)
    * [5.2. Detalhe da função e exemplo em Javascript](#52-detalhe-da-função-e-exemplo-em-javascript)
    * [5.3. Expor a Action como API](#53-expor-a-action-como-api)
* [6. Configurar Webhook no Watson Assistant](#6-configurar-webhook-no-watson-assistant)
* [7. Deploy da página de chat](#7-deploy-da-página-de-chat)
* [8. Submissão do desafio](#8-submissão-do-desafio)

## 1. Sobre o Desafio

Neste desafio, desenvolvido pela IBM, os participantes irão testar os conhecimentos na plataforma IBM Cloud baseado no tema proposto pelo Banco do Brasil: **Computação Afetiva**. Cada participante irá seguir o guia abaixo e desenvolver na sua conta da [IBM Cloud](https://cloud.ibm.com).

Você irá aprender, pelos materiais apresentados e pelo acesso a documentação dos serviços (veja a lista abaixo), a treinar um modelo preditivo com o Modeler Flow e disponibilizar no Watson Machine Learning, além de desenvolver a sua própria função dentro do IBM Cloud Functions, plataforma Serverless baseado no Apache OpenWhisk. Tudo isso para entregar uma assistente virtual capaz de analisar emoções e analisar o melhor perfil de investimento, baseado no histórico de recomendações com o uso de Machine Learning.

Você irá utilizar os seguintes serviços:

* Watson Assistant;
* Watson Machine Learning;
* Watson Studio;
* Watson Natural Language Understanding;
* Language Translator;
* IBM Cloud Functions;
* Cloud Foundry;
* Cloud Object Storage.

## 2. Pré-requisitos

* Entrar na comunidade do Banco do Brasil (restrito aos funcionários);
* Ter uma conta na [IBM Cloud](https://cloud.ibm.com) com o email corporativo (seu email do banco) - [clique aqui](https://ibm.biz/Bdzhws) para criar a sua conta;
* Ter uma conta no [Github](https://github.com), para dar a permissão para acessar o repositório.

**Obs:** Caso você tenha um problema para registrar uma nova conta na IBM Cloud, tente novamente. Se o erro persistir, você pode criar a conta no mesmo link e usando 4G.

**Obs2:** Se você tiver um email com `.` (ponto), mude para `-` (hífen) para que funcione todo o processo. Ele será alterado no dashboard para mostrar corretamente. Ex: `victor.shinya@ibm.com` -> `victor-shinya@ibm.com`.

## 3. Deploy do Chatbot

Você não precisa construir o chatbot. No Desafio da IBM você irá encontrar o Skill da Katia, a assistente virtual para analisar o perfil de investimento. Você deverá importar o Skill na sua instância de Watson Assistant e configurar o **Webhook** para chamar a sua API (no passo 5) para analisar emoções e identificar o perfil de investimento.

Acesse o link da Skill, clique no botão *Raw* e salve a página com o formato `.json` (Ex: `skill.json`) - [clique aqui](doc/source/assistant/skill.json).

Acesse a sua conta na IBM Cloud (caso não tenha, siga o passo anterior), e acesse o [catálogo de serviços na aba AI (Inteligência Artificial)](https://cloud.ibm.com/catalog?category=ai). Clique sobre o serviço de Watson Assistant e depois clique em *Create*.

<div align="center">
    <img src="doc/source/images/Watson Assistant 01.png" alt="IBM Cloud Catalog" width="375">
    <img src="doc/source/images/Watson Assistant 02.png" alt="IBM Watson Service" width="375">
</div>

Na página de detalhes da sua instância de Watson Assistant você deve clicar no botão *Launch Watson Assistant*. Vai abrir uma nova aba com a plataforma de treinamento. Você irá importar um chatbot pronto no serviço. Clique em *Skills* e depois clique em *Create skill*.

<div align="center">
    <img src="doc/source/images/Watson Assistant 03.png" alt="Detail IBM Watson" width="375">
    <img src="doc/source/images/Watson Assistant 04.png" alt="Detail IBM Watson" width="375">
</div>

<div align="center">
    <img src="doc/source/images/Watson Assistant 05.png" alt="IBM Watson Platform" width="375">
    <img src="doc/source/images/Watson Assistant 06.png" alt="Import new Skill" width="375">
</div>

Passos da imagem acima:

1. Baixe a Skill do Watson e busque na sua área de download.
2. Clique em *Import* para importar na sua instância de Watson Assistant.

## 4. Treinamento do Modelo Preditivo

Neste desafio você deve treinar um modelo preditivo usando o serviço do Watson Studio e Machine Learning, com o uso da ferramenta Modeler Flow (um dos recursos dentro do Watson Studio). Para criar a sua instância de Watson Studio e Watson Machine Learning, acesse o link para abrir o [catálogo de serviços na aba AI (Inteligência Artificial)](https://cloud.ibm.com/catalog?category=ai).

Caso ainda não tenha visto o vídeo distribuído no canais internos, você pode ver o vídeo abaixo.

Você irá acessar o Watson Studio, criar um novo projeto e habilitar o uso do Modeler Flow (também conhecido como SPSS Modeler), a ferramenta para treinamento de modelos de machine learning e preditivo. Siga o passo a passo no vídeo abaixo usando o dataset, fornecido no link abaixo, para treinar o seu modelo. Basta substituir o dataset e configurar com os campos **salario**, **gasto_mensal**, **filhos** e **escolaridade** como parâmetro de entrada.

Você deve baixar o [dataset](doc/source/dataset/dataset.csv), para treinar o modelo seguindo o vídeo acima. Acesse o link, clique no botão *Raw* e salve a página com o formato `.csv` (Ex: `dataset.csv`) - [clique aqui](doc/source/dataset/dataset.csv).

<div align="center">
    <a href="https://youtu.be/6qXR6sEFz3Y">
        <img width="375" src="doc/source/images/Youtube-MachineLearning.png">
    </a>
</div>

## 5. Desenvolvimento da Action

Após o treinamento do seu modelo preditivo, você vai criar uma *Action*, um bloco de código dentro da plataforma Serverless da IBM, **IBM Cloud Functions**, que deverá executar dois tipos de operações: analisar as emoções e analisar perfil de investimento (usando o modelo preditivo, criado no passo anterior).

Para conhecer sobre o **IBM Cloud Functions**, acesse o [blog aqui](https://medium.com/ibmdeveloperbr/serverless-com-ibm-cloud-functions-como-funciona-esse-tal-serverless-f24be837b7a4). E para extrair as emoções, você deverá usar o serviço de **Natural Language Understanding** (veja mais detalhes abaixo).

### 5.1. Criar instância de Natural Language Understanding e Language Translator

Para analisar emoções a partir das mensagens enviadas pelo usuário você deverá usar o **Natural Language Understanding** (também conhecido como **NLU**), o serviço de NLP (Natural Language Processing), que tem como uma das funcionalidades a análise de emoções. No entanto, essa funcionalidade não está disponível em Português (pt-br). Nesse caso, será necessário traduzir o texto para a língua Inglês (en-us) para retornar a lista de emoções. Você deverá criar também uma instância do **Language Translator** para fazer a tradução.

Para criar a instância do NLU, você irá abrir o catálogo da [IBM Cloud, na categoria AI (ou IA)](https://cloud.ibm.com/catalog?category=ai) e localizar o serviço. Clique sobre o serviço e depois clique no botão *Create*. Você será redirecionado a tela de detalhes da sua instância.

### Instância de Natural Language Understanding

<div align="center">
    <img src="doc/source/images/NLU 01.png" alt="IBM Cloud Catalog" width="375">
    <img src="doc/source/images/NLU 02.png" alt="IBM Watson Service" width="375">
</div>

Acesse a aba *Manage*. Salve o *API Key* que aparece na sua tela, pois você usará no desenvolvimento da sua função, ao conectar o seu código com a instância criada na sua conta. Basta clicar no ícone marcado na imagem abaixo. Ou, caso não funcione, clique sobre o botão *Show Credentials* e copie o código que irá aparecer na tela.

<div align="center">
    <img src="doc/source/images/NLU 03.png" alt="IBM Cloud Catalog" width="375">
</div>

<div align="center">
    <p><b>🚨 Salve a API Key do Natural Language Understanding. Você usará para desenvolver a API que será usada no Webhook dentro do Watson Assistant 🚨</b></p>
</div>

### Instância de Language Translator

Você vai fazer o mesmo procedimento com o serviço do Language Translator. Acesse o catálogo da [IBM Cloud, na categoria AI (ou IA)](https://cloud.ibm.com/catalog?category=ai) e localize o serviço. Clique sobre o serviço e depois clique no botão *Create*. Você será redirecionado a tela de detalhes da sua instância.

<div align="center">
    <img src="doc/source/images/Language Translator 01.png" alt="IBM Cloud Catalog" width="375">
    <img src="doc/source/images/Language Translator 02.png" alt="IBM Watson Service" width="375">
</div>

<div align="center">
    <p><b>🚨 Salve a API Key do Language Translator. Você usará para desenvolver a API que será usada no Webhook dentro do Watson Assistant 🚨</b></p>
</div>

### 5.2. Detalhe da função e exemplo em Javascript

[Clique aqui](doc/source/js/action.js) para acessar o exemplo da Action que será usada pelo Webhook no Watson Assistant. A função deve receber um parâmetro chamado "*type*" que pode ser "*ML*" ou "*Emotions*". Esse dado será usado para identificar qual serviço deverá ser chamado, ou o **Language Translator** com o **Natural Language Understanding** ou o **Machine Learning**.

Acesse a plataforma do [**IBM Cloud Functions**](https://cloud.ibm.com/functions/actions) e crie uma nova *Action* e importe o código (nesse caso, você precisa copiar e colar o código). Se você ainda não leu, acesse o [blog para aprender a usar o **IBM Cloud Functions**](https://medium.com/ibmdeveloperbr/serverless-com-ibm-cloud-functions-como-funciona-esse-tal-serverless-f24be837b7a4).

Caso utilize o exemplo pronto no link acima, veja que você terá que inserir dados (*API Key* e *URL*) nas linhas: **04**, **12**, **56** e **80**, além de ter que desenvolver a chamada de API para o NLU na linha **46**.

Acesso ao API Docs do serviço:

* Natural Language Understanding: https://cloud.ibm.com/apidocs/natural-language-understanding?code=node#analyze-text
* Language Translator: https://cloud.ibm.com/apidocs/language-translator?code=node#translate

A função desenvolvida receberá o seguinte JSON:

**Caso 01.** Chamada de API pelo webhook no Watson Assistant para analisar a emoção da frase.

```json
{
    "type": "Emotions",
    "text": "<texto enviado pelo usuário>"
}
```

Retorno esperado:

> Note que naturalmente o item *top_emotion* não é retornado pelo NLU. Nesse caso, você deve localizar qual é a emoção com maior nível de confiança (valor de 0 a 1) e salvar nesse novo item.

```json
{
    "emotion": {
        "document": {
            "emotion": {
                "anger": 0.233377,
                "disgust": 0.068175,
                "fear": 0.116511,
                "joy": 0.034529,
                "sadness": 0.727336
            }
        }
    },
    "language": "en",
    "top_emotion": "sadness",
    "usage": {
        "features": 1,
        "text_characters": 69,
        "text_units": 1
    }
}
```

**Caso 02.** Chamada de API pelo webhook no Watson Assistant para analisar o perfil de investimento (baseado no seu treinamento no Watson Studio e Watson Machine Learning).

```json
{
    "type": "ML",
    "filhos": "<números de filhos>",
    "salario": "<salário mensal>",
    "gastoMensal": "<gasto mensal>",
    "escolaridade": "<nível de escolaridade>"
}
```

Retorno esperado:

```json
{
    "err": false,
    "produto": "Agressivo"
}
```

🚨 **Você pode desenvolver a função EM QUALQUER LINGUAGEM (de programação). Não existe preferência. Desde que siga as regras descritas acima.** 🚨

### 5.3. Expor a Action como API

Agora que você já criou a sua função, chegou a hora de expor a sua *Action* como uma API. Para isso, basta você acessar a sua função e habilitar a opção *Web Action*, dentro da aba *Endpoints*.

<div align="center">
    <img src="doc/source/images/API 01.png" alt="Expor via API" width="750">
    <img src="doc/source/images/API 02.png" alt="Expor via API" width="750">
</div>

Na aba *Endpoints*, você deve habilitar a opção de *Web Action*. Isso vai liberar a URL que está na imagem. Com a URL você irá colocar o formato `.json` no final (veja o exemplo na imagem).

<div align="center">
    <img src="doc/source/images/API 03.png" alt="Expor via API" width="750">
    <img src="doc/source/images/API 04.png" alt="Expor via API" width="750">
</div>

## 6. Configurar Webhook no Watson Assistant

Para configurar o webhook, fizemos um vídeo para te guiar na tarefa. Assista o vídeo abaixo e veja como fazer dentro do Watson Assistant.

<div align="center">
    <a href="https://youtu.be/nqgi1Dr8tJc">
        <img width="375" src="doc/source/images/Youtube-Action.png">
    </a>
</div>

Ainda com dúvida? Acesse a documentação do Watson Assistant e leia o [guia completo](https://cloud.ibm.com/docs/services/assistant?topic=assistant-dialog-webhooks&locale=pt-br).

## 7. Deploy da página de chat

Para subir a aplicação na IBM Cloud, você deve `clicar no botão` abaixo para subir usando o IBM Continuous Delivery (também conhecido como Delivery Pipeline).

<div align="center">
    <p><b>🚨 Clique para fazer o DEPLOY da aplicação na IBM Cloud 🚨</b></p>
    <a href="https://cloud.ibm.com/devops/setup/deploy?repository=https://github.com/experimente-bb/desafio.git">
        <img src="https://cloud.ibm.com/devops/setup/deploy/button.png" alt="Deploy to IBM Cloud">
    </a>
</div>

Ao clicar no botão, você será redirecionado a tela de configuração do Delivery Pipeline. Segue abaixo os campos que devem ser preenchidos e como você deve preencher.

<div align="center">
    <img src="doc/source/images/Pipeline 01.png" alt="Configuração do Pipeline" width="375">
</div>

<div align="center">
    <img src="doc/source/images/Pipeline 02.png" alt="Configuração do Pipeline" width="375">
    <img src="doc/source/images/Pipeline 03.png" alt="Configuração do Pipeline" width="375">
</div>

Passos:

1. Preencha com o seu email, removendo a parte do `@` em diante. Ex: Email: `vshinya@br.ibm.com`; ID: `vshinya`.
2. Basta clicar no botão *Create* e clicar no botão *Create*.
3. *Veja na imagem abaixo, dentro da plataforma do Watson Assistant*.
4. *Veja na imagem abaixo, dentro da plataforma do Watson Assistant*.
5. *Veja na imagem abaixo, dentro da plataforma do Watson Assistant*. **Fique atento**: como a instância do Watson Assistant, na imagem abaixo, está na região de *Dallas* (ou *US-South*/*Sul dos EUA*), a URL do Watson Assistant será `https://gateway.watsonplatform.net/assistant/api`. Se a sua instância estiver em *Washington-DC* (ou *US-East*/*Leste dos EUA*), por exemplo, então a URL será `https://gateway-wdc.watsonplatform.net/assistant/api`.

<div align="center">
    <img src="doc/source/images/Watson Assistant 07.png" alt="Credenciais do Watson Assistant" width="750">
</div>

Após preencher, você deve clicar no botão *Create*, localizado no canto superior direito. Feito isso, você será redicionado a página inicial da sua *Toolchain*, seu ambiente dentro do *Delivery Pipeline*. Clique no quadrado do *Delivery Pipeline* e espere até o processo de *Build* e *Deploy* finalizar (e passar).

<div align="center">
    <img src="doc/source/images/Pipeline 04.png" alt="Configuração do Pipeline" width="375">
    <img src="doc/source/images/Pipeline 05.png" alt="Configuração do Pipeline" width="375">
</div>

Ao concluir o processo acima, você deve clicar no botão *View console*. Você será redirecionado a página de detalhes da sua aplicação. Dentro dessa tela, você irá clicar no botão *View App URL*.

<div align="center">
    <img src="doc/source/images/Pipeline 06.png" alt="Detalhes da Aplicação" width="750">
</div>

Como resultado final, você verá a aplicação abaixo. Agora você consegue testar o seu chatbot e fazer a submissão no final.

<div align="center">
    <img src="doc/source/images/App 01.png" alt="Detalhes da Aplicação" width="750">
</div>

## 8. Submissão do desafio

A submissão é feita através do chatbot Katia. Após os testes e validações do seu treinamento e desenvolvimento, você deve mandar a seguinte mensagem "`Quero submeter o meu desafio`". Você irá confirmar e depois disso, o seu desafio será analisado e pontuado. Você receberá o feedback no chatbot.

Você pode submeter quantas vezes quiser. Será armazenado a maior pontuação entregue para cada participante.

🚨 **Você pode submeter quantas vezes quiser. Não há limite de submissões.** 🚨

Quer saber a sua nota? Acesse o [dashboard](https://ibmdegla-rkp.us-south.containers.appdomain.cloud/) com os resultados.

---

## License

Copyright 2019 Maratona Behind the Code

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
