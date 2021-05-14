---
title: Verwalten von Gruppen für den kommerziellen Marketplace – Azure Marketplace
description: Hier erfahren Sie, wie Sie Gruppen im Programm für den kommerziellen Marketplace in Partner Center verwalten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: varsha-sarah
ms.author: vavargh
ms.custom: contperf-fy21q2
ms.date: 04/07/2021
ms.openlocfilehash: aa75082142aa5735350cd7fed30194314b6e7368
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2021
ms.locfileid: "107107978"
---
# <a name="manage-groups-for-the-commercial-marketplace"></a>Verwalten von Gruppen für den kommerziellen Marketplace

Mithilfe von Gruppen können Sie mehrere Benutzerrollen und Berechtigungen gleichzeitig verwalten.

## <a name="add-an-existing-group"></a>Hinzufügen einer vorhandenen Gruppe

So fügen Sie Ihrem Partner Center-Konto eine Gruppe hinzu, die bereits im Geschäftskonto (Azure AD-Mandant) Ihrer Organisation vorhanden ist

1. Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) die Option **Gruppen hinzufügen** aus.
1. Wählen Sie eine oder mehrere Gruppen aus der angezeigten Liste aus. Über das Suchfeld können Sie bestimmte Gruppen suchen. Wenn Sie mehrere Gruppen auswählen, die Ihrem Partner Center-Konto hinzugefügt werden sollen, müssen Sie diesen dieselbe Rolle oder denselben Satz benutzerdefinierter Berechtigungen zuweisen. Um mehrere Gruppen mit unterschiedlichen Rollen/Berechtigungen hinzuzufügen, wiederholen Sie diese Schritte für jede Rolle bzw. für jeden Satz benutzerdefinierter Berechtigungen.
1. Wenn Sie die Auswahl von Gruppen abgeschlossen haben, wählen Sie **Ausgewählte hinzufügen** aus.
1. Geben Sie im Abschnitt **Rollen** die Rolle(n) oder die benutzerdefinierten Berechtigungen für die ausgewählten Gruppen an. Alle Mitglieder der Gruppe erhalten Zugriff auf Ihr Partner Center-Konto mit den Berechtigungen, die Sie der Gruppe gewähren, unabhängig von den Rollen und Berechtigungen, die ihren jeweiligen einzelnen Konten zugewiesen sind.
1. Wählen Sie **Speichern** aus.

Wenn Sie eine vorhandene Gruppe hinzufügen, kann jeder Benutzer, der Mitglied dieser Gruppe ist, mit den Berechtigungen auf Ihr Partner Center-Konto zugreifen, die der zugewiesenen Rolle der Gruppe zugeordnet sind.

## <a name="add-a-new-group"></a>Neue Gruppe hinzufügen

So fügen Sie Ihrem Partner Center-Konto eine neue Gruppe hinzu

1. Wählen Sie auf der Seite **Benutzer** (unter **Kontoeinstellungen**) die Option **Gruppen hinzufügen** aus.
1. Wählen Sie auf der nächsten Seite **Neue Gruppe** aus.
1. Geben Sie den Anzeigenamen für die neue Gruppe ein.
1. Geben Sie die Rolle(n) oder die benutzerdefinierten Berechtigungen für die Gruppe an. Alle Mitglieder der Gruppe erhalten Zugriff auf Ihr Partner Center-Konto mit den Berechtigungen, die Sie hier gewähren, unabhängig von den Rollen und Berechtigungen, die ihren jeweiligen einzelnen Konten zugewiesen sind.
1. Wählen Sie aus der angezeigten Liste Benutzer für die neue Gruppe aus. Über das Suchfeld können Sie bestimmte Benutzer suchen.
1. Wenn Sie alle Benutzer ausgewählt haben, wählen Sie **Ausgewählte hinzufügen** aus, um sie der neuen Gruppe hinzuzufügen.
1. Wählen Sie **Speichern** aus.

Diese neue Gruppe wird im Geschäftskonto (Azure AD-Mandant) Ihrer Organisation erstellt und nicht nur in Ihrem Partner Center-Konto.

## <a name="remove-a-group"></a>Entfernen einer Gruppe

So entfernen Sie eine Gruppe aus Ihrem Arbeitskonto (Azure AD-Mandant):
1. Wechseln Sie zu **Benutzer** (unter **Kontoeinstellungen**).
1. Wählen Sie mithilfe des Kontrollkästchens in der Spalte ganz rechts die Gruppe aus, die Sie entfernen möchten.
1. Wählen Sie aus den verfügbaren Aktionen **Entfernen** aus. In einem Popupfenster können Sie bestätigen, dass Sie die ausgewählten Gruppen entfernen möchten.
