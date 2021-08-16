---
title: Untersuchen von Vorfällen mit UEBA-Daten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie bei Untersuchungen mithilfe von UEBA-Daten einen größeren Kontext für potenziell schädliche Aktivitäten in Ihrer Organisation erhalten.
services: sentinel
documentationcenter: na
author: batamig
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2021
ms.author: bagol
ms.openlocfilehash: 1a3f199276d5c9b04ab5ac117c022a63fd3fb866
ms.sourcegitcommit: 19dfdfa85e92c6a34933bdd54a7c94e8b00eacfd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2021
ms.locfileid: "109665304"
---
# <a name="investigate-incidents-with-ueba-data"></a>Untersuchen von Vorfällen mit UEBA-Daten

In diesem Artikel werden allgemeine Methoden und Beispielverfahren für die Verwendung der [Benutzer- und Entitätsverhaltensanalyse (User and Entity Behavior Analytics, UEBA)](identify-threats-with-entity-behavior-analytics.md) in Ihren regulären Untersuchungsworkflows beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie UEBA-Daten in Ihren Untersuchungen verwenden können, müssen Sie die [Benutzer- und Entitätsverhaltensanalyse (User and Entity Behavior Analytics, UEBA) in Azure Sentinel aktivieren](enable-entity-behavior-analytics.md). 

Etwa eine Woche nach dem Aktivieren von UEBA können Sie mit der Suche nach computergestützten Erkenntnissen beginnen.

## <a name="run-proactive-routine-searches-in-entity-data"></a>Ausführen proaktiver Routinesuchvorgänge in Entitätsdaten

Wir empfehlen, regelmäßig proaktive Suchvorgänge für Benutzeraktivitäten auszuführen, um Leads für weitere Untersuchungen zu erstellen.

