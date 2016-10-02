<properties
   pageTitle="Comparação do Azure Data Lake Store com o Armazenamento de Blobs do Azure | Microsoft Azure"
   description="Comparação do Azure Data Lake Store com o Armazenamento de Blobs do Azure"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# Comparar o Azure Data Lake Store e o Armazenamento de Blobs do Azure

A tabela neste artigo resume as diferenças entre o Azure Data Lake Store e o Armazenamento de Blobs do Azure em alguns aspectos fundamentais do processamento de big data. O Armazenamento de Blobs do Azure é um repositório de objetos escalonável de finalidade geral que foi criado para uma ampla variedade de cenários de armazenamento. O Azure Data Lake Store é um repositório de grande escala otimizado para cargas de trabalho de análise de big data.

| | Repositório Azure Data Lake | Armazenamento do Blobs do Azure |
|----|-----------------------|--------------------|
| Finalidade | Armazenamento otimizado para cargas de trabalho de análise de big data | Repositório de objetos de finalidade geral para uma ampla variedade de cenários de armazenamento |
| Casos de uso | Análise de streaming interativa em lotes e dados de aprendizado de máquina, como arquivos de log, dados da IoT, fluxos de cliques, grandes conjuntos de dados | Qualquer tipo de dados de texto ou binários, como back-end de aplicativo, dados de backup, armazenamento de mídia para streaming e dados de finalidade geral |
| Principais Conceitos | A conta do Data Lake Store contém pastas, que, por sua vez, contêm dados armazenados como arquivos | A conta de armazenamento tem contêineres, que, por sua vez, têm dados na forma de blobs |
| Estrutura | Sistema de arquivos hierárquico | Repositório de objetos com namespace simples |
| API | API REST sobre HTTPS | API REST sobre HTTP/HTTPS |
| API no servidor | [API REST compatível com WebHDFS](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [API REST do Armazenamento de Blobs do Azure](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Cliente do sistema de arquivos Hadoop | Sim | Sim |
| Operações de dados – autenticação | Com base em [Identidades do Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md) | Com base em segredos compartilhados – [Chaves de Acesso de Conta](../storage/storage-create-storage-account.md#manage-your-storage-account) e [Chaves de Assinatura de Acesso Compartilhado](../storage/storage-dotnet-shared-access-signature-part-1.md). |
| Operações de dados – protocolo de autenticação | OAuth 2.0. As chamadas devem conter um JWT (Token Web JSON) válido emitido pelo Azure Active Directory | HMAC (Código de Autenticação de Mensagem Baseado em Hash). As chamadas devem conter um hash SHA-256 codificado na Base64 em uma parte da solicitação HTTP. |
| Operações de dados – autorização | ACLs (Listas de Controle de Acesso) POSIX. ACLs baseadas em Identidades do Azure Active Directory podem ser definidas com nível de arquivo e pasta. | Para autorização de nível de conta – use [Chaves de Acesso de Conta](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Para autorização de conta, contêiner ou blob – use [Chaves de Assinatura de Acesso Compartilhado](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Operações de dados – auditoria | Disponível. Veja mais informações [aqui](data-lake-store-diagnostic-logs.md). | Disponível |
| Dados de criptografia em repouso | Transparente, no servidor (em breve)<ul><li>Com chaves de serviço gerenciado</li><li>Com chaves gerenciadas pelo cliente no Azure KeyVault</li></ul>| <ul><li>Transparente, no servidor</li> <ul><li>Com chaves de serviço gerenciado</li><li>Com chaves gerenciadas pelo cliente no Azure KeyVault (em breve)</li></ul><li>Criptografia no cliente</li></ul> |
| Operações de gerenciamento (por exemplo, Criar Conta) | RBAC ([Controle de acesso baseado em função](../active-directory/role-based-access-control-what-is.md)) fornecido pelo Azure para gerenciamento de contas | RBAC ([Controle de acesso baseado em função](../active-directory/role-based-access-control-what-is.md)) fornecido pelo Azure para gerenciamento de contas |
| SDKs de desenvolvedor | .NET, Java, Node.js | .Net, Java, Python, Node.js, C++, Ruby |
| Desempenho de Carga de Trabalho de Análise | Desempenho otimizado para cargas de trabalho de análise paralela. Alta taxa de transferência e IOPS. | Não otimizado para cargas de trabalho de análise |
| Limites de tamanho | Sem limites para tamanhos de conta, tamanhos de arquivo ou número de arquivos | Limites específicos documentados [aqui](../azure-subscription-service-limits.md#storage-limits) |
| Redundância geográfica | Com redundância local (várias cópias dos dados em uma região do Azure) | LRS (localmente redundante), GRS (globalmente redundante), RA-GRS (acesso de leitura globalmente redundante). Veja mais informações [aqui](../storage/storage-redundancy.md) |
| Estado do serviço | Visualização pública | Disponível |
| Disponibilidade regional | Veja [aqui](https://azure.microsoft.com/regions/#services)| Veja [aqui](https://azure.microsoft.com/regions/#services) |
| Preço | Veja [preços](https://azure.microsoft.com/pricing/details/data-lake-store/)| Veja [preços](https://azure.microsoft.com/pricing/details/storage/) |

### Próximas etapas

- [Visão geral do repositório Azure Data Lake](data-lake-store-overview.md)
- [Introdução ao Repositório Data Lake](data-lake-store-get-started-portal.md)

<!---HONumber=AcomDC_0914_2016-->