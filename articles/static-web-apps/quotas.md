---
title: Kontingente in Azure Static Web Apps (Vorschauversion)
description: Hier finden Sie Informationen zu Kontingenten im Zusammenhang mit der Vorschauversion von Azure Static Web Apps.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 1bf9a2cf7af8b18994ef2473c11176b8aea74f6e
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102042724"
---
# <a name="quotas-in-azure-static-web-apps-preview"></a>Kontingente in Azure Static Web Apps (Vorschauversion)

Für die Vorschauversion von Azure Static Web Apps gelten folgende Kontingente.

> [!IMPORTANT]
> Azure Static Web Apps befindet sich in der Public Preview-Phase und ist nicht für den Einsatz in der Produktion vorgesehen.

| Funktion                     | Free-Plan        |
|-----------------------------|------------------|
| Enthaltene Bandbreite          | 100 GB pro Monat |
| Zusätzliche Bandbreite           | Nicht verfügbar      |
| Apps pro Azure-Abonnement | 10               |
| App-Größe                    | 250 MB           |
| Präproduktionsumgebungen | 3                |
| Benutzerdefinierte Domänen              | 1                |
| Autorisierung<br><br>Mit benutzerdefinierten Rollen und Routingregeln | Maximal 25 eingeladene Endbenutzer und zugewiesene Rollen |
| Azure-Funktionen             | Verfügbar        |
| SLA                         | Keine             |

## <a name="github-storage"></a>GitHub-Speicher

Von GitHub Actions und Packages wird GitHub-Speicher beansprucht, wofür wiederum eigene Kontingente gelten. Wenn Sie eine Azure Static Web Apps-Website erstellen, werden die Artefakte auch nach der Bereitstellung noch von GitHub gespeichert.

Ausführlichere Informationen finden Sie in den folgenden Ressourcen:

- [Verwalten des Speicherplatzes von Actions](https://github.community/t5/GitHub-Actions/Managing-Actions-storage-space/td-p/38944)
- [Informationen zur Abrechnung für GitHub Actions](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions#about-billing-for-github-actions)
- [Verwalten Ihres Ausgabenlimits für GitHub Actions](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/managing-your-spending-limit-for-github-actions)

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht](overview.md)
