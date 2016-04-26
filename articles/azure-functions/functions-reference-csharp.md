<properties
	pageTitle="Referência ao Desenvolvedor de Funções do Azure | Microsoft Azure"
	description="Entenda como são desenvolvidas as Funções do Azure com C#."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor"/>

<tags
	ms.service="functions"
	ms.devlang="dotnet"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="04/06/2016"
	ms.author="chrande"/>

# Referência ao Desenvolvedor de Funções do Azure em C#

A experiência em c# para Funções do Azure é baseada no SDK de WebJobs do Azure. Fluxos de dados em suas funções c# via argumentos de metódos. Nomes de argumentos são espeficiados em `function.json` e existem nomes pré-definidos para acessar partes como funções de logger e tokens de cancelamento.

Este artigo assume que você já efetuou a leitura de [Referência ao desenvolvedor de Funções do Azure Azure](functions-reference.md).

## Como .csx funcionam

O formato `.csx` permite que você escreva menos texto padrão e foque em escrever apenas uma função em c#. Para Funções do Azure, você apenas inclui qualquer referência e namespaces que você precise no começo, como de constume, e ao invés de envolver tudo em uma classe e namespace, você pode apenas definir um método `Executar`. Se você precisa incluir qualquer classe, como por exemplo para definir objetos POCO, você pode incluir dentro do mesmo arquivo.

## Ligação à argumentos

As várias ligações estão vinculados a uma função C # através da propriedade `nome` na sua configuração *function.json*. Cada ligação possui seus próprios tipos suportados que são documentados por ligação; por exemplo, um gatilho em um blob pode suportar uma string, um POCO, ou diversos outros tipos. Você pode usar o tipo que melhor se adequa à sua necessidade.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## Logging

Para logar uma saída do seus logs de streaming em C#, você pode incluir um argumento do tipo `TraceWriter`. Nós recomendamos que você nomeie como `log`. Nós também recomendamos que você evite `Console.Write` em suas Funções do Azure.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
}
```

## Async

Para tornar sua função assíncrona, utilize a palavra `async` e retorne um objeto `Task`.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## Token de Cancelamento

Em alguns casos, você pode obter operações que são sensíveis à serem desligadas. Enquanto é sempre melhor escrever código que possa lidar com falhas, em alguns casos você pode querer tratar solicitações de desligamento, você define um [`Token de Cancelamento`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) como argumento tipado. Um `Token de Cancelamento` será fornecido se um host de desligamento for disparado.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## Importando namespaces

Se você precisa importar namespaces, você pode fazer como de costume, com a cláusula `using`.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Os namespaces à seguir são automaticamente importados e portanto opcionais:

* `System`
* `System.Collections.Generic`
* `System.Linq`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## Referenciando assemblies externos

Para assemblies do framework, você pode adicionar referência com o uso da diretiva `#r "AssemblyName"`.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Os namespaces à seguir são automaticamente adicionados pelo ambiente de hospedagem das Funções do Azure:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Além disso, os seguintes assemblies são casos especiais e podem ser referenciados pelo nome simples (ex: `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Se você precisa referenciar um assembly privado, você pode efetuar o upload do assembly dentro da pasta `bin` relativa À sua função e referencià-lo usando o nome do arquivo (ex: `#r "MyAssembly.dll"`).

## Gerenciamento de pacotes

Para gerenciamento de pacotes, use o arquivo *project.json*. A maioria dos coisas que funcionam com o formato *project.json* , funcionarão com as Funções do Azure. Um detalhe importante é incluir os `frameworks` como `net46`: as Funções do Azure suportam runtime .NET v4.6.

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Humanizer": "2.0.1"
        ...
```

## Próximos passos

Para mais informações, veja os seguintes recursos:

* [Referência ao desenvolvedor para Funções do Azure](functions-reference.md)
* [Referência do desenvolvedor NodeJS para Funções do Azure](functions-reference-node.md)
* [Triggers e ligações de Funções do Azure](functions-triggers-bindings.md)
