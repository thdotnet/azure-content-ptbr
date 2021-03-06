<properties 
	pageTitle="Habilitar o diagnóstico nos Serviços de Nuvem do Azure usando o PowerShell | Microsoft Azure" 
	description="Saiba como habilitar o diagnóstico nos serviços de nuvem usando o PowerShell" 
	services="cloud-services" 
	documentationCenter=".net" 
	authors="sbtron" 
	manager="" 
	editor=""/>

<tags 
	ms.service="cloud-services" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="na" 
	ms.devlang="dotnet" 
	ms.topic="article" 
	ms.date="02/09/2016" 
	ms.author="saurabh"/>


# Habilitar o diagnóstico nos Serviços de Nuvem do Azure usando o PowerShell

Você pode coletar dados de diagnóstico, como logs de aplicativo, contador de desempenho, etc., a partir de um Serviço de Nuvem usando a extensão de Diagnóstico do Azure. Este artigo descreve como habilitar a extensão de Diagnóstico do Azure para um Serviço de Nuvem usando o PowerShell. Consulte [Como instalar e configurar o Azure PowerShell](powershell-install-configure.md) para os pré-requisitos necessários para este artigo.

## Habilitar a extensão de diagnóstico como parte da implantação de um Serviço de Nuvem

Essa abordagem é boa para o tipo de cenários de integração contínua no qual a extensão de diagnóstico pode ser habilitada como parte da implantação do serviço de nuvem. Ao criar uma nova implantação de Serviço de Nuvem, você pode habilitar a extensão do diagnóstico passando o parâmetro *ExtensionConfiguration* para o cmdlet [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx). O parâmetro *ExtensionConfiguration* obtém uma série de configurações de diagnóstico que podem ser criadas usando o cmdlet [New-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx).

O exemplo a seguir mostra como você pode habilitar o diagnóstico para um serviço de nuvem com um WebRole e WorkerRole, cada um com uma configuração de diagnóstico diferente.

	$service_name = "MyService"
	$service_package = "CloudService.cspkg"
	$service_config = "ServiceConfiguration.Cloud.cscfg"
	$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
	$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

	$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
	$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath
	 
	New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig) 

Se o arquivo de configuração do diagnóstico especificar um elemento StorageAccount com um nome de conta de armazenamento, o cmdlet New-AzureServiceDiagnosticsExtensionConfig usará automaticamente essa conta de armazenamento. Para que isso funcione, a conta de armazenamento precisa estar na mesma assinatura que o Serviço de Nuvem que está sendo implantado.

A partir do SDK 2.6 do Azure, os arquivos de configuração de extensão gerados pela saída de destino de publicação do MSBuild incluirá o nome da conta de armazenamento, com base na cadeia de configuração de diagnóstico especificada no arquivo de configuração de serviço (.cscfg). O script a seguir mostra como analisar os arquivos de configuração de extensão da saída de destino de publicação, e como configurar a extensão de diagnóstico para cada função durante a implantação do serviço de nuvem.

	$service_name = "MyService"
	$service_package = "C:\build\output\CloudService.cspkg"
	$service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"
	
	#Find the Extensions path based on service configuration file
	$extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"
	
	$diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
	$diagnosticsConfigurations = @()
	foreach ($extPath in $diagnosticsExtensions)
	{
	#Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
	$roleName = ""
	$roles = $extPath -split ".",0,"simplematch"
	if ($roles -is [system.array] -and $roles.Length -gt 1)
	    {
	    $roleName = $roles[1] 
	    $x = 2
	    while ($x -le $roles.Length)
	        {
	           if ($roles[$x] -ne "PubConfig")
	            {
	                $roleName = $roleName + "." + $roles[$x]
	            }
	            else
	            {
	                break
	            }
	            $x++
	        }
	    $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
	    $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
	    $diagnosticsConfigurations += $diagnosticsconfig
	    }
	}
	New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

O Visual Studio Online usa uma abordagem parecida para implantações automatizadas dos Serviços de Nuvem com a extensão de diagnóstico. Confira [Publish-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) para obter um exemplo completo.

Se nenhuma StorageAccount tiver sido especificada na configuração de diagnóstico, você precisará passar o parâmetro StorageAccountName para o cmdlet. Se o parâmetro StorageAccountName for especificado, o cmdlet sempre usará a conta de armazenamento que está especificada no parâmetro, não aquela que está especificada no arquivo de configuração de diagnóstico.

Se a conta de armazenamento de diagnóstico estiver em uma assinatura diferente do Serviço de Nuvem, será necessário transmitir explicitamente os parâmetros StorageAccountName e StorageAccountKey para o cmdlet. O parâmetro StorageAccountKey não é necessário quando a conta de armazenamento de diagnóstico está na mesma assinatura, uma vez que o cmdlet pode consultar e definir automaticamente o valor de chave ao habilitar a extensão de diagnóstico. No entanto, se a conta de armazenamento de diagnóstico estiver em uma assinatura diferente, o cmdlet talvez não possa obter a chave automaticamente e você precisará explicitamente especificar a chave por meio do parâmetro StorageAccountKey.

	$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
	$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
 

## Habilitar extensão de diagnóstico em um Serviço de Nuvem existente

Você pode usar o cmdlet [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) para habilitar ou atualizar a configuração do diagnóstico em um Serviço de Nuvem que já está em execução.


	$service_name = "MyService"
	$webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
	$workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

	$webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
	$workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath
	
	Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name 
	  

## Obter a configuração de extensão de diagnóstico atual
Usar o cmdlet [Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) para obter a configuração de diagnóstico atual para um serviço de nuvem.
	
	Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## Remover extensão de diagnóstico
Para desativar o diagnóstico em um serviço de nuvem, você pode usar o cmdlet [Remove-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx).

	Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Se você tiver habilitado a extensão de diagnóstico usando o *Set-AzureServiceDiagnosticsExtension* ou o *New-AzureServiceDiagnosticsExtensionConfig* sem o parâmetro *Função*, pode remover a extensão usando *Remove-AzureServiceDiagnosticsExtension* sem o parâmetro *Função*. Se o parâmetro *Função* tiver sido usado ao habilitar a extensão, ele também deverá ser usado ao removê-la.

Para remover a extensão de diagnóstico de cada função individual:

	Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## Próximas etapas

- Para obter orientação adicional sobre como usar o diagnóstico do Azure e outras técnicas para solucionar problemas, consulte [Habilitar o Diagnóstico nos Serviços de Nuvem do Azure e Máquinas Virtuais](cloud-services-dotnet-diagnostics.md).
- O [Esquema de Configuração de Diagnóstico](https://msdn.microsoft.com/library/azure/dn782207.aspx) explica as várias opções de configurações de XML para a extensão de diagnóstico.
- Para saber como habilitar a extensão de diagnóstico para Máquinas Virtuais, consulte [Criar uma Máquina Virtual do Windows com monitoramento e diagnóstico usando o Modelo do Gerenciador de Recursos do Azure](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  

<!---HONumber=AcomDC_0323_2016-->