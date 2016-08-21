<properties
	pageTitle="Referência do desenvolvedor do Azure Functions | Microsoft Azure"
	description="Entenda os conceitos e componentes do Azure Functions que são comuns a todas as linguagens e associações."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="05/13/2016"
	ms.author="chrande"/>

# Referência do desenvolvedor do Azure Functions
Azure Functions compartilham alguns componentes e conceitos técnicos principais, independentemente da linguagem ou binding que você utilizar. Antes de saltar para o aprendizado específico a uma linguagem ou binding, assegure-se de ler esta visão geral que se aplica a todos eles.

Este artigo pressupõe que você já tenha lido a [Visão geral do Azure Functions](functions-overview.md) e está familiarizado com [conceitos do SDK de WebJobs como gatilhos, associações e tempo de execução do JobHost](../app-service-web/websites-dotnet-webjobs-sdk.md). O Azure Functions é baseado no SDK de WebJobs.

## função

Uma *função* é o principal conceito no Azure Functions. Você escreve o código para uma função em uma linguagem de sua escolha e salva os arquivos de código e um arquivo de configuração na mesma pasta. A configuração é em JSON, e o arquivo é nomeado `function.json`. Há suporte para várias linguagens, e cada uma tem uma experiência ligeiramente diferente, otimizada para funcionar melhor para esta linguagem. Exemplo de estrutura de pasta:

```
mynodefunction
| - function.json
| - index.js
| - node_modules
| | - ... packages ...
| - package.json
mycsharpfunction
| - function.json
| - run.csx
```

## function.JSON e bindings

O arquivo `function.json` contém uma configuração específica para uma função, incluindo seu binding. Em tempo de execução esse arquivo é lido para determinar quais eventos disparar, quais dados serão incluídos na chamada da função e para onde enviar os dados passados pela própria função.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Você pode impedir que o tempo de execução execute a função definindo a propriedade `disabled` como `true`.

A propriedade `bindings` é onde você configura gatilhos e associações. Cada binding compartilha algumas configurações comuns e outras que são específicas para um determinado tipo de binding. Todas as associações exigem as seguintes configurações:

|Propriedade|Valores/Tipos|Comentários|
|---|-----|------|
|`type`|string|Tipo de binding. Por exemplo: `queueTrigger`.
|`direction`|'in', 'out'| Indica se a associação é para receber dados na função ou enviar dados a partir da função.
| `name` | string | O nome que será usado para os dados associados na função. Em C#, ele será o nome de um argumento. Em JavaScript, será a chave em uma lista de chave/valor.

## Tempo de execução (host de script e host Web)

O tempo de execução, também conhecido como o host de script, é o host por baixo do SDK de WebJobs que escuta eventos, coleta e envia dados e, no fim das contas, executa seu código.

Para facilitar gatilhos HTTP, há também um host Web que foi desenvolvido para ficar na frente do host de script em cenários de produção. Isso ajuda a isolar o host de script a partir do tráfego de front-end gerenciado pelo host Web.

## Estrutura de pastas

Um host de script aponta para uma pasta que contém um arquivo de configuração e uma ou mais funções.

```
parentFolder (for example, wwwroot in a function app)
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

O arquivo *host.json* contém uma configuração específica de host de script e fica na pasta pai. Para obter informações sobre as configurações que estão disponíveis, confira [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) no wiki do repositório WebJobs.Script.

Cada função tem uma pasta que contém arquivos de código, *function.json* e outras dependências.

Ao configurar um projeto para implantar funções em um aplicativo de função no Serviço de Aplicativo do Azure, você poderá tratar essa estrutura de pastas como o código do site. Você pode usar ferramentas como implantação e integração contínuas ou scripts de implantação personalizados para implantar a instalação do pacote de tempo ou a transpilação do código.

## <a id="fileupdate"></a> Como atualizar os arquivos de aplicativo de funções

O editor de funções interno do portal do Azure permite que você atualize o arquivo *function.json* e o arquivo de código de uma função. Para carregar ou atualizar outros arquivos, como *package.json* ou *project.json*, ou dependências, você precisa usar outros métodos de implantação.

Os aplicativos de função baseiam-se no Serviço de Aplicativo; portanto, todas as [opções de implantação disponíveis para aplicativos Web padrão](../app-service-web/web-sites-deploy.md) também estão disponíveis para aplicativos de função. Aqui estão alguns métodos que você pode usar para carregar ou atualizar os arquivos de aplicativos de função.

#### Para usar o Visual Studio Online (Monaco)

1. No portal do Azure Functions, clique em **Configurações do aplicativo de funções**.

2. Na seção **Configurações Avançadas**, clique em **Ir para Configurações do Serviço de Aplicativo**.

3. Clique em **Ferramentas**.

4. Em **Desenvolver**, clique em **Visual Studio Online**.

5. **Ative-o** se ainda não estiver habilitado e clique em **Ir**.

	Após carregar o Visual Studio Online, você verá o arquivo *host.json* e pastas de função em *wwwroot*.

6. Abra os arquivos para editá-los ou arraste e solte do computador de desenvolvimento para carregar arquivos.

#### Para usar o ponto de extremidade SCM (Kudu) do aplicativo de funções

1. Navegue até: `https://<function_app_name>.scm.azurewebsites.net`.

