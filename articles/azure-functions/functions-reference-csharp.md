<properties
	pageTitle="Refer�ncia ao Desenvolvedor de Fun��es do Azure | Microsoft Azure"
	description="Entenda como s�o desenvolvidas as Fun��es do Azure com C#."
	services="functions"
	documentationCenter="na"
	authors="christopheranderson"
	manager="erikre"
	editor=""
	tags=""
	keywords="fun��es do azure, fun��es, processamento de eventos, webhooks, computa��o din�mica, arquitetura sem servidor"/>

<tags
	ms.service="functions"
	ms.devlang="dotnet"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="04/06/2016"
	ms.author="chrande"/>

# Refer�ncia ao Desenvolvedor de Fun��es do Azure em C#

A experi�ncia em c# para Fun��es do Azure � baseada no SDK de WebJobs do Azure. Fluxos de dados em suas fun��es c# via argumentos de met�dos. Nomes de argumentos s�o espeficiados em `function.json` e existem nomes pr�-definidos para acessar partes como fun��es de logger e tokens de cancelamento.

Este artigo assume que voc� j� efetuou a leitura de [Refer�ncia ao desenvolvedor de Fun��es do Azure Azure](functions-reference.md).

## Como .csx funcionam

O formato `.csx` permite que voc� escreva menos texto padr�o e foque em escrever apenas uma fun��o em c#. Para Fun��es do Azure, voc� apenas inclui qualquer refer�ncia e namespaces que voc� precise no come�o, como de constume, e ao inv�s de envolver tudo em uma classe e namespace, voc� pode apenas definir um m�todo `Executar`. Se voc� precisa incluir qualquer classe, como por exemplo para definir objetos POCO, voc� pode incluir dentro do mesmo arquivo.

## Liga��o � argumentos

As v�rias liga��es est�o vinculados a uma fun��o C # atrav�s da propriedade `nome` na sua configura��o *function.json*. Cada liga��o possui seus pr�prios tipos suportados que s�o documentados por liga��o; por exemplo, um gatilho em um blob pode suportar uma string, um POCO, ou diversos outros tipos. Voc� pode usar o tipo que melhor se adequa � sua necessidade.

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

Para logar uma sa�da do seus logs de streaming em C#, voc� pode incluir um argumento do tipo `TraceWriter`. N�s recomendamos que voc� nomeie como `log`. N�s tamb�m recomendamos que voc� evite `Console.Write` em suas Fun��es do Azure.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
}
```

## Async

Para tornar sua fun��o ass�ncrona, utilize a palavra `async` e retorne um objeto `Task`.

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

Em alguns casos, voc� pode obter opera��es que s�o sens�veis � serem desligadas. Enquanto � sempre melhor escrever c�digo que possa lidar com falhas, em alguns casos voc� pode querer tratar solicita��es de desligamento, voc� define um [`Token de Cancelamento`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) como argumento tipado. Um `Token de Cancelamento` ser� fornecido se um host de desligamento for disparado.

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

Se voc� precisa importar namespaces, voc� pode fazer como de costume, com a cl�usula `using`.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Os namespaces � seguir s�o automaticamente importados e portanto opcionais:

* `System`
* `System.Collections.Generic`
* `System.Linq`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## Referenciando assemblies externos

Para assemblies do framework, voc� pode adicionar refer�ncia com o uso da diretiva `#r "AssemblyName"`.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Os namespaces � seguir s�o automaticamente adicionados pelo ambiente de hospedagem das Fun��es do Azure:

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

Al�m disso, os seguintes assemblies s�o casos especiais e podem ser referenciados pelo nome simples (ex: `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Se voc� precisa referenciar um assembly privado, voc� pode efetuar o upload do assembly dentro da pasta `bin` relativa � sua fun��o e referenci�-lo usando o nome do arquivo (ex: `#r "MyAssembly.dll"`).

## Gerenciamento de pacotes

Para gerenciamento de pacotes, use o arquivo *project.json*. A maioria dos coisas que funcionam com o formato *project.json* , funcionar�o com as Fun��es do Azure. Um detalhe importante � incluir os `frameworks` como `net46`: as Fun��es do Azure suportam runtime .NET v4.6.

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Humanizer": "2.0.1"
        ...
```

## Pr�ximos passos

Para mais informa��es, veja os seguintes recursos:

* [Refer�ncia ao desenvolvedor para Fun��es do Azure](functions-reference.md)
* [Refer�ncia do desenvolvedor NodeJS para Fun��es do Azure](functions-reference-node.md)
* [Triggers e liga��es de Fun��es do Azure](functions-triggers-bindings.md)
