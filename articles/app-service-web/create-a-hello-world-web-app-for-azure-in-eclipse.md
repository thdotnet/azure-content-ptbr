<properties 
	pageTitle="Criar uma aplicativo Web Hello World para o Azure no Eclipse" 
	description="Este tutorial mostra como usar o Kit de Ferramentas do Azure para Eclipse para criar um aplicativo Web Hello World para o Azure." 
	services="app-service\web" 
	documentationCenter="java" 
	authors="rmcmurray" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service-web" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="Java" 
	ms.topic="article" 
	ms.date="03/07/2016" 
	ms.author="robmcm"/>

# Criar uma aplicativo Web Hello World para o Azure no Eclipse

Este tutorial mostra como criar e implantar um aplicativo Hello World básico para o Azure como um aplicativo Web usando o [Kit de Ferramentas do Azure para Eclipse]. Um exemplo básico de JSP é exibido para manter a simplicidade, mas etapas muito semelhantes podem ser apropriadas para um servlet Java quando o assunto é a implantação do Azure.

Após a conclusão deste tutorial, seu aplicativo será semelhante à ilustração a seguir quando exibido em um navegador da Web:

![][01]
 
## Pré-requisitos

* Um JDK (Java Developer Kit) versão 1.7 ou posterior.
* Um IDE do Eclipse para desenvolvedores do Java EE, Indigo ou posterior. Isso pode ser baixado em <http://www.eclipse.org/downloads/>.
* Uma distribuição de um servidor Web ou de um servidor de aplicativo baseado em Java, como o Apache Tomcat ou o Jetty.
* Uma assinatura do Azure, que pode ser adquirida de <https://azure.microsoft.com/pt-BR/free/> ou de <http://azure.microsoft.com/pricing/purchase-options/>.
* O Kit de Ferramentas do Azure para Eclipse. Para saber mais, confira [Instalação do Kit de Ferramentas do Azure para Eclipse].

## Para criar um aplicativo Hello World

Primeiro, vamos começar com a criação de um projeto Java.

1. Inicie o Eclipse, no menu clique em **Arquivo**, clique em **Novo** e depois em **Projeto Web Dinâmico**. (Se você não vir o **Projeto Web Dinâmico** listado como um projeto disponível depois de clicar em **Arquivo** e em **Novo**, faça o seguinte: clique em **Arquivo**, clique em **Novo**, clique em **Projeto...**, expanda **Web**, clique em **Projeto Web Dinâmico** e clique em **Avançar**).
1. Para o objetivo deste tutorial, nomeie o projeto **MyHelloWorld**. Sua tela será semelhante à seguinte:
   ![][02]
1. Clique em **Concluir**.
1. No modo de exibição do Gerenciador de Projeto do Eclipse, expanda **MyHelloWorld**. Clique com o botão direito do mouse em **WebContent**, clique em **Novo** e, em seguida, clique em **Arquivo JSP**.
1. Na caixa de diálogo **Novo arquivo JSP**, nomeie o arquivo **index.jsp**. Mantenha a pasta pai como **MyHelloWorld/WebContent**.
1. Na caixa de diálogo **Selecionar Modelo JSP**, para a finalidade deste tutorial, escolha **Novo Arquivo JSP (html)** e clique em **Concluir**.
1. Quando o arquivo index.jsp for aberto no Eclipse, adicione o texto para exibir dinamicamente **Hello World!** dentro do elemento `<body>` existente. Seu conteúdo do `<body>` atualizado deve ser parecido com o exemplo a seguir:
   `<body><b><% out.println("Hello World!"); %></b></body>` 
1. Salve o index.jsp.

## Para implantar seu aplicativo em um contêiner de aplicativos Web do Azure

Há várias maneiras pelas quais você pode implantar um aplicativo Web Java no Azure. Este tutorial descreve uma das mais simples: o aplicativo será implantado em um contêiner de aplicativos Web do Azure, e não há a necessidade de qualquer tipo de projeto especial nem de ferramentas adicionais. O JDK e o software do contêiner da Web serão fornecidos a você pelo Azure, portanto, não é necessário carregar seu próprio; tudo o que você precisa é de seu aplicativo Web Java. Como resultado, o processo de publicação de seu aplicativo demorará segundos, e não minutos.

