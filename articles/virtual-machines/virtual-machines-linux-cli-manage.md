<properties 
   pageTitle="Comandos básicos da CLI do Azure para Linux e Mac | Microsoft Azure"
   description="Comandos básicos da CLI do Azure para ajudá-lo a começar a gerenciar suas VMs no modo do Azure Resource Manager no Linux e Mac"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="RicksterCDN" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="04/11/2016"
   ms.author="rclaus" />

# Comandos comuns da CLI do Azure em Linux e Mac

Antes de usar a CLI do Azure com modelos e comandos do Gerenciador de Recursos para implantar recursos do Azure e cargas de trabalho usando grupos de recursos, será necessário ter uma conta com o Azure. Se você não tiver uma conta, você pode obter uma [avaliação gratuita do Azure aqui](https://azure.microsoft.com/pricing/free-trial/).

Se você ainda não tiver instalado a CLI do Azure e se conectado à sua assinatura, consulte [Instalar a CLI do Azure](../xplat-cli-install.md), defina o modo como `arm` com `azure config mode arm` e se conecte ao Azure com o comando `azure login`.

## Comandos básicos do Azure Resource Manager na CLI do Azure

Este artigo aborda os comandos básicos que você desejará usar com a CLI do Azure para gerenciar e interagir com os recursos do ARM (principalmente as VMs) em sua assinatura do Azure. Para obter ajuda mais detalhada com opções específicas de linha de comando, você pode usar as opções e a ajuda online sobre os comandos digitando `azure <command> <subcommand> --help` ou `azure help <command> <subcommand>`.

> [AZURE.NOTE] Esses exemplos não incluem operações baseadas em modelo que geralmente são recomendadas para implantações de VM no Gerenciador de Recursos. Para saber mais, confira [Usar a CLI do Azure com o Gerenciador de Recursos do Azure](../xplat-cli-azure-resource-manager.md) [Implantar e gerenciar máquinas virtuais usando modelos do Gerenciador de Recursos do Azure e a CLI do Azure](virtual-machines-linux-cli-deploy-templates.md).

Tarefa | Gerenciador de Recursos
-------------- | ----------- | -------------------------
Criar a VM mais básica | `azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(Obtenha o `image-urn` com o comando `azure vm image list`. Confira [este artigo](virtual-machines-linux-cli-ps-findimage.md) para obter exemplos.)
Criar uma VM do Linux | `azure  vm create [options] <resource-group> <name> <location> -y "Linux"`
Criar uma VM do Windows | `azure  vm create [options] <resource-group> <name> <location> -y "Windows"`
Listar VMs | `azure  vm list [options]`
Obter informações sobre uma VM | `azure  vm show [options] <resource_group> <name>`
Iniciar uma VM | `azure vm start [options] <resource_group> <name>`
Parar uma VM | `azure vm stop [options] <resource_group> <name>`
Desalocar uma VM | `azure vm deallocate [options] <resource-group> <name>`
Reiniciar uma VM | `azure vm restart [options] <resource_group> <name>`
Excluir uma VM | `azure vm delete [options] <resource_group> <name>`
Capturar uma VM | `azure vm capture [options] <resource_group> <name>`
Criar uma VM por meio de uma imagem do usuário | `azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>`
Criar uma VM por meio de um disco especializado | `azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>`
Adicionar um disco de dados a uma VM | `azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]`
Remover um disco de dados de uma VM | `azure  vm disk detach [options] <resource-group> <vm-name> <lun>`
Adicionar uma extensão genérica a uma VM |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>`
Adicionar a extensão de acesso de VM a uma VM | `azure vm reset-access [options] <resource-group> <name>`
Adicionar uma extensão Docker a uma VM | `azure  vm docker create [options] <resource-group> <name> <location> <os-type>`
Remover uma extensão de VM | `azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>`
Conferir o uso dos recursos de VM | `azure vm list-usage [options] <location>`
Conferir todos os tamanhos de VM disponíveis | `azure vm sizes [options]`


## Próximas etapas

* Para obter exemplos adicionais dos comandos da CLI que vão além do gerenciamento básico de VMs, consulte [Usando a CLI do Azure com o Azure Resource Manager](azure-cli-arm-commands.md).

<!---HONumber=AcomDC_0504_2016-->