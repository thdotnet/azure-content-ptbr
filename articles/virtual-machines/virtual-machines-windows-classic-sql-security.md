<properties
	pageTitle="Considerações de Segurança para o SQL Server no Azure | Microsoft Azure"
	description="Este tópico refere-se a recursos criados com o modelo de implantação clássica e fornece orientação geral para proteger o SQL Server em execução em uma Máquina Virtual do Azure."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="carlrabeler"
	manager="jhubbard"
   editor=""    
   tags="azure-service-management"/>
<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="03/23/2016"
	ms.author="carlrab" />

# Considerações sobre Segurança para SQL Server em Máquinas Virtuais do Azure

Este tópico inclui diretrizes gerais de segurança que ajudam a estabelecer acesso seguro a instâncias do SQL Server em uma VM do Azure. No entanto, para garantir melhor proteção às instâncias de banco de dados do SQL Server no Azure, recomendamos que você implemente as práticas de segurança tradicionais locais, além das práticas recomendadas de segurança do Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Modelo do Gerenciador de Recursos.


Para obter mais informações sobre as práticas de segurança do SQL Server, consulte [Práticas Recomendadas de Segurança do SQL Server 2008 R2 - Tarefas Operacionais e Administrativas](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

O Azure está em conformidade com várias normas e padrões do setor que permitem criar uma solução compatível com o SQL Server em execução em uma máquina virtual. Para obter informações sobre conformidade regulamentar com o Azure, consulte a [Central de Confiabilidade do Azure](https://azure.microsoft.com/support/trust-center/).

Esta é uma lista de recomendações de segurança que deve ser considerada ao configurar e conectar à instância do SQL Server em uma VM do Azure.

## Considerações para gerenciamento de contas:

- Crie uma conta de administrador local exclusiva que não esteja nomeada como **Administrador**.

- Use senhas fortes e complexas para todas as suas contas. Para obter mais informações sobre como criar uma senha forte, consulte o artigo [Dicas para criar senhas fortes](http://windows.microsoft.com/pt-BR/windows-vista/Tips-for-creating-a-strong-password).

- Por padrão, o Azure seleciona a autenticação do Windows durante a instalação da máquina Virtual do SQL Server. Portanto, o logon **SA** é desabilitado e uma senha é atribuída pela instalação. É recomendável que o logon **SA** não seja usado nem habilitado. A seguir, algumas estratégias alternativas caso você queira um logon do SQL:
	- Crie uma conta do SQL que tenha associação a sysadmin.
	- Se você precisar usar um logon **SA**, habilite o logon, renomeie-o e atribua uma nova senha.
	- As opções mencionadas anteriormente exigem uma alteração do modo de autenticação para **Modo de Autenticação do SQL Server e do Windows**. Para obter mais informações, consulte [Alterar Modo de Autenticação do Servidor](https://msdn.microsoft.com/library/ms188670.aspx).

## Considerações sobre conexões seguras à máquina Virtual do Azure:

- Considere o uso da [Rede Virtual do Azure](../virtual-network/virtual-networks-overview.md) para administrar máquinas virtuais, em vez de portas RDP públicas.

- Use um [NSG](../virtual-network/virtual-networks-nsg.md) (grupo de segurança de rede) para permitir ou negar tráfego de rede à sua máquina virtual. Se você quiser usar um NSG e já tiver uma ACL de ponto de extremidade à disposição, primeiro remova a ACL de ponto de extremidade. Para saber mais sobre como fazer isso, confira [Gerenciando listas de controle de acesso (ACLs) para pontos de extremidade usando o PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

- Se você está usando pontos de extremidade, remova quaisquer pontos de extremidade da máquina virtual, se não for usá-los. Para obter instruções sobre como usar ACLs com pontos de extremidade, consulte [Gerenciar a ACL em um ponto de extremidade](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- Habilite uma opção de conexão criptografada para uma instância do mecanismo de banco de dados do SQL Server em máquinas virtuais do Azure. Configure a instância do SQL Server com um certificado assinado. Para obter mais informações, consulte [Habilitar Conexões Criptografadas ao Mecanismo de Banco de Dados](https://msdn.microsoft.com/library/ms191192.aspx) e [Sintaxe de Cadeia de Conexão](https://msdn.microsoft.com/library/ms254500.aspx).

- Se as máquinas virtuais forem acessadas somente de uma rede específica, use o Firewall do Windows para restringir o acesso a determinados endereços IP ou sub-redes da rede.

## Próximas etapas

Se você também estiver interessado em práticas recomendadas sobre desempenho, consulte [Práticas Recomendadas de Desempenho do SQL Server em Máquinas Virtuais do Azure](virtual-machines-windows-classic-sql-perf.md).

Para outros tópicos relacionados à execução do SQL Server em VMs do Azure, consulte [Visão geral do SQL Server em Máquinas Virtuais do Azure](virtual-machines-windows-classic-sql-overview.md).

<!-----------HONumber=AcomDC_0330_2016-->