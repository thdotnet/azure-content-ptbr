<properties 
	pageTitle="Bloquear recursos com o Gerenciador de Recursos | Microsoft Azure" 
	description="Impeça que os usuários atualizem ou excluam determinados recursos ao aplicar uma restrição a todos os usuários e funções." 
	services="azure-resource-manager" 
	documentationCenter="" 
	authors="tfitzmac" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="azure-resource-manager" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/04/2016" 
	ms.author="tomfitz"/>

# Bloquear recursos com o Gerenciador de Recursos do Azure

Como administrador, há situações nas quais você deseja colocar um bloqueio em uma assinatura, em um grupo de recursos ou em um recurso para impedir que outros usuários em sua organização excluam acidentalmente recursos essenciais. Quando bloqueados, os usuários autorizados ainda poderão ler e modificar os recursos, mas não poderão excluir os recursos.

Os bloqueios são diferentes do uso de controle de acesso baseado em função para atribuir permissões de usuário para executar determinadas ações. Para saber mais sobre como configurar permissões para usuários e funções, confira [Controle de Acesso Baseado em Função do Azure](./active-directory/role-based-access-control-configure.md). Ao contrário do controle de acesso baseado em função, você utiliza os bloqueios de gerenciamento para aplicar uma restrição a todos os usuários e funções, e normalmente aplica os bloqueios por apenas uma duração limitada.

Quando você aplica um bloqueio a um escopo pai, todos os recursos filho herdam o mesmo bloqueio.

## Quem pode criar ou excluir bloqueios na sua organização

Para criar ou excluir os bloqueios de gerenciamento, você deverá ter acesso às ações **Microsoft.Authorization/*** ou **Microsoft.Authorization/locks/***. Das funções internas, somente **Proprietário** e **Administrador do Acesso de Usuário** recebem essas ações. Para saber mais sobre como atribuir o controle de acesso, confira [Controle de acesso baseado em função do Azure](./active-directory/role-based-access-control-configure.md).

## Criando um bloqueio em um modelo

O exemplo a seguir mostra um modelo que cria um bloqueio em uma conta de armazenamento. A conta de armazenamento em que o bloqueio será aplicado é fornecida como um parâmetro. O nome do bloqueio é criado por meio da concatenação do nome do recurso com **/Microsoft.Authorization/** e o nome do bloqueio, nesse caso, **myLock**.

O tipo fornecido é específico para o tipo de recurso. Para armazenamento, esse tipo é "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## Criando um bloqueio com a API REST

Você pode bloquear os recursos implantados com a [API REST para bloqueios de gerenciamento](https://msdn.microsoft.com/library/azure/mt204563.aspx). A API REST permite que você crie e exclua bloqueios e recupere informações sobre bloqueios existentes.

Para criar um bloqueio, execute:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

O escopo pode ser uma assinatura, grupo de recursos ou recurso. O nome do bloqueio é como você deseja chamar o bloqueio. Para a api-version, use **2015-01-01**.

Na solicitação, inclua um objeto JSON que especifica as propriedades do bloqueio.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Para obter exemplos, confira [API REST para bloqueios de gerenciamento](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## Criando um bloqueio com o Azure PowerShell

Você pode bloquear os recursos implantados com o Azure PowerShell usando o **New-AzureRmResourceLock** como mostrado abaixo.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites

O Azure PowerShell fornece outros comandos para trabalhar com bloqueios, tais como **Set-AzureRmResourceLock** para atualizar um bloqueio e **Remove-AzureRmResourceLock** para excluir um bloqueio.

## Próximas etapas

- Para saber mais sobre como trabalhar com bloqueios de recursos, confira [Bloquear os recursos do Azure](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Para saber mais sobre a organização lógica de recursos, confira [Usando marcas para organizar os recursos](resource-group-using-tags.md)
- Para alterar o grupo de recursos em que um recurso reside, confira [Mover recursos para um novo grupo de recursos](resource-group-move-resources.md)
- É possível aplicar restrições e convenções em sua assinatura com políticas personalizadas. Para saber mais, confira [Usar a Política para gerenciar recursos e controlar o acesso](resource-manager-policy.md).

<!---HONumber=AcomDC_0406_2016-->