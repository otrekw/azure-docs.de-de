---
title: Azure-Diagnoseüberwachung für Azure Attestation
description: Azure-Diagnoseüberwachung für Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 2b0cd0402348e4aa45b291f30b677fc9e4bbdb98
ms.sourcegitcommit: 3c460886f53a84ae104d8a09d94acb3444a23cdc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107833632"
---
# <a name="set-up-diagnostics-with-a-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Einrichten der Diagnose mit einem TPM-Endpunkt (Trusted Platform Module) von Azure Attestation

Dieser Artikel unterstützt Sie beim Erstellen und Konfigurieren von Diagnoseeinstellungen, um Plattformmetriken und -protokolle an verschiedene Ziele zu senden. [Plattformprotokolle](/azure/azure-monitor/platform/platform-logs-overview) in Azure, z. B. das Azure-Aktivitätsprotokoll und Ressourcenprotokolle, liefern ausführliche Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. [Plattformmetriken](/azure/azure-monitor/platform/data-platform-metrics) werden standardmäßig gesammelt und in der Azure Monitor-Metrikdatenbank gespeichert.

Stellen Sie zunächst sicher, dass Sie [Azure Attestation mit Azure PowerShell eingerichtet haben](quickstart-powershell.md).

Der TMP-Endpunktdienst (Trusted Platform Module) wird in den Diagnoseeinstellungen aktiviert und kann zum Überwachen der Aktivität verwendet werden. Richten Sie die [Azure-Überwachung](/azure/azure-monitor/overview) für den TPM-Dienstendpunkt mithilfe des folgenden Codes ein:

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```

Aktivitätsprotokolle befinden sich im Abschnitt **Container** des Speicherkontos. Weitere Informationen finden Sie unter [Tutorial: Sammeln und Analysieren von Ressourcenprotokollen von einer Azure-Ressource](/azure/azure-monitor/learn/tutorial-resource-logs).
