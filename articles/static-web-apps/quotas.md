---
title: Kontingente in Azure Static Web Apps (Vorschauversion)
description: Hier finden Sie Informationen zu Kontingenten im Zusammenhang mit der Vorschauversion von Azure Static Web Apps.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 6fa70eba5875c32d41fa1cfc8daf9b1cdf8f19df
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599109"
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
| App-Größe                    | 100 MB           |
| Präproduktionsumgebungen | 1                |
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
