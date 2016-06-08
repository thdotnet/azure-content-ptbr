<properties
   pageTitle="Exportar dados do Log Analytics para o Power BI | Microsoft Azure"
   description="O Power BI é um serviço de análise de negócios baseado em nuvem da Microsoft que fornece relatórios e visualizações avançadas para análise de diferentes conjuntos de dados. O Log Analytics pode realizar exportação contínua de dados do repositório do OMS para o Power BI automaticamente para que você possa aproveitar suas visualizações e ferramentas de análise. Este artigo descreve como configurar consultas no Log Analytics exportados automaticamente para o Power BI em intervalos regulares."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="bwren" />

# Exportar dados do Log Analytics para o Power BI

O [Power BI](https://powerbi.microsoft.com/pt-BR/documentation/powerbi-service-get-started/) é um serviço de análise de negócios baseado em nuvem da Microsoft que fornece relatórios e visualizações avançadas para análise de diferentes conjuntos de dados. O Log Analytics pode exportar dados do repositório do OMS para o Power BI automaticamente para que você possa aproveitar suas visualizações e ferramentas de análise.

Ao configurar o Power BI com o Log Analytics, você poderá criar consultas de log que exportam seus resultados para conjuntos de dados correspondentes no Power BI. A consulta e a exportação continuarão a ser executadas automaticamente em uma agenda definida por você para manter o conjunto de dados atualizado com os últimos dados coletados pelo Log Analytics.

![Log Analytics para Power BI](media/log-analytics-powerbi/overview.png)

## Agendas do Power BI

Uma *Agenda do Power BI* inclui uma pesquisa de log que exporta um conjunto de dados do repositório do OMS para um conjunto de dados correspondente no Power BI e uma agenda que define a frequência com que essa pesquisa é executada para manter o conjunto de dados atualizado.

Os campos no conjunto de dados corresponderão às propriedades dos registros retornados pela pesquisa de log. Se a pesquisa retornar registros de diferentes tipos, o conjunto de dados incluirá todas as propriedades de cada um dos tipos de registro incluídos.

> [AZURE.NOTE] É uma melhor prática usar uma consulta de pesquisa de log que retorna dados brutos em vez de executar qualquer consolidação usando comandos como [Measure](log-analytics-search-reference.md#measure). Você pode executar qualquer agregação e cálculo no Power BI por meio dos dados brutos.

## Conectar o espaço de trabalho do OMS ao Power BI

Antes de exportar do Log Analytics para o Power BI, você deve conectar o seu espaço de trabalho do OMS à conta do Power BI usando o procedimento a seguir.

1. No console do OMS, clique no bloco **Configurações**.
2. Selecione **Contas**.
3. Na seção **Informações sobre o Espaço de Trabalho**, clique em **Conectar à Conta do Power BI**.
4. Insira as credenciais da sua conta do Power BI.

## Criar uma agenda do Power BI

Crie uma agenda do Power BI para cada conjunto de dados usando o procedimento a seguir.

1. No console do OMS, clique no bloco **Pesquisa de Log**.
2. Digite uma nova consulta ou selecione uma pesquisa salva que retorna os dados que você deseja exportar para o **Power BI**.  
3. Clique no botão **Power BI** na parte superior da página para abrir o diálogo **Power BI**.
4. Forneça as informações na tabela a seguir e clique em **Salvar**.

| Propriedade | Descrição |
|:--|:--|
| Nome | Nome para identificar a agenda ao exibir a lista de agendamentos do Power BI. |
| Pesquisa Salva | A pesquisa de log a ser executada. Você pode selecionar a consulta atual ou uma pesquisa salva existente na lista suspensa. |
| Agenda | A frequência de execução da pesquisa salva e exportação para o conjunto de dados do Power BI. O valor deve ser entre 15 minutos e 24 horas. |
| Nome do conjunto de dados | O nome do conjunto de dados no Power BI. Ele será criado se ele não existir e atualizado se existir. |

## Exibir e remover agendas do Power BI

Veja a lista de agendamentos do Power BI existentes com o procedimento a seguir.

1. No console do OMS, clique no bloco **Configurações**.
2. Selecione **Power BI**.

Além dos detalhes da agenda, são exibidos o número de vezes que a agenda foi executada na última semana e o status da última sincronização. Se a sincronização encontrou erros, você pode clicar no link para executar uma pesquisa de log de registros com detalhes do erro.

É possível remover um agendamento clicando no **X** na **coluna Remover**. É possível desabilitar um agendamento selecionando **Desativado**. Para modificar uma agenda, você deve removê-la e recriá-la com as novas configurações.

![Agendas do Power BI](media/log-analytics-powerbi/schedules.png)

## Passo a passo de exemplo
A seção a seguir demonstra o passo a passo de um exemplo de como criar uma agenda do Power BI e usar seu conjunto de dados para criar um relatório simples. Neste exemplo, todos os dados de desempenho para um conjunto de computadores são exportado para o Power BI e um gráfico de linha é criado para exibir a utilização do processador.

### Criar pesquisa de log
Começamos criando uma pesquisa de log para os dados que desejamos enviar para o conjunto de dados. Neste exemplo, usaremos uma consulta que retorna todos os dados de desempenho de computadores com um nome que começa com *srv*.

![Agendas do Power BI](media/log-analytics-powerbi/walkthrough-query.png)

### Criar a pesquisa do Power BI
Clicamos no botão **Power BI** para abrir o diálogo Power BI e fornecemos as informações necessárias. Queremos que essa pesquisa seja executada uma vez por hora e crie um conjunto de dados chamado *Contoso Perf*. Como já temos aberta uma pesquisa que cria os dados que queremos, mantemos a opção padrão *Usar consulta de pesquisa atual* para **Pesquisa Salva**.

![Pesquisa do Power BI](media/log-analytics-powerbi/walkthrough-schedule.png)

### Verifique a pesquisa do Power BI
Para verificar se criamos o agendamento corretamente, exibimos a lista de Pesquisas do Power BI sob o bloco **Configurações** no painel do OMS. Aguarde alguns minutos e atualize essa exibição até que ela informe que a sincronização foi executada.

![Pesquisa do Power BI](media/log-analytics-powerbi/walkthrough-schedules.png)

### Verificar o conjunto de dados no Power BI
Fazemos logon em nossa conta em [powerbi.microsoft.com](http://powerbi.microsoft.com/) e rolamos até **Conjuntos de Dados** na parte inferior do painel esquerdo. Podemos observar que o conjunto de dados *Contoso Perf* é listado, indicando que nossa exportação foi executada com êxito.

![Conjunto de dados do Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

### Criar um relatório com base no conjunto de dados
Selecionamos o conjunto de dados **Contoso Perf** e clicamos em **Resultados** no painel **Campos** à direita para exibir os campos que fazem parte desse conjunto de dados. Para criar um gráfico de linha mostrando a utilização do processador para cada computador, executamos as seguintes ações.

1. Selecione a visualização de Gráfico de linha.
2. Arraste **ObjectName** para **Filtro no nível de relatório** e marque **Processor**.
3. Arraste **CounterName** para **Filtro no nível de relatório** e marque **% Processor Time**.
4. Arraste **CounterValue** para **Valores**.
5. Arraste **Computer** para **Legenda**.
6. Arraste **TimeGenerated** para **Eixo**.

Podemos ver que o gráfico de linhas resultante é exibido com os dados do nosso conjunto de dados.

![Gráfico de linha do Power BI](media/log-analytics-powerbi/walkthrough-linegraph.png)

### Salvar o relatório
Salvamos o relatório clicando no botão Salvar na parte superior da tela e validamos se agora ele está listado na seção Relatórios no painel esquerdo.

![Relatórios do Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## Próximas etapas

- Saiba mais sobre [pesquisas de log](log-analytics-log-searches.md) para criar consultas que podem ser exportadas para o Power BI.
- Saiba mais sobre o [Power BI](powerbi.microsoft.com) para criar visualizações baseadas em exportações do Log Analytics.

<!---HONumber=AcomDC_0518_2016-->