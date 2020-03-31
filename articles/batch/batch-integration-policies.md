---
title: 'Integration in Azure Policy: Azure Batch | Microsoft-Dokumentation'
description: ''
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/24/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 9a306457f838fc79d12be3217d96cc8fb25c9c1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617866"
---
# <a name="integration-with-azure-policy"></a>Integration in Azure Policy

Azure Policy ist ein Dienst in Azure, der Ihnen das Erstellen, Zuweisen und Verwalten von Richtlinien ermöglicht, die unterschiedliche Regeln für Ihre Ressourcen erzwingen, damit diese Ressourcen stets konform mit Ihren Unternehmensstandards und Vereinbarungen zum Service Level bleiben. Mit Azure Policy werden Ihre Ressourcen auf Nichteinhaltung der von Ihnen zugewiesenen Richtlinien überprüft. 

Azure Batch verfügt über zwei integrierte Erweiterungen, die Sie bei der Verwaltung der Konformität von Richtlinien unterstützen. 

|**Name**...|   **Beschreibung**|**Auswirkungen**|  **Version**|    **Quelle**
|----------------|----------|----------|----------------|---------------|
|Diagnoseprotokolle in Batch-Konten sollten aktiviert sein.|   Hiermit wird die Aktivierung von Diagnoseprotokollen überwacht. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn es zu einem Sicherheitsincident kommt oder Ihr Netzwerk gefährdet ist.|AuditIfNotExists, Disabled|  2.0.0|  GitHub|
|Metrikwarnungsregeln sollten in Batch-Konten konfiguriert werden.| Hiermit wird die Konfiguration von Metrikwarnungsregeln für ein Batch-Konto überwacht, um die erforderliche Metrik zu aktivieren.|   AuditIfNotExists, Disabled| 1.0.0|  GitHub|

Richtliniendefinitionen beschreiben die Bedingungen, die erfüllt sein müssen. Eine Bedingung vergleicht die Ressourceneigenschaft mit einem erforderlichen Wert. Der Zugriff auf Ressourceneigenschaftsfelder erfolgt über vordefinierte Aliase. Mit Eigenschaftsaliasen können Sie auf bestimmte Eigenschaften für einen Ressourcentyp zugreifen. Mithilfe von Aliasen können Sie beschränken, welche Werte oder Bedingungen für eine Eigenschaft einer Ressourcen zulässig sind. Jeder Alias wird Pfaden in verschiedenen API-Versionen für einen bestimmten Ressourcentyp zugeordnet. Bei der Richtlinienauswertung ruft das Richtlinienmodul den Eigenschaftenpfad für diese API-Version ab.

Zu den für Batch erforderlichen Ressourcen gehören: Konto, Computeknoten, Pool, Auftrag und Task. Sie würden also Eigenschaftsaliase verwenden, um auf bestimmte Eigenschaften für diese Ressourcen zuzugreifen. Weitere Informationen zu [Aliasen](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

Um sicherzustellen, dass Sie die aktuellen Aliase kennen und ihre Ressourcen und Richtlinien überprüfen, verwenden Sie die Azure-Richtlinienerweiterung für Visual Studio Code. Sie kann auf allen Plattformen installiert werden, die von Visual Studio Code unterstützt werden. Diese Unterstützung umfasst Windows, Linux und macOS. Weitere Informationen finden Sie in den [Installationsleitfäden](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode).



