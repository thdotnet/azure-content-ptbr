<properties
	pageTitle="Lista de APIs gerenciadas pela Microsoft | Serviço de Aplicativo do Microsoft Azure"
	description="Obter uma lista completa das APIs Gerenciadas pela Microsoft que você pode usar para compilar Aplicativos Lógicos no Serviço de Aplicativo do Azure"
	services="app-service\logic"
	documentationCenter=""
	authors="MSFTMAN"
	manager="erikre"
	editor=""
    tags="connectors"/>

<tags
	ms.service="app-service-logic"
	ms.workload="integration"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="03/28/2016"
	ms.author="deonhe"/>

# Lista de APIs gerenciadas

>[AZURE.NOTE] Esta versão do artigo aplica-se à versão do esquema 2015-08-01-preview de aplicativos lógicos. Para obter a versão do esquema 2014-12-01-preview, clique em [Lista de Conectores](../app-service-logic/app-service-logic-connectors-list.md).

Para obter informações de preço e uma lista do que é incluído com cada camada de serviço, consulte [Preços do Serviço de Aplicativo do Azure](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Se você quiser começar a usar os Aplicativos Lógicos do Azure antes de se inscrever em uma conta do Azure, acesse [Experimentar o aplicativo lógico](https://tryappservice.azure.com/?appservice=logic). Você pode criar instantaneamente um aplicativo lógico de início e de curta duração no Serviço de Aplicativo. Nenhum cartão de crédito é exigido, sem compromissos.

Selecione um ícone para saber como aproveitar rapidamente essas APIs para compilar aplicativos que chamam esses serviços. Essas APIs podem ser usadas para compilar aplicativos lógicos e/ou PowerApps ou em ambos.

|APIs||||
|-----------|-----------|-----------|-----------|
|[![Ícone de API][blobicon]<br/>**Blob do Azure**][azureblobdoc]|[![Ícone de API][bingsearchicon]<br/>**Pesquisa do Bing**][bingsearchdoc]|[![Ícone de API][boxicon]<br/>**Caixa**][boxDoc]|[![Ícone de API][crmonlineicon]<br/>**CRM Online**][crmonlinedoc]|
|[![Ícone de API][dropboxicon]<br/>**Dropbox**][dropboxdoc]|[![Ícone de API][facebookicon]<br/>**Facebook**][facebookdoc]|[![Ícone de API][ftpicon]<br/>**FTP**][ftpdoc]|[![Ícone de API][googledriveicon]<br/>**Google Drive**][googledrivedoc]|
|[![Ícone de API][microsofttranslatoricon]<br/>**Tradutor**][microsofttranslatordoc]|[![Ícone de API][office365icon]<br/>**Office 365**<br/>**Outlook**][office365outlookdoc]|[![Ícone de API][office365icon]<br/>**Office 365**<br/>**Usuários**][office365usersdoc]|[![Ícone de API][office365icon]<br/>**Office 365**<br/>**Vídeo**][office365videodoc]|
|[![Ícone de API][onedriveicon]<br/>**OneDrive**][onedrivedoc]|[![Ícone de API][salesforceicon]<br/>**Salesforce**][salesforcedoc]|[![Ícone de API][servicebusicon]<br/>**Barramento de Serviço**][servicebusdoc]|[![Ícone de API][sftpicon]<br/>**SFTP**][sftpdoc]|
|[![Ícone de API][sharepointicon]<br/>**SharePoint**<br/>**Online**][sharepointdoc]|[![Ícone de API][slackicon]<br/>**Margem de atraso**<br/>][slackdoc]|[![Ícone de API][smtpicon]<br/>**SMTP**][smtpdoc]|[![Ícone de API][sqlicon]<br/>**SQL Azure**][sqldoc]|
|[![Ícone de API][twilioicon]<br/>**Twilio**][twiliodoc]|[![Ícone de API][twittericon]<br/>**Twitter**][twitterdoc]|[![Ícone de API][yammericon]<br/>**Yammer**][yammerdoc] | |

> [AZURE.NOTE] Se você já tiver criado aplicativos lógicos usando o esquema 2014-12-01-preview, perceberá que as APIs de integração empresarial como aquelas para o BizTalk não estão listadas acima. Sabemos que elas são importantes e estamos trabalhando duro para mostrá-las a você em breve. Enquanto não temos uma data exata de disponibilidade para compartilhar, lembre-se de que essa é uma das nossas principais prioridades. Enquanto isso, você pode acessar suas [APIs v1 e APIs do BizTalk APIs dos Aplicativos Lógicos](https://blogs.msdn.microsoft.com/logicapps/2016/02/25/accessing-v1-apis-and-biztalk-apis-from-logic-apps/). Obrigado por sua compreensão. Fique atento.


### APIs podem ser gatilhos
Várias APIs fornecem gatilhos que podem notificar seu aplicativo quando eventos específicos ocorrem. Por exemplo, a API de FTP tem o gatilho OnUpdatedFile. É possível compilar um aplicativo lógico ou um PowerApp que escuta esse gatilho e executa uma ação sempre que ele é acionado.

Há dois tipos de gatilhos:

* Gatilhos de Pesquisa: esses gatilhos pesquisam seu serviço de interesse a uma frequência especificada para verificar se há novos dados. Quando novos dados estão disponíveis, uma nova instância do seu aplicativo é executada com os dados como entrada. O gatilho pode limpar os dados já lidos e transmitidos para seu aplicativo para impedir que os mesmos dados sejam consumidos várias vezes.
* Gatilhos Push: esses gatilhos escutam em busca de dados em um ponto de extremidade ou da ocorrência de eventos. Em seguida, ele dispara uma nova instância do seu aplicativo. A API do twitter é um exemplo.


### APIs podem ser ações
APIs também podem ser usadas como ações dentro dos seus aplicativos. Ações são úteis para pesquisar dados no que podem então ser usados na execução do seu aplicativo. Por exemplo, talvez seja necessário pesquisar dados de um cliente de um banco de dados SQL durante o processamento de um pedido. Ou então, talvez seja necessário escrever, atualizar ou excluir dados em uma tabela de destino. Você pode fazer isso usando as ações fornecidas pelas APIs. As ações são mapeadas para as operações que são definidas nos metadados do Swagger.


[Novidades](../app-service-logic/app-service-logic-schema-2015-08-01.md) [Compilar um aplicativo lógico agora](../app-service-logic/app-service-logic-create-a-logic-app.md) [Introdução aos PowerApps](../power-apps/powerapps-get-started-azure-portal.md) [Migrar aplicativos lógicos para a versão mais recente do esquema](connectors-schema-migration.md)

<!--API Documentation-->
[azureblobdoc]: ./connectors-create-api-azureblobstorage.md "Conecte-se ao Blob do Azure para gerenciar arquivos no seu contêiner de blob."
[bingsearchDoc]: ./connectors-create-api-bingsearch.md "Pesquisa do Bing para Web, imagens, notícias e vídeo."
[boxDoc]: ./connectors-create-api-box.md "Conecta-se a caixa e pode carregar, obter, excluir, listar e mais tarefas de arquivo."
[crmonlinedoc]: ./connectors-create-api-crmonline.md "Conecte-se ao Dynamics CRM Online e faça mais com os dados do CRM Online."
[dropboxdoc]: ./connectors-create-api-dropbox.md "Conecte-se ao Dropbox e pode obter, excluir, listar e mais tarefas de arquivo."
[exceldoc]: ./connectors-create-api-excel.md "Conecte-se ao Excel."
[facebookdoc]: ./connectors-create-api-facebook.md "Conecte-se ao Facebook para postar em uma linha do tempo, receber um feed de página e muito mais."
[ftpdoc]: ./connectors-create-api-ftp.md "Conecta-se a um servidor FTP / FTPS e executa tarefas FTP diferentes, incluindo carregar, obter, excluir arquivos e muito mais."
[googledrivedoc]: ./connectors-create-api-googledrive.md "Conecte-se ao GoogleDrive e interaja com os dados."
[microsofttranslatordoc]: ./connectors-create-api-microsofttranslator.md
[office365outlookdoc]: ./connectors-create-api-office365-outlook.md "O Conector do Office 365 pode enviar e receber emails, gerenciar seu calendário e gerenciar seus contatos usando sua conta do Office 365."
[officeunifieddoc]: ./connectors-create-api-bingsearch.md
[office365usersdoc]: ./connectors-create-api-office365-users.md
[office365videodoc]: ./connectors-create-api-office365-video.md
[onedrivedoc]: ./connectors-create-api-onedrive.md "Se conecta ao seu Microsoft OneDrive pessoal e carrega, exclui, lista arquivos e muito mais."
[salesforcedoc]: ./connectors-create-api-salesforce.md "Conecte-se à sua conta da equipe de vendas e gerencia contas, clientes potenciais, oportunidades e muito mais."
[servicebusdoc]: ./connectors-create-api-servicebus.md "Pode enviar mensagens de tópicos e filas do Barramento de Serviço e receber mensagens de assinaturas e filas do Barramento de Serviço."
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Conecta-se ao SharePoint Online para gerenciar documentos e itens de lista."
[slackdoc]: ./connectors-create-api-slack.md "Conecte-se à margem de atraso e poste mensagens em canais de margem de atraso."
[sftpdoc]: ./connectors-create-api-sftp.md "Conecta-se ao SFTP e pode carregar, obter, excluir arquivos e muito mais."
[smtpdoc]: ./connectors-create-api-smtp.md "Conecta-se a um servidor SMTP e pode enviar emails com anexos."
[sqldoc]: ./connectors-create-api-sqlazure.md "Conecte-se ao Banco de Dados SQL Azure. Você pode criar, atualizar, obter e excluir entradas em uma tabela de banco de dados SQL."
[twiliodoc]: ./connectors-create-api-twilio.md "Conecta-se a Twilio e pode enviar e receber mensagens, obter os números disponíveis, gerenciar de números de telefone de entrada e muito mais."
[twitterdoc]: ./connectors-create-api-twitter.md "Se conecta ao Twitter e obtém cronogramas, posta tweets e muito mais."
[yammerdoc]: ./connectors-create-api-yammer.md "Conecta-se no Yammer para postar mensagens e obter novas mensagens."

<!--Icon references-->
[blobicon]: ./media/apis-list/blobicon.png
[bingsearchicon]: ./media/apis-list/bingsearchicon.png
[boxicon]: ./media/apis-list/boxicon.png
[ftpicon]: ./media/apis-list/ftpicon.png
[crmonlineicon]: ./media/apis-list/dynamicscrmicon.png
[dropboxicon]: ./media/apis-list/dropboxicon.png
[excelicon]: ./media/apis-list/excelicon.png
[facebookicon]: ./media/apis-list/facebookicon.png
[googledriveicon]: ./media/apis-list/googledriveicon.png
[microsofttranslatoricon]: ./media/apis-list/translatoricon.png
[office365icon]: ./media/apis-list/office365icon.png
[onedriveicon]: ./media/apis-list/onedriveicon.png
[salesforceicon]: ./media/apis-list/salesforceicon.png
[servicebusicon]: ./media/apis-list/servicebusicon.png
[sftpicon]: ./media/apis-list/sftpicon.png
[sharepointicon]: ./media/apis-list/sharepointicon.png
[slackicon]: ./media/apis-list/slackicon.png
[smtpicon]: ./media/apis-list/smtpicon.png
[sqlicon]: ./media/apis-list/sqlicon.png
[twilioicon]: ./media/apis-list/twilioicon.png
[twittericon]: ./media/apis-list/twittericon.png
[yammericon]: ./media/apis-list/yammericon.png

<!---HONumber=AcomDC_0330_2016-->