2. Clique em **Console de Depuração > CMD**.

3. Navegue até `D:\home\site\wwwroot` para atualizar *host.json* ou `D:\home\site\wwwroot<function_name>` para atualizar os arquivos de uma função.

4. Arraste e solte um arquivo que você deseja carregar para a pasta apropriada na grade de arquivos. Há duas áreas na grade de arquivo onde é possível soltar um arquivo. No caso de arquivos *.zip*, uma caixa é exibida com o rótulo "Arraste até aqui para carregar e descompactar". No caso de outros tipos de arquivo, solte na grade de arquivo, mas fora da caixa "descompactar".

#### Para usar o FTP

1. Siga [estas](../app-service-web/web-sites-deploy.md#ftp) instruções para configurar o FTP.

2. Quando estiver conectado ao site do aplicativo de funções, copie um arquivo *host.json* atualizado para `/site/wwwroot` ou copie arquivos de função para `/site/wwwroot/<function_name>`.

## Execução paralela

Quando vários eventos de gatilho ocorrem mais rápido do que um tempo de execução single-threaded de função pode processar, o tempo de execução pode invocar a função várias vezes em paralelo. Se um aplicativo de funções estiver usando o [Plano de Serviço Dinâmico](functions-scale.md#dynamic-service-plan), ele poderá escalar horizontalmente de modo automático para até dez instâncias simultâneas. Cada instância do aplicativo de funções, quer seja executada no Plano de Serviço Dinâmico, quer em um [Plano do Serviço de Aplicativo](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) comum, pode processar chamadas simultâneas de função em paralelo usando vários threads. O número máximo de chamadas simultâneas de função em cada instância do aplicativo de função varia com base no tamanho da memória do aplicativo de função.

## Azure Functions Pulse  

O Pulse é um fluxo de evento dinâmico que mostra quantas vezes sua função é executada, os êxitos e as falhas. Você também pode monitorar o tempo médio de execução. Incluiremos mais recursos e personalização ao longo do tempo. Você pode acessar a página **Pulso** na guia **Monitoramento**.

## Repositórios

O código para o Azure Functions é software livre e é armazenado em repositórios do GitHub:

* [Tempo de execução do Azure Functions](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Portal do Azure Functions](https://github.com/projectkudu/AzureFunctionsPortal)
* [Modelos do Azure Functions](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [SDK WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk/)
* [Extensões do SDK WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## Associações

Veja uma tabela de todas as associações com suporte.

[AZURE.INCLUDE [computação dinâmica](../../includes/functions-bindings.md)]

## Problemas de relatórios

[AZURE.INCLUDE [Problemas de relatórios](../../includes/functions-reporting-issues.md)]

## Próximas etapas

Para saber mais, consulte os recursos a seguir:

* [Referência do desenvolvedor de C# do Azure Functions](functions-reference-csharp.md)
* [Referência do desenvolvedor de NodeJS do Azure Functions](functions-reference-node.md)
* [Gatilhos e de associações do Azure Functions](functions-triggers-bindings.md)
* [Azure Functions: The Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) (Azure Functions: a jornada) no blog da equipe do Serviço de Aplicativo do Azure. Um histórico de como o Azure Functions foi desenvolvido.

<!---HONumber=AcomDC_0601_2016-->