1. No Gerenciador de Projetos do Eclipse, clique com o botão direito do mouse em **MyHelloWorld**.

1. No menu de contexto, escolha **Azure** e clique em **Publicar como Aplicativo Web do Azure...**
   ![][03]
1. Se não ainda não tiver entrado no Azure pelo Eclipse, você receberá uma solicitação para entrar em sua conta do Azure:
   ![][04]
   Observação: se você tiver várias contas do Azure, alguns avisos durante o processo de entrada poderão ser enviados mais de uma vez, mesmo que pareçam ser o mesmo aviso. Quando isso acontecer, continue seguindo as instruções de entrada.
1. Após o logon bem-sucedido em sua conta do Azure, a caixa de diálogo **Gerenciar Assinaturas** exibirá uma lista de assinaturas associadas às suas credenciais. Se houver várias assinaturas listadas e se você quiser trabalhar com apenas um subconjunto específico delas, poderá, opcionalmente, desmarcar as que deseja usar. Depois de selecionar suas assinaturas, clique em **Fechar**.
   ![][05]
1. Quando a caixa de diálogo **Implantar no Contêiner de Aplicativos Web do Azure** aparecer, ela exibirá qualquer contêiner de aplicativos Web criado anteriormente por você; se você não tiver criado qualquer contêiner, a lista estará vazia.   
   ![][06]
1. Se você nunca tiver criado um contêiner de aplicativos Web do Azure, ou se quiser publicar seu aplicativo em um novo contêiner, use as etapas a seguir. Caso contrário, selecione um Contêiner de Aplicativo Web existente e pule para a etapa 7 abaixo.

  1. Clique em **Novo...**

  1. A caixa de diálogo **Novo Contêiner de Aplicativo Web** será exibida:

        ![][07]

  1. Insira um **Rótulo DNS** para seu Contêiner de Aplicativos Web; isso formará a folha de rótulo DNS da URL do host de seu aplicativo Web no Azure. Observação: o nome deve estar disponível e de acordo com os requisitos de nomenclatura de aplicativo Web do Azure.

  1. No menu suspenso **Contêiner da Web**, selecione o software apropriado ao seu aplicativo.

        No momento, você pode escolher entre o Tomcat 8, Tomcat 7 ou Jetty 9. Uma distribuição recente do software selecionado será fornecida pelo Azure e ele será executado em uma distribuição recente do JDK 8 criado pela Oracle e fornecido pelo Azure.

  1. No menu suspenso **Assinatura**, selecione a assinatura que você deseja usar para essa implantação.

  1. No menu suspenso **Grupo de Recursos**, selecione o Grupo de Recursos com o qual você deseja associar seu Aplicativo Web.

        Observação: os Grupos de Recursos do Azure lhe permitem agrupar recursos relacionados para que, por exemplo, possam ser excluídos juntos.

        Você pode selecionar um Grupo de Recursos existente (se houver) e ignorar a etapa g abaixo ou usar as seguintes etapas para criar um novo Grupo de Recursos:

      * Clique em **Novo...**

      * A caixa de diálogo **Novo Grupo de Recursos** será exibida:

            ![][08]

      * Na caixa de texto **Nome**, especifique um nome para seu novo Grupo de Recursos.

      * No menu suspenso **Região**, selecione o local apropriado do data center do Azure para seu Grupo de Recursos.

      * Clique em **OK**.

  1. O menu suspenso **Plano de Serviço de Aplicativo** lista os planos de serviço de aplicativo que estão associados ao Grupo de Recursos que você selecionou.

        Observação: um Plano do Serviço de Aplicativo especifica informações como o local de seu Aplicativo Web, o tipo de preço e o tamanho da instância de computação. Um único Plano do Serviço de Aplicativo pode ser usado para vários Aplicativos Web, por isso, ele é mantido separadamente de uma implantação de Aplicativo Web específica.

        Você pode selecionar um Plano do Serviço de Aplicativo existente (se houver) e ignorar a etapa h abaixo ou usar as seguintes etapas para criar um novo Plano do Serviço de Aplicativo:

      * Clique em **Novo...**

      * A caixa de diálogo **Novo Plano de Serviço de Aplicativo** será exibida:

            ![][09]

      * Na caixa de texto **Nome**, especifique um nome para seu novo Plano de Serviço de Aplicativo.

      * No menu suspenso **Local**, selecione o local apropriado do data center do Azure para o plano.

      * No menu suspenso **Tipo de Preço**, selecione o preço apropriado para o plano. Para fins de teste, você pode escolher **Gratuito**.

      * No menu suspenso **Tamanho da Instância**, selecione o tamanho de instância apropriado ao plano. Para fins de teste, você pode escolher **Pequeno**.

  1. Depois de concluir todas as etapas acima, a caixa de diálogo Novo Contêiner de Aplicativos Web deve ser semelhante à ilustração a seguir:

        ![][10]

  1. Clique em **OK** para concluir a criação de seu novo Contêiner de Aplicativo Web.

        Aguarde alguns segundos para a lista de contêineres de Aplicativo Web ser atualizada. O contêiner do aplicativo Web recém-criado agora deve ser selecionado na lista.

