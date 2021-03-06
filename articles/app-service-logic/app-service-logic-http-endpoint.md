<properties
   pageTitle="Aplicativos lógicos como pontos de extremidade escaláveis"
   description="Como criar e configurar o Ouvinte HTTP e usá-lo em um aplicativo lógico no Serviço de Aplicativo do Azure"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="02/17/2016"
   ms.author="stepsic"/>


# Aplicativos lógicos como pontos de extremidade escaláveis

A versão anterior do esquema de Aplicativos lógicos (*2014-12-01-preview*) exigia um aplicativo de API chamado **Ouvinte de HTTP** para expor um ponto de extremidade HTTP que pode ser chamado de forma síncrona. Com o esquema mais recente (*2015-08-01-preview*), os aplicativos lógicos podem expor de forma nativa um ponto de extremidade HTTP síncrono.

## Adição de um gatilho para sua definição
A primeira etapa é adicionar um gatilho à sua definição de Aplicativo lógico que pode receber solicitações de entrada. Há três tipos de gatilhos que podem receber solicitações: * manual * apiConnectionWebhook * httpWebhook

Para o restante do artigo, usaremos **manual** como exemplo, mas todas os princípios são aplicados de forma idêntica aos outros dois tipos de gatilhos. A adição do gatilho à sua definição de fluxo de trabalho a deixará parecida com o seguinte:

```
{
    "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
    "triggers" : {
        "myendpointtrigger" : {
            "type" : "manual"
        }
    }
}
```

Isso criará um ponto de extremidade que pode ser chamado em uma URL como esta:
 
```
https://prod-03.brazilsouth.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?...
```

Você pode obter esse ponto de extremidade na interface do usuário, ou, chamando:

```
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

## Chamar o ponto de extremidade do gatilho do Aplicativo lógico
Depois de obter o ponto de extremidade de seu gatilho, você pode salvá-lo em seu sistema back-end e chamá-lo por meio de um `POST` para a URL completa. Você pode incluir outros parâmetros de consulta, cabeçalhos e qualquer conteúdo em seu corpo.

Se o content-type for `application/json`, você poderá fazer referência a propriedades de dentro da solicitação. Caso contrário, ele será tratado como uma única Unidade binária que pode ser passada para outras APIs, mas que não pode ser referenciada dentro.

## Como fazer referência ao conteúdo da solicitação de entrada
A função `@triggerOutputs()` produzirá o conteúdo da solicitação de entrada. Por exemplo, ele teria a seguinte aparência:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Você pode usar o atalho `@triggerBody()` para acessar a propriedade `body` de forma específica.

Essa é uma pequena diferença em relação à versão *2014-12-01-preview*, onde você acessaria o corpo de um Ouvinte HTTP por meio de uma função como: `@triggerOutputs().body.Content`.

## Como responder à solicitação
Para algumas solicitações que iniciam um Aplicativo lógico, convém responder com algum conteúdo para o chamador. Há um novo tipo de ação chamado **response** que pode ser usado para construir o código de status, o corpo e os cabeçalhos de resposta. Observe que se não houver uma forma **response**, o ponto de extremidade do Aplicativo lógico responderá *imediatamente* com **202 Aceito** (isso é equivalente a *Enviar resposta automaticamente* no Ouvinte HTTP).

```
"myresponse" : {
    "type" : "response",
    "inputs" : {
        "statusCode" : 200,
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        }
    },
    "conditions" : []
}
```

As respostas têm o seguinte:

| propriedade | Descrição |
| -------- | ----------- |
| statusCode | O código de status HTTP para responder à solicitação de entrada. Pode ser qualquer código de status válido que comece com 2xx, 4xx ou 5xx. Não há permissão para códigos de status 3xx. | 
| body | Um objeto body pode ser uma cadeia de caracteres, um objeto JSON ou até mesmo um conteúdo binário referenciado em uma etapa anterior. | 
| headers | Você pode definir qualquer quantidade de cabeçalhos para inclusão na resposta | 

Todas as etapas no Aplicativo lógico que são necessárias para a resposta devem ser concluídas em até *60 segundos* para que a solicitação original receba a solicitação. Se nenhuma ação de resposta for atingida dentro de 60 segundos, a solicitação de entrada atingirá o tempo limite e receberá uma resposta HTTP **408 Tempo limite do cliente**.

## Configuração avançada do ponto de extremidade
Aplicativos lógicos têm suporte interno para o ponto de extremidade de acesso direto e sempre usam o método `POST` para iniciar a execução. O aplicativo de API **Ouvinte HTTP** também oferecia suporte à alteração de segmentos de URL e do método HTTP. Você podia até mesmo configurar a segurança adicional ou um domínio personalizado por meio da adição ao host de aplicativo de API (o aplicativo Web que hospedava o aplicativo de API).

Essa funcionalidade está disponível por meio do **gerenciamento de API**: * [Alterar o método da solicitação](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) * [Alterar os segmentos da URL da solicitação](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL) * Configure seus domínios de gerenciamento de API na guia **Configurar** no Portal Clássico do Azure * Configure a política para verificar a autenticação básica (**link necessário**)

## Resumo de migração da 2014-12-01-preview

| 2014-12-01-preview | 2015-08-01-preview |
|---------------------|--------------------|
| Clique em aplicativo de API **Ouvinte HTTP** | Clique em **Gatilho manual** (nenhum aplicativo de API é necessário) |
| Configuração do Ouvinte HTTP "*Envia a resposta automaticamente*" | Inclui ou não uma ação de **resposta** na definição do fluxo de trabalho |
| Configurar a autenticação básica ou OAuth | por meio do gerenciamento de API |
| Configurar o método HTTP | por meio do gerenciamento de API |
| Configurar o caminho relativo | por meio do gerenciamento de API |
| Referenciar o corpo de entrada por meio de `@triggerOutputs().body.Content` | Referenciar por meio de `@triggerOutputs().body` |
| Ação **Enviar resposta HTTP** no Ouvinte HTTP | Clique em **Responder à solicitação HTTP** (nenhum aplicativo de API é necessário)

<!---HONumber=AcomDC_0224_2016-->