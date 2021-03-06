<properties
	pageTitle="Saída do Repositório Data Lake do Stream Analytics | Microsoft Azure"
	description="Configuração de autenticação e autorização de um Repositório Azure Data Lake em um trabalho do Stream Analytics"
	keywords=""
	services="stream-analytics"
	documentationCenter=""
	authors="jeffstokes72"
	manager="paulettm"
	editor="cgronlun"
/>

<tags
	ms.service="stream-analytics"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="big-data"
	ms.date="03/18/2016"
	ms.author="jeffstok"
/>

# Saída do Repositório Data Lake do Stream Analytics

Trabalhos do Stream Analytics dão suporte a vários métodos de saída, sendo um deles um [Repositório Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/). O Repositório Azure Data Lake é um repositório em hiper-escala corporativo para cargas de trabalho de análise de big data. O Repositório Data Lake permite que você armazene dados de qualquer tamanho, tipo e velocidade de ingestão para análises operacionais e exploratórias. Este artigo aborda a autorização, configuração e renovação da autorização de um Repositório Azure Data Lake no Portal Clássico do Azure no Stream Analytics.

> [AZURE.NOTE] No momento, há suporte para a criação e configuração das saídas do Repositório Data Lake **apenas** no Portal Clássico do Azure.

## Autorizar uma conta do Repositório Data Lake

1.  Quando o Repositório Data Lake é selecionado como uma saída no portal de Gerenciamento do Azure, é solicitado que você autorize o uso de seu Repositório Data Lake existente ou solicite acesso à Preview do Repositório Data Lake por meio do Portal Clássico do Azure.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.jpg)

2.  Se você já tiver acesso ao Repositório Data Lake, clique em "Autorizar agora" e, por um curto período, uma página será exibida indicando "Redirecionando para autorização...". A página será fechada automaticamente e você verá a página que permite configurar a saída do Repositório Data Lake.

Se não tiver feito a inscrição para a Preview do Repositório Data Lake, você pode seguir o link "Inscrever-se agora" para iniciar a solicitação ou executar as [instruções de início](../data-lake-store/data-lake-store-get-started-portal.md).

## Configurar as propriedades de saída do Repositório Data Lake

Uma vez que a conta do Repositório Data Lake foi autenticada, você pode configurar as propriedades de saída do Repositório Data Lake. A tabela a seguir é a lista de nomes de propriedade e sua descrição para configurar a saída do Repositório Data Lake.

<table>
<tbody>
<tr>
<td><B>NOME DA PROPRIEDADE</B></td>
<td><B>DESCRIÇÃO</B></td>
</tr>
<tr>
<td>Alias de saída</td>
<td>Esse é um nome amigável utilizado em consultas para direcionar a saída da consulta para esse Repositório Data Lake.</td>
</tr>
<tr>
<td>Conta do Repositório Data Lake</td>
<td>O nome da conta de armazenamento para o qual você está enviando a saída Você verá uma lista suspensa de contas do Repositório Data Lake às quais o usuário conectado ao portal tem acesso.</td>
</tr>
<tr>
<td>Padrão de prefixo do caminho [<I>opcional</I>]</td>
<td>O caminho do arquivo usado para gravar seus arquivos na Conta do Repositório Data Lake especificada. <BR>{data}, {hora}<BR>Exemplo 1: pasta1/logs/{data}/{hora}<BR>Exemplo 2: pasta1/logs/{data}</td>
</tr>
<tr>
<td>Formato de data [<I>opcional</I>]</td>
<td>Se o token de data for usado no caminho do prefixo, você pode selecionar o formato de data na qual os arquivos são organizados. Exemplo: AAAA/MM/DD</td>
</tr>
<tr>
<td>Formato de hora [<I>opcional</I>]</td>
<td>Se o token de hora for usado no caminho do prefixo, você pode selecionar o formato de hora na qual os arquivos são organizados. Atualmente, o único valor aceito é HH.</td>
</tr>
<tr>
<td>Formato de serialização do evento</td>
<td>Formato de serialização para dados de saída. Há suporte para JSON, CSV e Avro.</td>
</tr>
<tr>
<td>Codificação</td>
<td>Se o formato for CSV ou JSON, uma codificação deve ser especificada. UTF-8 é o único formato de codificação com suporte no momento.</td>
</tr>
<tr>
<td>Delimitador</td>
<td>Aplicável somente à serialização de CSV. O Stream Analytics é compatível com vários delimitadores comuns para serialização de dados CSV. Os valores suportados são vírgula, ponto e vírgula, espaço, tab e barra vertical.</td>
</tr>
<tr>
<td>Formatar</td>
<td>Aplicável somente para serialização JSON. Uma linha separada especifica que a saída será formatada com cada objeto JSON separado por uma nova linha. Matriz especifica que a saída será formatada como uma matriz de objetos JSON.</td>
</tr>
</tbody>
</table>

## Renovar autorização do Repositório Data Lake

Atualmente, há uma limitação em que o token de autenticação deve ser atualizado manualmente a cada 90 dias para todos os trabalhos com saída do Repositório Data Lake. Você também precisará autenticar novamente sua conta do Repositório Data Lake caso sua senha tenha sido alterada depois de seu trabalho ter sido criado ou autenticado pela última vez. Um sintoma desse problema é nenhuma saída de trabalho e um erro nos Logs de Operação indicando a necessidade de uma nova autorização.

Para resolver esse problema, pare seu trabalho em execução e vá para a saída do Repositório Data Lake. Clique no link "Renovar autorização" e por um curto período uma página será exibida indicando "Redirecionando para autorização...". A página será fechada automaticamente e, se for bem-sucedida, indicará "A autorização foi renovada com êxito". Em seguida, você precisa clicar em "Salvar" na parte inferior da página e poderá continuar reiniciando seu trabalho da última vez em que foi interrompido para evitar perda de dados.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.jpg)

<!---HONumber=AcomDC_0323_2016-->