1. Agora você está pronto para concluir a implantação inicial de seu Aplicativo Web no Azure:

    ![][11]

    Clique em **OK** para implantar seu aplicativo Java no contêiner de aplicativos Web selecionado.

    Observação: por padrão, o aplicativo será implantado como um subdiretório do servidor de aplicativos. Se você quiser implantá-lo como o aplicativo raiz, marque a caixa de seleção **Implantar na raiz** antes de clicar em **OK**.

1. Em seguida, você deverá ver o modo de exibição do **Log de Atividades do Azure**, que indicará o status da implantação do seu aplicativo Web.

    ![][12]

    O processo de implantação de seu aplicativo Web no Azure deve demorar apenas alguns segundos para ser concluído. Quando seu aplicativo estiver pronto, você verá um link chamado **Publicado** na coluna **Status**. Ao clicar no link, você será levado à home page do Aplicativo Web implantado.

## Atualização de seu aplicativo Web

A atualização de um aplicativo Web do Azure em execução é um processo rápido e fácil, e você tem duas opções de atualização:

* Você pode atualizar a implantação de um aplicativo Web Java existente.
* Você pode publicar um aplicativo Java adicional no mesmo contêiner de aplicativo Web.

Em ambos os casos, o processo é idêntico e demora apenas alguns segundos:

1. No explorador de projetos do Eclipse, clique com o botão direito do mouse no aplicativo Java que você deseja atualizar ou adicionar a um contêiner de aplicativos Web existente.

2. Quando o menu de contexto aparecer, escolha **Azure** e **Publicar como Aplicativo Web do Azure...**

3. Como você já fez logon anteriormente, verá uma lista com seus contêineres de aplicativo Web existentes. Selecione aquele que você quer publicar, ou no qual deseja publicar seu aplicativo Java novamente, e clique em **OK**.

Alguns segundos depois, o modo de exibição do **Log de Atividades do Azure** mostrará a implantação atualizada como **Publicada** e você será capaz de verificar seu aplicativo atualizado em um navegador da Web.

## Interromper um aplicativo Web existente

Para interromper um contêiner de Aplicativo Web do Azure existente, (incluindo todos os aplicativos Java implantados nele), você pode usar o modo de exibição do **Azure Explorer**.

Se o modo de exibição do **Azure Explorer** ainda não estiver aberto, você poderá abri-lo clicando no menu **Janela** no Eclipse, clicando em **Mostrar Modo de Exibição**, em **Outros...**, em **Azure** e em **Azure Explorer**. Se você não tiver feito logon anteriormente, receberá uma solicitação para fazê-lo.

Quando o modo de exibição do **Azure Explorer** for exibido, execute estas etapas para interromper o Aplicativo Web:

1. Expanda o nó **Azure**.
2. Expanda o nó **Aplicativos Web**. 
3. Clique com botão direito do mouse no Aplicativo Web desejado.
5. Quando o menu de contexto aparecer, clique em **Parar**.
    ![][13]

## Próximas etapas

Para obter mais informações, consulte os links a seguir:

* [Central de desenvolvedores do Java](/develop/java/).
* [Visão geral de Aplicativos Web](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Kit de Ferramentas do Azure para Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Instalação do Kit de Ferramentas do Azure para Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png

<!---HONumber=AcomDC_0309_2016-->

