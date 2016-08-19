<properties
	pageTitle="Visão geral do SDK para Web do Azure Mobile Engagement | Microsoft Azure"
	description="As atualizações e procedimentos mais recentes do SDK para Web do Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="web"
	ms.devlang="js"
	ms.topic="article"
	ms.date="06/07/2016"
	ms.author="piyushjo" />


# SDK Web do Azure Mobile Engagement

Comece aqui para obter todos os detalhes sobre como integrar o Azure Mobile Engagement em um aplicativo Web. Se você gostaria de experimentá-lo antes de começar com seu próprio aplicativo Web, confira nosso [tutorial de 15 minutos](mobile-engagement-web-app-get-started.md).

## Procedimentos de integração
1. Saiba [Como integrar o Mobile Engagement ao seu aplicativo Web](mobile-engagement-web-integrate-engagement.md).

2. Para a implementação do plano de marcação, saiba [Como usar a API de marcação avançada do Mobile Engagement em seu aplicativo Web](mobile-engagement-web-use-engagement-api.md).

## Notas de versão

### 2\.0.1 (10/6/2016)

-   Desabilitado o SDK Web do Mobile Engagement no Internet Explorer 8 e no Internet Explorer 9.
-   Detecção corrigida do navegador da Web Opera.

Para obter todas as versões, veja as [notas de versão completas](mobile-engagement-web-release-notes.md).

## Procedimentos de atualização

### Atualizar da 1.2.1 para a 2.0.0

As seções a seguir descrevem como migrar uma integração do SDK para Web do Mobile Engagement do serviço Capptain, oferecido pelo Capptain SAS, para um aplicativo do Azure Mobile Engagement. Se você estiver migrando de uma versão anterior à 1.2.1, consulte o site do Capptain para migrar primeiro para 1.2.1 e depois aplicar o procedimento a seguir.

Essa versão do SDK para Web do Mobile Engagement não dá suporte para Samsung Smart TV, Opera TV, webOS ou com o recurso Reach.

>[AZURE.IMPORTANT] O Capptain e o Azure Mobile Engagement não são o mesmo serviço e os procedimentos a seguir destacam apenas como migrar o aplicativo cliente. Migrar o SDK para Web do Mobile Engagement no aplicativo não migrará os dados de um servidor do Capptain para um servidor do Mobile Engagement.

#### Arquivos JavaScript

Substitua o arquivo capptain-sdk.js pelo arquivo azure-engagement.js e atualize suas importações de script adequadamente.

#### Remover o Capptain Reach

Essa versão do SDK para Web do Mobile Engagement não dá suporte ao recurso Reach. Se você integrou o Capptain Reach ao aplicativo, será necessário removê-lo.

Remova a importação de CSS do Reach da sua página e exclua o arquivo .css relacionado (capptain-reach.css, por padrão).

Exclua os seguintes recursos do Reach: a imagem de fechamento (capptain-close.png, por padrão) e o ícone da marca (capptain-notification-icon, por padrão).

Remova a interface do usuário do Reach para notificações no aplicativo. O layout padrão tem esta aparência:

	<!-- capptain notification -->
	<div id="capptain_notification_area" class="capptain_category_default">
	  <div class="icon">
	    <img src="capptain-notification-icon.png" alt="icon" />
	  </div>
	  <div class="content">
	    <div class="title" id="capptain_notification_title"></div>
	    <div class="message" id="capptain_notification_message"></div>
	  </div>
	  <div id="capptain_notification_image"></div>
	  <div>
	    <button id="capptain_notification_close">Close</button>
	  </div>
	</div>

Remova a interface do usuário do Reach para anúncios da Web e pesquisas. O layout padrão tem esta aparência:

	<div id="capptain_overlay" class="capptain_category_default">
	  <button id="capptain_overlay_close">x</button>
	  <div id="capptain_overlay_title"></div>
	  <div id="capptain_overlay_body"></div>
	  <div id="capptain_overlay_poll"></div>
	  <div id="capptain_overlay_buttons">
	    <button id="capptain_overlay_exit"></button>
	    <button id="capptain_overlay_action"></button>
	  </div>
	</div>

Remova o objeto `reach` da sua configuração, se ele existir. Ele tem esta aparência:

	window.capptain = {
	  [...]
	  reach: {
	    [...]
	  }
	}

Remova qualquer outra personalização do Reach, como categorias.

#### Remover APIs substituídas

Algumas das APIs do Capptain foram preteridas na versão do SDK para Web do Mobile Engagement.

Remova todas as chamadas para as seguintes APIs: `agent.connect`, `agent.disconnect`, `agent.pause` e `agent.sendMessageToDevice`.

Remova todos os seguintes retornos de chamada da sua configuração do Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived` e `onPushMessageReceived`.

#### Configuração

O Mobile Engagement usa uma cadeia de conexão para configurar os identificadores do SDK, por exemplo, o identificador do aplicativo.

Substitua a ID do aplicativo pela cadeia de conexão. Observe que o objeto global para a configuração do SDK muda de `capptain` para `azureEngagement`.

Antes da migração:

	window.capptain = {
	  appId: ...,
	  [...]
	};

Após a migração:

	window.azureEngagement = {
	  connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
	  [...]
	};

A cadeia de conexão para o seu aplicativo é exibida no portal do Azure.

#### APIs do JavaScript

O objeto `window.capptain` global do JavaScript foi renomeado para `window.azureEngagement`, mas você pode usar o alias `window.engagement` para chamadas à API. Você não pode usar o alias para definir a configuração do SDK.

Por exemplo: `capptain.deviceId` se torna `engagement.deviceId`, `capptain.agent.startActivity` se torna `engagement.agent.startActivity` e assim por diante.

Se você já tiver integrado uma versão anterior do SDK Web do Azure Mobile Engagement ao seu aplicativo, leia sobre [procedimentos de atualização](mobile-engagement-web-upgrade-procedure.md).

<!---HONumber=AcomDC_0713_2016-->