---
title: 'Azure-Diagnoseüberwachung: Azure Attestation'
description: Azure-Diagnoseüberwachung für Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: d01e7817906927295591353b710afe2899aacdf1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101726477"
---
# <a name="setting-up-diagnostics-with-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Einrichten der Diagnose mit dem TPM-Endpunkt (Trusted Platform Module) von Azure Attestation

[Plattformprotokolle](../azure-monitor/essentials/platform-logs-overview.md) in Azure, z. B. das Azure-Aktivitätsprotokoll und Ressourcenprotokolle, liefern ausführliche Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. [Plattformmetriken](../azure-monitor/essentials/data-platform-metrics.md) werden standardmäßig gesammelt und in der Regel in der Azure Monitor-Metrikdatenbank gespeichert. Dieser Artikel enthält Details zum Erstellen und Konfigurieren von Diagnoseeinstellungen, um Plattformmetriken und -protokolle an verschiedene Ziele zu senden. 

Der TPM-Endpunktdienst wird mit der Diagnoseeinstellung aktiviert und kann zum Überwachen der Aktivität verwendet werden. Führen Sie die folgenden Schritte aus, um die [Azure-Überwachung](../azure-monitor/overview.md) für den TPM-Dienstendpunkt mithilfe von PowerShell einzurichten: 

Einrichten des Azure Attestation-Diensts 

[Einrichten von Azure Attestation mithilfe von Azure PowerShell](./quickstart-powershell.md)

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```
Die Aktivitätsprotokolle finden Sie im Abschnitt „Container“ des Speicherkontos. Ausführliche Informationen finden Sie unter [Tutorial: Sammeln und Analysieren von Ressourcenprotokollen von einer Azure-Ressource](../azure-monitor/essentials/tutorial-resource-logs.md).
