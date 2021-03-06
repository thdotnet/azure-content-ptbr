<properties 
	pageTitle="Modelo de dados do Application Insights" 
	description="Descreve as propriedades exportadas de exportação contínua em JSON e usados como filtros." 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/21/2016" 
	ms.author="awills"/>

# Modelo de dados de exportação do Application Insights

Esta tabela lista as propriedades de telemetria enviadas dos SDKs do [Application Insights](app-insights-overview.md) para o portal. Você verá essas propriedades na saída de dados de [Exportação Contínua](app-insights-export-telemetry.md). Elas também aparecerão em filtros de propriedade em [Gerenciador de Métricas](app-insights-metrics-explorer.md) e em [Pesquisa de Diagnóstico](app-insights-diagnostic-search.md).

Pontos a serem observados:

* `[0]` nessas tabelas indica um ponto no caminho no qual você precisa inserir um índice; mas nem sempre é 0.
* A duração é em décimos de microssegundo, portanto, 10000000 = = 1 segundo.
* Datas e horas estão em UTC e são fornecidas no formato ISO `yyyy-MM-DDThh:mm:ss.sssZ`

Existem vários [exemplos](app-insights-export-telemetry.md#code-samples) que ilustram como usá-los.



## Exemplo

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // Always 1
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## Contexto

Todos os tipos de telemetria são acompanhados por uma seção de contexto. Nem todos esses campos são transmitidos com cada ponto de dados.



|Caminho|Tipo|Observações|
|---|---|---|
| context.custom.dimensions [0] | object [ ] | Pares de cadeira de caractere chave-valor definidos pelo parâmetro das propriedades personalizadas. Comprimento máximo da chave 100, comprimento máximo dos valores 1024. Mais de 100 valores exclusivos, é possível pesquisar na propriedade, mas não usá-la para segmentação. Máximo de 200 chaves por ikey. |
| context.custom.metrics [0] | object [ ] | Pares de chave-valor definidos pelo parâmetro de medidas personalizadas e por TrackMetrics. Comprimento máximo da chave 100, os valores podem ser numéricos. |
| context.data.eventTime | string | UTC |
| context.data.isSynthetic | booleano | A solicitação parece ser proveniente de um teste na Web ou de um bot. |
| context.data.samplingRate | número | Porcentagem de telemetria gerada pelo SDK enviado ao portal. Intervalo 0.0-100.0.|
| context.device | objeto | Dispositivo de cliente |
| context.device.deviceModel | string | |
| context.device.deviceName | string | |
| context.device.id | string | |
| context.device.locale | string | por exemplo, en-GB, de-DE |
| context.device.network | string | |
| context.device.oemName | string | |
| context.device.osVersion | string | |
| context.device.roleInstance | string | |
| context.device.roleName | string | |
| context.device.type | string | |
| context.location | objeto | Derivado de clientip. |
| context.location.city | string | |
| context.location.clientip | string | Último octógono tornado anônimo pelo valor 0. |
| context.location.continent | string | |
| context.location.country | string | |
| context.location.province | string | |
| context.operation.id | string | Itens que têm a mesma ID de operação são mostrados como Itens Relacionados no portal. Normalmente a ID da solicitação. |
| context.operation.parentId | string | Permite itens relacionados aninhados. |
| context.session.id | string | ID de um grupo de operações da mesma fonte. Um período de 30 minutos sem uma operação sinaliza o término de uma sessão. |
| context.session.isFirst | booleano | |
| context.user.accountAcquisitionDate | string | |
| context.user.anonAcquisitionDate | string | |
| context.user.anonId | string | |
| context.user.authAcquisitionDate | string | [Usuário autenticado](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | booleano | |
| internal.data.documentVersion | string | |
| internal.data.id | string | |



## Eventos

Eventos personalizados gerados por [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event).


|Caminho|Tipo|Observações|
|---|---|---|
| event [0] count | inteiro | |
| event [0] name | string | Nome do evento. Máx.de 250 caracteres |
| event [0] url | string | |
| event [0] urlData.base | string | |
| event [0] urlData.host | string | |

## Exceções

[Exceções](app-insights-asp-net-exceptions.md) do relatório no servidor e no navegador.


|Caminho|Tipo|Observações|
|---|---|---|
| basicException [0] assembly | string | |
| basicException [0] count | inteiro | |
| basicException [0] exceptionGroup | string | |
| basicException [0] exceptionType | string | |string | |
| basicException [0] failedUserCodeMethod | string | |
| basicException [0] failedUserCodeAssembly | string | |
| basicException [0] handledAt | string | |
| basicException [0] hasFullStack | booleano | |
| basicException [0] id | string | |
| basicException [0] method | string | |
| basicException [0] message | string | Mensagem de exceção. Máx. de 10 mil caracteres|
| basicException [0] outerExceptionMessage | string | |
| basicException [0] outerExceptionThrownAtAssembly | string | |
| basicException [0] outerExceptionThrownAtMethod | string | |
| basicException [0] outerExceptionType | string | |
| basicException [0] outerId | string | |
| basicException [0] parsedStack [0] assembly | string | |
| basicException [0] parsedStack [0] fileName | string | |
| basicException [0] parsedStack [0] level | inteiro | |
| basicException [0] parsedStack [0] line | inteiro | |
| basicException [0] parsedStack [0] method | string | |
| basicException [0] stack | string | Máx. de 10 mil|
| basicException [0] typeName | string | |



## Mensagens de rastreamento

Enviado por [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace) e pelos [adaptadores de registro em log](app-insights-asp-net-trace-logs.md).


|Caminho|Tipo|Observações|
|---|---|---|
| message [0] loggerName | string ||
| message [0] parameters | string ||
| message [0] raw | string | A mensagem de log, comprimento máximo de 10 mil. Você pode pesquisar nessas cadeias de caracteres no portal. |
| message [0] severityLevel | string | |



## Dependência remota

Enviado por TrackDependency. Usado para indicar o desempenho e o uso das [chamadas para dependências](app-insights-asp-net-dependencies.md) no servidor, e chamadas do AJAX no navegador.

|Caminho|Tipo|Observações|
|---|---|---|
| remoteDependency [0] async | booleano | |
| remoteDependency [0] baseName | string | |
| remoteDependency [0] commandName | string | Por exemplo, asp "home/index" |
| remoteDependency [0] count | inteiro | |
| remoteDependency [0] dependencyTypeName | string | HTTP, SQL, ... |
| remoteDependency [0] durationMetric.value | número | Tempo desde a chamada até a conclusão da resposta por dependência |
| remoteDependency [0] id | string | |
| remoteDependency [0] name | string | Url. Máx.de 250 caracteres|
| remoteDependency [0] resultCode | string | da dependência de HTTP |
| remoteDependency [0] success | booleano | |
| remoteDependency [0] type | string | Http, Sql,... |
| remoteDependency [0] url | string | Máx. de 2 mil |
| remoteDependency [0] urlData.base | string | Máx. de 2 mil |
| remoteDependency [0] urlData.hashTag | string | |
| remoteDependency [0] urlData.host | string | Máx. de 200|


## Solicitações

Enviado por [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). Os módulos padrão usam isso para indicar o tempo de resposta do servidor, medido no servidor.


|Caminho|Tipo|Observações|
|---|---|---|
| request [0] count | inteiro | |
| request [0] durationMetric.value | número | Tempo de chegada da solicitação até a resposta. 1e7 == 1s |
| request [0] id | string | ID da operação |
| request [0] name | string | GET/POST + url base. Máx.de 250 caracteres |
| request [0] responseCode | inteiro | Resposta HTTP enviada ao cliente |
| request [0] success | booleano | Padrão == responseCode<400 |
| request [0] url | string | Não incluindo o host |
| request [0] urlData.base | string | |
| request [0] urlData.hashTag | string | |
| request [0] urlData.host | string | |


## Desempenho de exibição da página

Enviado pelo navegador. Mede o tempo de processamento de uma página, desde o início da solicitação do usuário até a exibição completa (excluindo as chamadas do AJAX assíncronas).

Os valores de contexto mostram a versão do navegador e do sistema operacional cliente.


|Caminho|Tipo|Observações|
|---|---|---|
| clientPerformance [0] clientProcess.value | inteiro | Tempo desde o término do recebimento do HTML até a exibição da página. |
| clientPerformance [0] name | string | |
| clientPerformance [0] networkConnection.value | inteiro | Tempo necessário para estabelecer uma conexão de rede. |
| clientPerformance [0] receiveRequest.value | inteiro | Tempo desde o término do envio da solicitação até o recebimento do HTML na resposta. |
| clientPerformance [0] sendRequest.value | inteiro | Tempo necessário para enviar a solicitação HTTP. |
| clientPerformance [0] total.value | inteiro | Tempo desde o início até o envio da solicitação para exibição da página. |
| clientPerformance [0] url | string | URL dessa solicitação |
| clientPerformance [0] urlData.base | string | |
| clientPerformance [0] urlData.hashTag | string | |
| clientPerformance [0] urlData.host | string | |
| clientPerformance [0] urlData.protocol | string | |

## Visualizações de página

Enviado pelo trackPageView() ou [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Caminho|Tipo|Observações|
|---|---|---|
| view [0] count | inteiro | |
| view [0] durationMetric.value | inteiro | Valor definido opcionalmente em trackPageView() ou por start/stopTrackPage. Não é igual aos valores de clientPerformance. |
| view [0] name | string | Título da página. Máx.de 250 caracteres |
| view [0] url | string | |
| view [0] urlData.base | string | |
| view [0] urlData.hashTag | string | |
| view [0] urlData.host | string | |



## Disponibilidade

Relata os [testes de disponibilidade na Web](app-insights-monitor-web-app-availability.md).

|Caminho|Tipo|Observações|
|---|---|---|
| availability [0] availabilityMetric.name | string | disponibilidade |
| availability [0] availabilityMetric.value | número |1\.0 ou 0.0 |
| availability [0] count | inteiro | |
| availability [0] dataSizeMetric.name | string | |
| availability [0] dataSizeMetric.value | inteiro | |
| availability [0] durationMetric.name | string | |
| availability [0] durationMetric.value | número | Duração do teste. 1e7==1s |
| availability [0] message | string | Diagnóstico de falha |
| availability [0] result | string | Aprovado/Reprovado |
| availability [0] runLocation | string | Fonte geográfica de solicitações http |
| availability [0] testName | string | |
| availability [0] testRunId | string | |
| availability [0] testTimestamp | string | |




## Métricas

Gerado por TrackMetric().

A métrica é encontrada em context.custom.metrics[0]

Por exemplo:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## Sobre valores de métricas

Valores de métricas, tanto em relatórios de métrica quanto em outros locais, são relatados com uma estrutura de objeto padrão. Por exemplo:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Atualmente, embora isso possa mudar no futuro, em todos os valores relatados pelos módulos padrão do SDK, `count==1` e apenas os campos `name` e `value` são úteis. O único caso em que eles poderiam ser diferentes seria se você escrevesse suas próprias chamadas TrackMetric, nas quais definiria os outros parâmetros.

A finalidade dos outros campos é permitir que as métricas sejam agregadas ao SDK, a fim de reduzir o tráfego no portal. Por exemplo, você pode realizar várias leituras sucessivas antes de enviar cada relatório de métricas. Depois, você deve calcular o desvio mín., máx. e padrão e o valor agregado (soma ou média), e definir a contagem como o número de leituras representado pelo relatório.

Nas tabelas acima, omitimos os campos min, max, stdDev e sampledValue, que são usados raramente.

Em vez de agregar previamente as métricas, você pode usar a [amostragem](app-insights-sampling.md) se precisar reduzir o volume de telemetria.


### Durações

Exceto quando indicado o contrário, as durações são representadas em décimos de microssegundo, de modo que 10000000.0 significa 1 segundo.



## Consulte também

* [Application Insights](app-insights-overview.md) 
* [Exportação Contínua](app-insights-export-telemetry.md)
* [Exemplos de código](app-insights-export-telemetry.md#code-samples)

<!---HONumber=AcomDC_0323_2016-->