Mithilfe der Azure Sentinel-Arbeitsmappe [Benutzer- und Entitätsverhaltensanalyse (UEBA)](identify-threats-with-entity-behavior-analytics.md#hunting-queries-and-exploration-queries) können Sie zum Beispiel Daten für folgende Bereiche abfragen:

- **Benutzer mit dem höchsten Risiko**, mit Anomalien oder angefügten Vorfällen
- **Daten zu bestimmten Benutzern**, um zu ermitteln, ob die Person tatsächlich kompromittiert wurde oder ob eine Insiderbedrohung aufgrund einer vom Benutzerprofil abweichenden Aktion besteht.

Erfassen Sie darüber hinaus in der UEBA-Arbeitsmappe nicht routinemäßige Aktionen, und verwenden Sie diese, um nach anomalen Aktivitäten und potenziell nicht konformen Methoden zu suchen.

### <a name="investigate-an-anomalous-sign-in"></a>Untersuchen einer anomalen Anmeldung

Mit den folgenden Schritten führen Sie beispielsweise eine Untersuchung für einen Benutzer aus, der eine Verbindung mit einem zuvor noch nie genutzten VPN hergestellt hat, was eine anomale Aktivität darstellt.

1. Suchen Sie im Azure Sentinel-Bereich **Arbeitsmappen** die Arbeitsmappe **Benutzer- und Entitätsverhaltensanalyse (UEBA)** , und öffnen Sie diese.
1. Suchen Sie nach einem bestimmten Benutzernamen, der untersucht werden soll, und wählen Sie den Namen in der Tabelle **Wichtigste zu untersuchende Benutzer** aus.
1. Scrollen Sie in den Tabellen **Aufschlüsselung von Vorfällen** und **Aufschlüsselung von Anomalien** nach unten, um die Vorfälle und Anomalien anzuzeigen, die mit dem ausgewählten Benutzer verbunden sind.
1. Überprüfen Sie für die jeweilige Anomalie (z. B. **Anomale erfolgreiche Anmeldung**) die in der Tabelle angezeigten Details, um diese zu untersuchen. Beispiel:

    |Schritt  |Beschreibung  |
    |---------|---------|
    |**Beachten Sie die Beschreibung auf der rechten Seite**     |    Jede Anomalie enthält eine Beschreibung mit einem Link, über den Sie weitere Informationen aus der [MITRE ATT&CK-Wissensdatenbank ](https://attack.mitre.org/) abrufen können. <br>Beispiel: <br><br>        **_Initial Access_* _ <br>_The adversary is trying to get into your network.* (Erstzugang_Der Angreifer versucht, in Ihr Netzwerk zu gelangen) <br>* „Initial Access“ (Erstzugang) besteht aus Techniken, die verschiedene Eintrittsvektoren nutzen, um in einem Netzwerk Fuß zu fassen. Zu den Techniken, die verwendet werden, um ins Netzwerk zu gelangen, gehören gezieltes Spear-Phishing und das Ausnutzen von Schwachstellen auf öffentlich zugänglichen Webservern. Ein durch den Erstzugang erworbener Netzwerkzugriff ermöglicht ggf. den fortgesetzten Zugriff (z. B. gültige Konten und die Verwendung externer Remotedienste) oder kann durch das Ändern von Kennwörtern eingeschränkt werden.*     |
    |**Beachten Sie den Text in der Spalte „Beschreibung“**     |   Scrollen Sie in der Zeile „Anomalie“ nach rechts, um eine zusätzliche Beschreibung anzuzeigen. Wählen Sie den Link aus, um den vollständigen Text anzuzeigen. Beispiel: <br><br> *Angreifer stehlen ggf. die Anmeldeinformationen eines bestimmten Benutzers oder Dienstkontos, indem sie Techniken für den Zugriff auf Anmeldeinformationen verwenden oder Anmeldeinformationen vorher bei einem Erkundungsvorgang durch Social Engineering erfassen, um den Erstzugang zu erhalten. APT33 beispielsweise verwendet gültige Konten für den Erstzugang. Mit der folgenden Abfrage wird die Ausgabe einer erfolgreichen Anmeldung generiert, die von einem Benutzer von einem neuen geografischen Standort aus durchgeführt wurde, von dem er und seine Kollegen noch nie eine Verbindung hergestellt haben.*     |
    |**Beachten Sie die UsersInsights-Daten**     |  Scrollen Sie in der Zeile „Anomalie“ weiter nach rechts, um die UserInsights-Daten (z. B. Anzeigename des Kontos und Kontoobjekt-ID) anzuzeigen. Wählen Sie den Text aus, um die vollständigen Daten auf der rechten Seite anzuzeigen.         |
    |**Beachten Sie die Beweisdaten**     |  Scrollen Sie in der Zeile „Anomalie“ weiter nach rechts, um die Beweisdaten für die Anomalie anzuzeigen. Wählen Sie den Text aus, um die vollständigen Daten auf der rechten Seite anzuzeigen, z. B. die folgenden Felder: <br><br>-  **ActionUncommonlyPerformedByUser** <br>- **UncommonHighVolumeOfActions** <br>- **FirstTimeUserConnectedFromCountry** <br>- **CountryUncommonlyConnectedFromAmongPeers** <br>- **FirstTimeUserConnectedViaISP** <br>- **ISPUncommonlyUsedAmongPeers** <br>- **CountryUncommonlyConnectedFromInTenant** <br>- **ISPUncommonlyUsedInTenant** |
    |     |         |

Ermitteln Sie mithilfe der Daten in der Arbeitsmappe **Benutzer- und Entitätsverhaltensanalyse (UEBA)** , ob die Benutzeraktivität verdächtig ist und weitere Maßnahmen erfordert.

## <a name="use-ueba-data-to-analyze-false-positives"></a>Verwenden von UEBA-Daten zum Analysieren falsch positiver Ergebnisse

Manchmal ist ein in einer Untersuchung erfasster Vorfall falsch positiv.

Ein häufiges Beispiel für falsch positive Ergebnisse sind Reiseaktivitäten, die als unmöglich erkannt werden (z. B. bei einem Benutzer, der sich innerhalb einer Stunde bei einer Anwendung oder einem Portal in New York und London angemeldet). Auch wenn Azure Sentinel den unmöglichen Ortswechsel als Anomalie erkennt, kann eine Untersuchung des Benutzers klären, dass ein VPN mit einem anderen Standort als dem tatsächlichen Standort des Benutzers verwendet wurde.

### <a name="analyze-a-false-positive"></a>Analysieren eines falsch positiven Ergebnisses

Navigieren Sie beispielsweise bei einem Vorfall des Typs **Unmöglicher Ortswechsel**, nachdem Sie mit dem Benutzer geklärt haben, dass ein VPN verwendet wurde, von der Vorfallsseite zur Benutzerentitätsseite. Verwenden Sie die dort angezeigten Daten, um zu ermitteln, ob die erfassten Standorte in den allgemein bekannten Standorten des Benutzers enthalten sind.

Beispiel:

[ ![Öffnen Sie die Benutzerentitätsseite für einen Vorfall.](media/ueba/open-entity-pages.png) ](media/ueba/open-entity-pages.png#lightbox)

Die Benutzerentitätsseite ist auch mit der [Vorfallsseite](tutorial-investigate-cases.md#how-to-investigate-incidents) selbst und dem [Untersuchungsdiagramm](tutorial-investigate-cases.md#use-the-investigation-graph-to-deep-dive) verknüpft.

> [!TIP]
> Nachdem Sie die Daten auf der Benutzerentitätsseite für den speziellen Benutzer bestätigt haben, der dem Vorfall zugeordnet ist, wechseln Sie zum Azure Sentinel-Bereich **Hunting**, um zu erfahren, ob auch die Kollegen des Benutzers in der Regel von den gleichen Standorten aus eine Verbindung herstellen. Wenn das der Fall ist, wäre dieses Wissen ein noch stärkerer Hinweis auf ein falsch positives Ergebnis.
>
> Führen Sie im Bereich **Hunting** die Abfrage **Anmeldung von anomalen geografischen Standorten** aus. Weitere Informationen finden Sie unter [Suchen nach Bedrohungen mit Azure Sentinel](hunting.md).
>

## <a name="identify-password-spray-and-spear-phishing-attempts"></a>Identifizieren von Kennwortspray- und Spear-Phishing-Versuchen

Wenn die mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) nicht aktiviert ist, sind Benutzeranmeldeinformationen anfällig für Angreifer, die mit [Kennwortspray-](https://www.microsoft.com/security/blog/2020/04/23/protecting-organization-password-spray-attacks/) oder [Spear-Phishing-Versuchen](https://www.microsoft.com/security/blog/2019/12/02/spear-phishing-campaigns-sharper-than-you-think/) Angriffe zur Kennwortkompromittierung durchführen möchten.

### <a name="investigate-a-password-spray-incident-with-ueba-insights"></a>Untersuchen eines Kennwortspray-Vorfalls mit UEBA-Erkenntnissen

Wenn Sie beispielsweise einen Kennwortspray-Vorfall mit UEBA-Erkenntnissen untersuchen möchten, könnten Sie folgende Schritte ausführen, um mehr zu erfahren:

1. Wählen Sie im Vorfall unten links die Option **Untersuchen** aus, um die Konten, Computer und anderen Datenpunkte anzuzeigen, die möglicherweise Ziel eines Angriffs waren.

    Beim Durchsuchen der Daten wird möglicherweise ein Administratorkonto mit einer relativ großen Anzahl von Anmeldefehlern angezeigt. Auch wenn dies verdächtig ist, möchten Sie das Konto möglicherweise nicht ohne weitere Bestätigung einschränken.

1. Wählen Sie in der Zuordnung die Entität des Administratorbenutzers und dann auf der rechten Seite **Erkenntnisse** aus, um nach weiteren Details (z. B. Diagramm der Anmeldungen im Zeitverlauf) zu suchen.

1. Wählen Sie auf der rechten Seite **Info** und dann **Vollständige Details anzeigen** aus, um zur [Benutzerentitätsseite](identify-threats-with-entity-behavior-analytics.md#entity-pages) zu wechseln und einen weiteren Drilldown auszuführen. 

    Achten Sie beispielsweise darauf, ob dies der erste potenzielle Kennwortspray-Vorfall des Benutzers ist, oder beobachten Sie den Anmeldeverlauf des Benutzers, um zu erkennen, ob die Fehler anomal waren.

> [!TIP]
> Sie können auch die [Hunting-Abfrage](hunting.md) **Anomale fehlgeschlagene Anmeldung** ausführen, um alle ungewöhnlichen fehlgeschlagenen Anmeldungen in einer Organisation zu überwachen. Verwenden Sie die Ergebnisse der Abfrage, um Untersuchungen zu möglichen Kennwortspray-Angriffen zu starten.
>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu UEBA, Untersuchungen und Hunting finden Sie hier:

- [Erkennen komplexerer Bedrohungen mit User and Entity Behavior Analytics (UEBA) in Azure Sentinel](identify-threats-with-entity-behavior-analytics.md)
- [Referenz zu Azure Sentinel-UEBA-Anreicherungen](ueba-enrichments.md)
- [Tutorial: Untersuchen von Vorfällen mit Azure Sentinel](tutorial-investigate-cases.md)
- [Suchen nach Bedrohungen mit Azure Sentinel](hunting.md)
