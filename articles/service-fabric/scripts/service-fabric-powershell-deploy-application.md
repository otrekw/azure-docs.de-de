---
title: Bereitstellen einer Anwendung in einem Cluster in PowerShell
description: Azure PowerShell-Skriptbeispiel – Bereitstellen einer Anwendung in einem Service Fabric-Cluster.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 1d25ede5ae871eddd965594224b518ec42525451
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98791289"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Bereitstellen einer Anwendung in einem Service Fabric-Cluster

Dieses Beispielskript kopiert ein Anwendungspaket in einen Clusterimagespeicher, registriert den Anwendungstyp im Cluster, entfernt das nicht benötigte Anwendungspaket und erstellt eine Anwendungsinstanz aus dem Anwendungstyp.  Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden diese Dienste in diesem Schritt erstellt. Passen Sie die Parameter nach Bedarf an. 

Wenn Sie das Service Fabric-PowerShell-Modul benötigen, installieren Sie es zusammen mit dem [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nachdem das Beispielskript ausgeführt wurde, kann das Skript unter [Entfernen einer Anwendung](service-fabric-powershell-remove-application.md) dazu verwendet werden, die Anwendungsinstanz zu entfernen, die Registrierung des Anwendungstyps aufzuheben und das Anwendungspaket aus dem Imagespeicher zu löschen.

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
|[Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster)| Erstellt eine Verbindung mit einem Service Fabric-Cluster |
|[Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage) | Kopiert ein Anwendungspaket in den Clusterimagespeicher  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype)| Registriert einen Anwendungstyp und eine Version auf dem Cluster |
|[New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication)| Erstellt eine Anwendung aus einem registrierten Anwendungstyp |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage) | Entfernt ein Service Fabric-Anwendungspakets aus dem Imagespeicher|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Fabric-PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/service-fabric/overview).

Zusätzliche PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).
