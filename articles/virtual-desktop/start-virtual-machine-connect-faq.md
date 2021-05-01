---
title: 'Windows Virtual Desktop: FAQ zu „VM bei Verbindung starten“: Azure'
description: Häufig gestellte Fragen und bewährte Methoden zur Verwendung des Features „VM bei Verbindung starten“.
author: Heidilohr
ms.topic: conceptual
ms.date: 03/31/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 0a35d0541358af2f5fac5e4c7486a1be93797922
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106445583"
---
# <a name="start-vm-on-connect-faq-preview"></a>FAQ zu „VM bei Verbindung starten“ (Vorschau)

> [!IMPORTANT]
> Das Feature „VM bei Verbindung starten“ befindet sich derzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Dieser Artikel behandelt häufig gestellte Fragen zum Feature „VM bei Verbindung starten“ (Vorschau) für Windows Virtual Desktop-Hostpools.

## <a name="are-vms-automatically-deallocated-when-a-user-stops-using-them"></a>Wird die Zuordnung der virtuellen Computer automatisch aufgehoben, wenn ein Benutzer deren Verwendung beendet?

Nein. Sie müssen zusätzliche Richtlinien konfigurieren, um Benutzer von ihren Sitzungen abzumelden, und Azure-Automatisierungsskripts ausführen, um die Zuordnung von VMs aufzuheben.

So konfigurieren Sie die Richtlinie zum Aufheben der Zuordnung:

1. Stellen Sie eine Remoteverbindung mit der VM her, für die Sie die Richtlinie festlegen möchten.

2. Öffnen Sie den lokalen **Gruppenrichtlinien-Editor**, und navigieren Sie zu **Richtlinie für lokalen Computer** > **Computerkonfiguration** > **Administrative Vorlagen** > **Windows-Komponenten** > **Remotedesktopdienste** > **Remotedesktop-Sitzungshost** > **Sitzungszeitlimits**.

3. Suchen Sie nach der Richtlinie **Zeitlimit für getrennte Sitzungen festlegen**, und ändern Sie deren Wert in **Aktiviert**.

4. Nachdem Sie die Richtlinie aktiviert haben, wählen Sie **Getrennte Sitzung beenden** aus.

>[!NOTE]
>Stellen Sie sicher, dass das Zeitlimit für die Richtlinie „Getrennte Sitzung beenden“ auf einen höheren Wert als fünf Minuten festgelegt ist. Ein niedriges Zeitlimit kann dazu führen, dass Sitzungen von Benutzern beendet werden, wenn Ihr Netzwerk die Verbindung zu lange verliert, was zu einem Arbeitsverlust führen kann.

Wenn Benutzer sich abmelden, wird die Zuordnung ihrer VMs nicht aufgehoben. Weitere Informationen zum Aufheben der Zuordnung von VMs finden Sie unter [VMs außerhalb der Geschäftszeiten starten/beenden – Übersicht](../automation/automation-solution-vm-management.md).

## <a name="can-users-turn-off-the-vm-from-their-clients"></a>Können Benutzer die VM von ihren Clients aus ausschalten?

Ja. Benutzer können den virtuellen Computer während der Sitzung wie einen physischen Computer im Startmenü herunterfahren. Wenn sie die VM herunterfahren, wird deren Zuordnung jedoch nicht aufgehoben. Weitere Informationen zum Aufheben der Zuordnung von VMs finden Sie unter [VMs außerhalb der Geschäftszeiten starten/beenden – Übersicht](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren von „VM bei Verbindung starten“ finden Sie unter [VM bei Verbindung starten (Vorschau)](start-virtual-machine-connect.md).

Antworten auf allgemeinere Fragen zu Windows Virtual Desktop finden Sie in diesem [FAQ](faq.md)-Artikel.
