---
title: Kontingente in Azure Static Web Apps
description: Hier finden Sie Informationen zu Kontingenten im Zusammenhang mit Azure Static Web Apps.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 12666d69f6dcab043e909dbb2b49276644a70d76
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110069560"
---
# <a name="quotas-in-azure-static-web-apps"></a>Kontingente in Azure Static Web Apps

Für Azure Static Web Apps gelten folgende Kontingente.

| Funktion                     | Free-Plan        | Standard-Plan |
|-----------------------------|------------------|---------------|
| Enthaltene Bandbreite          | 100 GB pro Monat und Abonnement | 100 GB pro Monat und Abonnement |
| Zusätzliche Bandbreite           | Nicht verfügbar      | 0,20 USD pro GB |
| Apps pro Azure-Abonnement | 10               | Unbegrenzt |
| App-Größe                    | 250 MB           | 500 MB |
| Präproduktionsumgebungen | 3                | 10 |
| Benutzerdefinierte Domänen              | 2 pro App        | 5 pro App |
| Autorisierung (mit benutzerdefinierten Rollen und Routingregeln) | Maximal 25 Endbenutzer, die möglicherweise zu benutzerdefinierten Rollen gehören | Maximal 25 Endbenutzer, die möglicherweise zu benutzerdefinierten Rollen gehören |

## <a name="github-storage"></a>GitHub-Speicher

Von GitHub Actions und Packages wird GitHub-Speicher beansprucht, wofür wiederum eigene Kontingente gelten. Wenn Sie eine Azure Static Web Apps-Website erstellen, werden die Artefakte auch nach der Bereitstellung noch von GitHub gespeichert.

Ausführlichere Informationen finden Sie in den folgenden Ressourcen:

- [Verwalten des Speicherplatzes von Actions](https://github.community/t5/GitHub-Actions/Managing-Actions-storage-space/td-p/38944)
- [Informationen zur Abrechnung für GitHub Actions](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/about-billing-for-github-actions#about-billing-for-github-actions)
- [Verwalten Ihres Ausgabenlimits für GitHub Actions](https://help.github.com/github/setting-up-and-managing-billing-and-payments-on-github/managing-your-spending-limit-for-github-actions)

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht](overview.md)
