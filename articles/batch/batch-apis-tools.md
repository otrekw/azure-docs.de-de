---
title: APIs und Tools für Entwickler
description: Es werden die APIs und Tools beschrieben, die zum Entwickeln von Lösungen mit dem Azure Batch-Dienst verfügbar sind.
ms.topic: conceptual
ms.date: 05/22/2010
ms.custom: seodec18
ms.openlocfilehash: cd87419cd8c389be861cb88071884f8da1c3c1a3
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83870597"
---
# <a name="overview-of-batch-apis-and-tools"></a>Übersicht über Batch-APIs und -Tools

Die Verarbeitung paralleler Workloads mit Azure Batch wird üblicherweise programmgesteuert mit einer der Batch-APIs durchgeführt. Ihre Clientanwendung oder Ihr Dienst kann mithilfe der Batch-APIs mit dem Batch-Dienst kommunizieren. Mithilfe der Batch-APIs können Sie Pools mit Computeknoten (virtuelle Computer oder Clouddienste) erstellen und verwalten. Anschließend können Sie die Ausführung von Aufträgen und Aufgaben auf diesen Knoten planen.

Sie können größere Workloads für Ihr Unternehmen auf effiziente Weise verarbeiten oder ein Dienst-Front-End für Ihre Kunden bereitstellen, damit sie Aufträge und Aufgaben – bedarfsgesteuert oder nach Zeitplan – auf einem, Hunderten oder Tausenden von Knoten ausführen können. Sie können Azure Batch auch als Teil eines größeren Workflows nutzen und mithilfe von Tools wie [Azure Data Factory](../data-factory/transform-data-using-dotnet-custom-activity.md?toc=%2fazure%2fbatch%2ftoc.json) verwalten.

> [!TIP]
> Weitere Informationen zu den in Azure Batch verwendeten Features und dem Workflow finden Sie unter [Workflow und Ressourcen des Batch-Diensts](batch-service-workflow-features.md).

## <a name="azure-accounts-for-batch-development"></a>Azure-Konten für die Batch Entwicklung

Beim Entwickeln von Batch-Lösungen verwenden Sie die folgenden Konten in Ihrem Azure-Abonnement:

- **Batch-Konto:** Azure Batch-Ressourcen (einschließlich Pools, Computeknoten, Aufträgen und Aufgaben) werden einem [Azure Batch-Konto](accounts.md) zugeordnet. Wenn Ihre Anwendung eine Anforderung an den Batch-Dienst richtet, wird die Anforderung anhand des Azure Batch-Kontonamens, der URL des Kontos und entweder eines Zugriffsschlüssels oder eines Azure Active Directory-Tokens authentifiziert. Die [Erstellung eines Batch-Kontos](batch-account-create-portal.md) kann über das Azure-Portal oder programmgesteuert erfolgen.
- **Speicherkonto:** Batch unterstützt die Verwendung von Dateien in [Azure Storage](../storage/index.yml). In nahezu jedem Batch-Szenario wird Azure Blob Storage für das Staging der Programme, die von Ihren Aufgaben ausgeführt werden, und für die dabei verarbeiteten Daten sowie für die Speicherung der generierten Ausgabedaten verwendet. Jedes Batch-Konto ist normalerweise einem Speicherkonto zugeordnet.

## <a name="service-level-and-management-level-apis"></a>APIs auf Dienst- und Verwaltungsebene

Azure Batch verfügt über zwei Sätze von APIs, einen für die Dienstebene und einen für die Verwaltungsebene. Ihre Namen sind oft ähnlich, aber sie führen zu unterschiedlichen Ergebnissen.

Nur Aktionen aus den Verwaltungs-APIs werden im Aktivitätsprotokoll nachverfolgt. APIs auf Dienstebene umgehen die Azure-Ressourcenverwaltungsschicht (management.azure.com) und werden nicht protokolliert.

