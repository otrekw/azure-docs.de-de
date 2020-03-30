---
title: Anwenden von Systemupdates in Azure Security Center | Microsoft Docs
description: In diesem Dokument erfahren Sie, wie Sie die Azure Security Center-Empfehlungen **Systemupdates anwenden** und **Neustart nach Systemupdates** implementieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 3f27753b0775f44cbdf9d4c478a19e423b8e1f19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604558"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Anwenden von Systemupdates in Azure Security Center
Azure Security Center überprüft virtuelle Computer (Virtual Machines, VMs) und physische Computer unter Windows und Linux täglich auf fehlende Betriebssystemupdates. Security Center ruft eine Liste mit verfügbaren Sicherheitsupdates und wichtigen Updates von Windows Update oder WSUS (Windows Server Update Services) ab – je nachdem, welcher Dienst für einen Windows-Computer konfiguriert ist. Darüber hinaus prüft Security Center auch die neuesten Updates für Linux-Systeme. Falls auf Ihrem virtuellen oder physischen Computer ein Systemupdate fehlt, empfiehlt Security Center die Anwendung von Systemupdates.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung
„Systemupdates anwenden“ wird als Empfehlung in Security Center präsentiert. Wenn auf Ihrem virtuellen oder physischen Computer ein Systemupdate fehlt, wird diese Empfehlung unter **Empfehlungen** und unter **Compute** angezeigt.  Wenn Sie auf die Empfehlung klicken, wird das Dashboard **Systemupdates anwenden** geöffnet.

In diesem Beispiel verwenden wir **Compute**.

1. Klicken Sie im Hauptmenü von Security Center auf **Compute**.

   ![Auswählen von „Compute“][1]

2. Klicken Sie unter **Compute** auf **Fehlende Systemupdates**. Das Dashboard **Systemupdates anwenden** wird geöffnet.

   ![Dashboard „Systemupdates anwenden“][2]

   Im oberen Bereich des Dashboards befindet sich Folgendes:

    - Die Gesamtanzahl virtueller und physischer Computer unter Windows und Linux, auf denen Systemupdates fehlen.
    - Die Gesamtanzahl wichtiger Updates, die auf Ihren virtuellen und physischen Computern fehlen.
    - Die Gesamtanzahl von Sicherheitsupdates, die auf Ihren virtuellen und physischen Computern fehlen.

   Im unteren Bereich des Dashboards werden alle fehlenden Updates für Ihre virtuellen und physischen Computer sowie der Schweregrad des fehlenden Updates aufgeführt.  Die Liste enthält Folgendes:

    - NAME: Der Name des fehlenden Updates.
    - ANZAHL VON VMS UND COMPUTERN: Die Gesamtzahl virtueller und physischer Computer, auf denen dieses Update fehlt.
    - STATUS: Der aktuelle Status der Empfehlung:

      - Offen: Die Empfehlung wurde noch nicht umgesetzt.
      - In Bearbeitung: Die Empfehlung wird derzeit auf die Ressourcen angewendet, und es ist keine Aktion Ihrerseits erforderlich.
      - Behoben: Die Empfehlung wurde bereits umgesetzt. (Wenn das Problem behoben wurde, wird der Eintrag abgeblendet dargestellt.)

    - SCHWEREGRAD: Beschreibt den Schweregrad der jeweiligen Empfehlung:

      - Hoch: Ein Sicherheitsrisiko betrifft eine wichtige Ressource (Anwendung, virtueller Computer oder Netzwerksicherheitsgruppe) und erfordert eine Aktion.
      - Mittel: Nicht kritische oder zusätzliche Schritte sind erforderlich, um einen Prozess abzuschließen oder ein Sicherheitsrisiko zu beseitigen.
      - Niedrig: Es besteht ein Sicherheitsrisiko, das behandelt werden sollte, aber es ist keine unmittelbare Aktion erforderlich. (Standardmäßig werden Empfehlungen mit dem Status „Niedrig“ nicht angezeigt, aber Sie können bei Bedarf nach diesen Empfehlungen filtern.)

3. Klicken Sie auf ein fehlendes Update in der Liste, um weitere Informationen anzuzeigen.

   ![Fehlendes Sicherheitsupdate][3]

4. Klicken Sie auf dem oberen Menüband auf das Symbol **Suche**.  Eine nach Computern mit dem fehlenden Update gefilterte Suchabfrage aus Azure Monitor-Protokolle wird geöffnet.

   ![Azure Monitor-Protokollsuche][4]

5. Klicken Sie auf einen Computer in der Liste, um weitere Informationen zu erhalten. Ein weiteres Suchergebnis wird geöffnet, das gefilterte Informationen für diesen speziellen Computer enthält.

    ![Azure Monitor-Protokollsuche][5]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](tutorial-security-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
* [Verwalten von Sicherheitsempfehlungen in Azure Security Center:](security-center-recommendations.md) Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center:](security-center-partner-solutions.md) Erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
