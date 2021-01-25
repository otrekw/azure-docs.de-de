---
title: 'Azure-Diagnoseüberwachung: Azure Attestation'
description: Azure-Diagnoseüberwachung für Azure Attestation
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 40bc76f839cf6757b8f874112504249e611c3e1a
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98605945"
---
# <a name="setting-up-diagnostics-with-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Einrichten der Diagnose mit dem TPM-Endpunkt (Trusted Platform Module) von Azure Attestation

[Plattformprotokolle](/azure/azure-monitor/platform/platform-logs-overview) in Azure, z. B. das Azure-Aktivitätsprotokoll und Ressourcenprotokolle, liefern ausführliche Diagnose- und Überwachungsinformationen für Azure-Ressourcen und die Azure-Plattform, von der sie abhängen. [Plattformmetriken](/azure/azure-monitor/platform/data-platform-metrics) werden standardmäßig gesammelt und in der Regel in der Azure Monitor-Metrikdatenbank gespeichert. Dieser Artikel enthält Details zum Erstellen und Konfigurieren von Diagnoseeinstellungen, um Plattformmetriken und -protokolle an verschiedene Ziele zu senden. 

Der TPM-Endpunktdienst wird mit der Diagnoseeinstellung aktiviert und kann zum Überwachen der Aktivität verwendet werden. Führen Sie die folgenden Schritte aus, um die [Azure-Überwachung](/azure/azure-monitor/overview) für den TPM-Dienstendpunkt mithilfe von PowerShell einzurichten: 

Einrichten des Azure Attestation-Diensts 

[Einrichten von Azure Attestation mithilfe von Azure PowerShell](/azure/attestation/quickstart-powershell#:~:text=%20Quickstart%3A%20Set%20up%20Azure%20Attestation%20with%20Azure,Register%20Microsoft.Attestation%20resource%20provider.%20Register%20the...%20More%20)

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```
Die Aktivitätsprotokolle finden Sie im Abschnitt „Container“ des Speicherkontos. Ausführliche Informationen finden Sie unter [Tutorial: Sammeln und Analysieren von Ressourcenprotokollen von einer Azure-Ressource](/azure/azure-monitor/learn/tutorial-resource-logs).