Beispielsweise ist die [API des Batch-Diensts zum Löschen eines Pools](https://docs.microsoft.com/rest/api/batchservice/pool/delete) direkt auf das Batch-Konto ausgerichtet: `DELETE {batchUrl}/pools/{poolId}`

Die [Batch-Verwaltungs-API zum Löschen eines Pools](https://docs.microsoft.com/rest/api/batchmanagement/pool/delete) ist hingegen auf die Schicht management.azure.com ausgerichtet: `DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Batch/batchAccounts/{accountName}/pools/{poolName}`

## <a name="batch-service-apis"></a>Batch-Dienst-APIs

Ihre Anwendungen und Dienste können direkte REST-API-Aufrufe ausgeben oder Ihre Azure Batch-Workloads unter Verwendung einer oder mehrerer der folgenden Clientbibliotheken ausführen und verwalten:

| API | API-Referenz | Download | Lernprogramm | Codebeispiele | Weitere Informationen |
| --- | --- | --- | --- | --- | --- |
| **Batch REST (in englischer Sprache)** |[docs.microsoft.com](https://docs.microsoft.com/rest/api/batchservice/) |– |- |- | [Unterstützte Versionen](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com](https://docs.microsoft.com/dotnet/api/overview/azure/batch?view=azure-dotnet) |[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Batch/) |[Tutorial](tutorial-parallel-dotnet.md) |[GitHub](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp) | [Versionshinweise](https://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[docs.microsoft.com](https://docs.microsoft.com/python/api/overview/azure/batch/client?view=azure-python) |[PyPI](https://pypi.org/project/azure-batch/) |[Tutorial](tutorial-parallel-python.md)|[GitHub](https://github.com/Azure-Samples/azure-batch-samples/tree/master/Python/Batch) | [Infodatei](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/batch/azure-batch/README.md) |
| **Batch Node.js** |[docs.microsoft.com](https://docs.microsoft.com/javascript/api/overview/azure/batch/client?view=azure-node-latest) |[npm](https://www.npmjs.com/package/azure-batch) |[Tutorial](batch-nodejs-get-started.md) |- | [Infodatei](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[docs.microsoft.com](https://docs.microsoft.com/java/api/overview/azure/batch?view=azure-java-stable) |[Maven](https://search.maven.org/search?q=a:azure-batch) |- |[GitHub](https://github.com/Azure-Samples/azure-batch-samples/tree/master/Java) | [Infodatei](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Batch Management-APIs

Die Azure Resource Manager-APIs für Batch ermöglichen den programmgesteuerten Zugriff auf Batch-Konten. Mit diesen APIs können Sie Batch-Konten, Kontingente, Anwendungspakete und andere Ressourcen über den Microsoft.Batch-Anbieter programmgesteuert verwalten.  

| API | API-Referenz | Download | Lernprogramm | Codebeispiele |
| --- | --- | --- | --- | --- |
| **Batch Management – REST** |[docs.microsoft.com](https://docs.microsoft.com/rest/api/batchmanagement/) |- |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Management .NET** |[docs.microsoft.com](https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet) |[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/) | [Tutorial](batch-management-dotnet.md) |[GitHub](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp) |
| **Batch Management – Python** |[docs.microsoft.com](https://docs.microsoft.com/python/api/overview/azure/batch/management?view=azure-python) |[PyPI](https://pypi.org/project/azure-mgmt-batch/) |- |- |
| **Batch Management – Node.js** |[docs.microsoft.com](https://docs.microsoft.com/javascript/api/overview/azure/batch/management?view=azure-node-latest) |[npm](https://www.npmjs.com/package/azure-arm-batch) |- |- | 
| **Batch Management – Java** |[docs.microsoft.com](https://docs.microsoft.com/java/api/overview/azure/batch/management?view=azure-java-stable) |[Maven](https://search.maven.org/search?q=a:azure-batch) |- |- |

## <a name="batch-command-line-tools"></a>Batch-Befehlszeilentools

Diese Befehlszeilentools bieten die gleiche Funktionalität wie die Batch-Dienst- und Batch Management-APIs: 

- [Batch-PowerShell-Cmdlets](https://docs.microsoft.com/powershell/module/az.batch/): Mit den Azure Batch-Cmdlets im [Azure PowerShell](/powershell/azure/overview)-Modul können Sie Batch-Ressourcen mit PowerShell verwalten.
- [Azure CLI](/cli/azure): Die Azure CLI ist ein plattformübergreifendes Toolset mit Shellbefehlen für die Interaktion mit vielen Azure-Diensten, z.B. dem Batch-Dienst und dem Batch Management-Dienst. Weitere Informationen zur Verwendung der Azure CLI mit Batch finden Sie unter [Verwalten von Batch-Ressourcen mit der Azure CLI](batch-cli-get-started.md).

## <a name="other-tools-for-application-development"></a>Weitere Tools für die Anwendungsentwicklung

Diese zusätzlichen Tools können beim Entwickeln und Debuggen Ihrer Batch-Anwendungen und -Dienste hilfreich sein.

- [Azure-Portal](https://portal.azure.com/): Über das Azure-Portal können Sie Batch-Pools, -Aufträge und -Aufgaben erstellen, überwachen und löschen. Sie können Statusinformationen für diese und andere Ressourcen beim Ausführen der Aufträge anzeigen und sogar Dateien von den Serverknoten in Ihre Pools herunterladen. Beispielsweise können Sie bei der Problembehandlung die Datei `stderr.txt` einer fehlerhaften Aufgabe herunterladen. Außerdem können Sie RDP-Dateien (Remotedesktop) herunterladen, die sich zum Anmelden an Computeknoten eignen.
- [Azure Batch Explorer](https://azure.github.io/BatchExplorer/): Batch Explorer (früher: BatchLabs) ist ein kostenloses eigenständiges Clienttool mit zahlreichen Features, das Sie beim Erstellen, Debuggen und Überwachen von Azure Batch-Anwendungen unterstützt. Ein Installationspaket für Mac, Linux oder Windows können Sie [hier](https://azure.github.io/BatchExplorer/) herunterladen.
- [Azure Batch Shipyard:](https://github.com/Azure/batch-shipyard) Batch Shipyard ist ein Tool, das Sie beim Bereitstellen, Ausführen und Überwachen der containerbasierten Batchverarbeitung und von HPC-Workloads in Azure Batch unterstützt.
- [Azure Storage-Explorer](https://azure.microsoft.com/features/storage-explorer/): Dies ist zwar kein Azure Batch-Tool, aber der Storage-Explorer ist ein weiteres wertvolles Tool, das Sie beim Entwickeln und Debuggen Ihrer Batch-Lösungen einsetzen können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Weitere Informationen zum Protokollieren von Ereignissen in der Batch-Anwendung finden Sie unter [Batch-Metriken, -Warnungen und -Protokolle für die Diagnoseauswertung und -überwachung](batch-diagnostics.md).
- Referenzinformationen zu Ereignissen, die vom Batch-Dienst ausgelöst werden, finden Sie unter [Batch-Analysen](batch-analytics.md).
- Informationen zu Umgebungsvariablen für Serverknoten finden Sie unter [Umgebungsvariablen der Azure Batch-Laufzeit](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den [Workflow des Batch-Diensts und primäre Ressourcen](batch-service-workflow-features.md) wie Pools, Knoten, Aufträge und Aufgaben.
- [Erste Schritte mit der Azure Batch-Bibliothek für .NET](tutorial-parallel-dotnet.md) erfahren Sie, wie Sie mit C# und der Batch .NET-Bibliothek eine einfache Workload mit einem allgemeinen Batch-Workflow ausführen. Es stehen auch eine [Python-Version](tutorial-parallel-python.md) und ein [Node.js-Tutorial](batch-nodejs-get-started.md) zur Verfügung.
- Laden Sie die [Codebeispiele von GitHub](https://github.com/Azure-Samples/azure-batch-samples) herunter. Diese zeigen, wie C# und Python mit Batch verknüpft werden können, um Beispielworkloads zu planen und zu verarbeiten.
