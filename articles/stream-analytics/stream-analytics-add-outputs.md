<properties 
	pageTitle="Como configurar saídas de dados para trabalhos do Stream Analytics | Microsoft Azure" 
	description="Configurar saídas para trabalhos do Stream Analytics | segmento do roteiro de aprendizagem."
	keywords="dados de saída, movimentação de dados"
	documentationCenter=""
	services="stream-analytics"
	authors="jeffstokes72" 
	manager="paulettm" 
	editor="cgronlun"/>

<tags 
	ms.service="stream-analytics" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="data-services" 
	ms.date="03/18/2016" 
	ms.author="jeffstok"/>

# Como configurar saídas de dados para trabalhos do Stream Analytics

Os trabalhos do Stream Analytics do Azure podem ser conectados a uma ou mais saídas de dados, o que define uma conexão com um coletor de dados existente. Como o trabalho do Stream Analytics processa e transforma dados de entrada, um fluxo de eventos de saída de dados são gravados na saída do trabalho.

As saídas de dados do Stream Analytics podem ser usadas para dar origem a painéis ou alertas em tempo real, disparar fluxos de trabalho movimentação de dados ou simplesmente arquivar dados para processamento em lote posteriormente. O Stream Analytics integra-se perfeitamente a vários serviços do Azure, que são documentados em detalhes aqui.

Para adicionar uma saída ao trabalho do Stream Analytics:

1. No portal clássico do Azure, clique em **Saídas** e, em seguida, clique em **Adicionar saída** no trabalho do Stream Analytics.

    ![Adicionar saídas](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)

    No portal do Azure, clique no bloco **Saídas** no seu trabalho do Stream Analytics.

    ![Adicionar Saídas no Portal do Azure](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Especifique o tipo de saída:

    ![Escolher o tipo de movimentação de dados](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)

    ![Escolher tipo de movimentação de dados no Portal do Azure](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Forneça um nome amigável para essa saída na caixa **Alias de saída**. Esse nome pode ser usado na consulta do seu trabalho posteriormente para fazer referência à saída.
    
    Preencha o restante das propriedades de conexão necessárias para se conectar à saída. Esses campos variam de acordo com o tipo de saída e são definidos em detalhes aqui.

    ![Adicionar propriedades de saída de dados](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)

4. Dependendo do tipo de saída, talvez seja necessário especificar como os dados são serializados ou formatados. As configurações específicas de serialização para cada tipo de saída estão documentadas aqui.

    Preencha o restante das propriedades de conexão necessárias para se conectar à fonte de dados. Esses campos variam de acordo com o tipo de entrada e de fonte e são definidos detalhadamente [aqui](stream-analytics-create-a-job.md).

    ![Adicionar saída de dados ao hub de eventos](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)

    ![Saída de dados para hub de eventos no Portal do Azure](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)

> [Azure.Note] Qualquer elemento de saída adicionado ao trabalho deve existir antes de o trabalho ser iniciado e de os eventos começarem a fluir. Por exemplo, se você usar o Armazenamento de Blobs como uma saída, o trabalho não criará uma conta de armazenamento automaticamente. Ele precisa ser criado pelo usuário antes de o trabalho ASA ser iniciado.

## Obter ajuda
Para obter mais assistência, experimente nosso [Fórum do Stream Analytics do Azure](https://social.msdn.microsoft.com/Forums/pt-BR/home?forum=AzureStreamAnalytics)

## Próximas etapas

- [Introdução ao Stream Analytics do Azure](stream-analytics-introduction.md)
- [Introdução ao uso do Stream Analytics do Azure](stream-analytics-get-started.md)
- [Dimensionar trabalhos do Stream Analytics do Azure](stream-analytics-scale-jobs.md)
- [Referência de Linguagem de Consulta do Stream Analytics do Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referência da API REST do Gerenciamento do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!---HONumber=AcomDC_0323_2016-->