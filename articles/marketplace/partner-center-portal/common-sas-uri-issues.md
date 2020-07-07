---
title: 'Häufige SAS-URI-Probleme und ihre Behebung: Azure Marketplace'
description: Enthält eine Beschreibung der häufigen Probleme und empfohlenen Lösungen bei der Arbeit mit Shared Access Signatures.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2020
ms.openlocfilehash: 56db3562efdc0406e745fd38b73df0a473d0ecd5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83724597"
---
# <a name="common-sas-uri-issues-and-fixes"></a>Häufige SAS-URI-Probleme und ihre Behebung

Unten sind einige häufige Probleme im Zusammenhang mit der Verwendung von Shared Access Signatures sowie Lösungsvorschläge aufgeführt. Shared Access Signatures dienen zur Identifizierung und gemeinsamen Verwendung der hochgeladenen VHDs für Ihre Lösung.

| **Problem** | **Fehlermeldung** | **Behebung** |
| --------- | ------------------- | ------- |
| *Fehler beim Kopieren von Images* |  |  |
| „?“ wurde im SAS-URI nicht gefunden. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aktualisieren Sie den SAS-URI mit den empfohlenen Tools. |
| Die Parameter „st“ und „se“ sind im SAS-URI nicht enthalten. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aktualisieren Sie den SAS-URI mit geeigneten Werten für **Startdatum** und **Enddatum**. |
| „sp=rl“ ist im SAS-URI nicht enthalten. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Aktualisieren Sie den SAS-URI mit den Berechtigungen `Read` und `List`. |
| Der VHD-Name im SAS-URI enthält Leerzeichen. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Entfernen Sie die Leerzeichen aus dem SAS-URI. |
| SAS-URI-Autorisierungsfehler | `Failure: Copying Images. Not able to download blob due to authorization error.` | Überprüfen und korrigieren Sie das SAS-URI-Format. Generieren Sie ihn bei Bedarf neu. |
| Die Parameter „st“ und „se“ des SAS-URI verfügen nicht über eine vollständige Angabe für Datum und Uhrzeit. | `Failure: Copying Images. Not able to download blob due to incorrect SAS Uri.` | Für den SAS-URI muss für die Parameter **Startdatum** und **Enddatum** (Teilzeichenfolgen `st` und `se`) das vollständige Format mit Datum und Uhrzeit verwendet werden, z. B. `11-02-2017T00:00:00Z`. Gekürzte Versionen sind ungültig (bei einigen Azure CLI-Befehlen werden standardmäßig ggf. gekürzte Werte generiert). |
|  |  |  |

Ausführlichere Informationen finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).
