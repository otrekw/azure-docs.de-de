---
title: Umwandlung von Azure Monitor-Ansichts-Designer in Arbeitsmappen – Zusammenfassung und Zugriff
description: ''
author: austonli
ms.author: aul
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: e49b4c0220b6c00fe3a7a6e1cab219364a2a89b7
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77170611"
---
# <a name="view-designer-to-workbooks-conversion-summary-and-access"></a>Umwandlung von Ansichts-Designer in Arbeitsmappen – Zusammenfassung und Zugriff
Der [Ansicht-Designer](view-designer.md) ist eine Funktion in Azure Monitor, mit der Sie verschiedene benutzerdefinierten Ansichten erstellen können, die Ihnen bei der Visualisierung von Daten in Ihrem Log Analytics-Arbeitsbereich mittels Diagrammen, Listen und Zeitachsen helfen. Sie laufen aus und werden durch Arbeitsmappen ersetzt, die zusätzliche Funktionalität bereitstellen. In diesem Artikel erfahren Sie, wie Sie eine Übersichtszusammenfassung und Berechtigungen erstellen können, die für den Zugriff auf Arbeitsmappen erforderlich sind.

## <a name="creating-your-workspace-summary-from-azure-dashboard"></a>Erstellen Ihre Arbeitsbereichszusammenfassung über das Azure-Dashboard
Benutzer des Ansicht-Designers sind möglicherweise mit einer Übersichtskachel vertraut, die der Darstellung einer Gruppe von Ansichten dient. Um eine visuelle Übersicht zu erhalten, wie z. B. die Arbeitsbereichszusammenfassung des Ansicht-Designers, bieten Arbeitsmappen angeheftete Schritte, die Sie an Ihr [Azure-Portal-Dashboard](../../azure-portal/azure-portal-dashboards.md) anheften können. Genau wie die Übersichtskacheln in der Arbeitsbereichszusammenfassung, sind angeheftete Arbeitsmappenelemente direkt mit der Arbeitsmappenansicht verknüpft.

Sie können den hohen Grad an Anpassungsfunktionen von Azure-Dashboards nutzen, was die automatische Aktualisierung, das Verschieben, die Größenanpassung und das zusätzliche Filtern nach Ihren angehefteten Elementen und Visualisierungen ermöglicht. 

![Dashboard](media/view-designer-conversion-access/dashboard.png)

Erstellen Sie ein neues Azure-Dashboard, oder wählen Sie ein vorhandenes Dashboard aus, um mit dem Anheften von Arbeitsmappen zu beginnen.

Zum Anheften einzelner Elemente müssen Sie das „Anheften“-Symbol für den jeweiligen Schritt aktivieren. Wählen Sie hierzu die entsprechende Schaltfläche **Bearbeiten** für Ihren Schritt aus, und wählen Sie dann das Zahnradsymbol aus, um **Erweiterte Einstellungen** zu öffnen. Aktivieren Sie die Option **Immer Stecknadelsymbol für diesen Schritt anzeigen**, woraufhin in der oberen rechten Ecke Ihres Schritts ein Stecknadelsymbol zum Anheften angezeigt wird. Dieses Stecknadelsymbol ermöglicht es Ihnen, bestimmte Visualisierungen an Ihr Dashboard anzuheften, wie z. B. die Übersichtskacheln.

![Schritt anheften](media/view-designer-conversion-access/pin-step.png)


Möglicherweise möchten Sie auch mehrere Visualisierungen aus der Arbeitsmappe oder den gesamten Arbeitsmappeninhalt an ein Dashboard anheften. Um die gesamte Arbeitsmappe anzuheften, wählen Sie in der oberen Symbolleiste **Bearbeiten** aus, um den **Bearbeitungsmodus** umzuschalten. Ein Stecknadelsymbol wird angezeigt, mit dem Sie entweder das gesamte Arbeitsmappenelement oder jeden der einzelnen Schritte und Visualisierungen in der Arbeitsmappe anheften können.

![Alles anheften](media/view-designer-conversion-access/pin-all.png)



## <a name="sharing-and-viewing-permissions"></a>Freigeben und Berechtigungen zum Anzeigen 
Arbeitsmappen haben den Vorteil, dass sie entweder ein privates oder ein freigegebenes Dokument sind. Standardmäßig werden Arbeitsmappen unter **Meine Berichte** gespeichert, was bedeutet, dass nur der Ersteller diese Arbeitsmappe anzeigen kann.

Sie können Ihre Arbeitsmappen freigeben, indem Sie das Symbol **Freigeben** in der oberen Symbolleiste auswählen, während Sie im **Bearbeitungsmodus** sind. Sie werden aufgefordert, Ihre Arbeitsmappe in **Freigegebene Berichte** zu verschieben, wodurch ein Link generiert wird, der den direkten Zugriff auf die Arbeitsmappe bereitstellt.

Damit ein Benutzer eine freigegebene Arbeitsmappe anzeigen kann, benötigt er Zugriff sowohl auf das Abonnement als auch auf die Ressourcengruppe, unter der die Arbeitsmappe gespeichert ist.

![Abonnementbasierter Zugriff](media/view-designer-conversion-access/subscription-access.png)

## <a name="next-steps"></a>Nächste Schritte

- [Häufige Aufgaben](view-designer-conversion-tasks.md)