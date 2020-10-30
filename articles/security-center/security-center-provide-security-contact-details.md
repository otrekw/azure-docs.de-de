---
title: Konfigurieren von E-Mail-Benachrichtigungen für Azure Security Center-Warnungen
description: Hier erfahren Sie, wie Sie die von Azure Security Center gesendeten E-Mail-Typen für Sicherheitswarnungen optimieren.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2020
ms.author: memildin
ms.openlocfilehash: 6b54f02b39e233dcf35f0d18682ca102883d76c3
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791867"
---
# <a name="configure-email-notifications-for-security-alerts"></a>Konfigurieren von E-Mail-Benachrichtigungen für Sicherheitswarnungen 

Sicherheitswarnungen müssen die richtigen Personen in Ihrer Organisation erreichen. Standardmäßig sendet Security Center E-Mails an Abonnementbesitzer, wenn für ein Abonnement eine Warnung mit hohem Schweregrad ausgelöst wird. Auf dieser Seite wird erläutert, wie diese Benachrichtigungen angepasst werden.

Wenn Sie eigene Einstellungen für Benachrichtigungs-E-Mails definieren möchten, können Sie auf der Seite mit Einstellungen für **E-Mail-Benachrichtigungen** von Azure Security Center Folgendes auswählen:

- **_Wer_ soll benachrichtigt werden:** E-Mails können an ausgewählte Einzelpersonen oder an Personen mit einer bestimmten Azure-Rolle für ein Abonnement gesendet werden. 
- **_Worüber_ soll eine Person benachrichtigt werden:** Ändern Sie die Schweregrade, für die Security Center Benachrichtigungen senden soll.

Zur Vermeidung von Warnungsmüdigkeit wird die Menge der ausgehenden E-Mails von Security Center begrenzt. Für jedes Abonnement wird von Security Center Folgendes gesendet:

- Maximal **vier** E-Mails pro Tag für Warnungen mit **hohem Schweregrad**
- Maximal **zwei** E-Mails pro Tag für Warnungen mit **mittlerem Schweregrad**
- Maximal **eine** E-Mail pro Tag für Warnungen mit **niedrigem Schweregrad**

:::image type="content" source="./media/security-center-provide-security-contacts/email-notification-settings.png" alt-text="Konfigurieren der Details des Kontakts, der E-Mails zu Sicherheitswarnungen erhalten soll." :::
 
## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar (Generally Available, GA)|
|Preise:|Kostenlos|
|Erforderliche Rollen und Berechtigungen:|**Sicherheitsadministrator**<br>**Besitzer des Abonnements** |
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)|
|||


## <a name="customize-the-security-alerts-email-notifications"></a>Anpassen der E-Mail-Benachrichtigungen für Sicherheitswarnungen<a name="email"></a>

Sie können E-Mail-Benachrichtigungen an Einzelpersonen oder an alle Benutzer mit bestimmten Azure-Rollen senden.

1. Wählen Sie in Security Center im Bereich **Preise und Einstellungen** das entsprechende Abonnement und dann die Option **E-Mail-Benachrichtigungen** aus.

1. Definieren Sie die Empfänger für Ihre Benachrichtigungen mit einer der folgenden oder beiden Optionen:

    - Wählen Sie in der Dropdownliste eine der verfügbaren Rollen aus.
    - Geben Sie bestimmte E-Mail-Adressen durch Kommas getrennt ein. Sie können beliebig viele E-Mail-Adressen eingeben.

1. Wählen Sie **Speichern** aus, um die Sicherheitskontaktinformationen für Ihr Abonnement zu übernehmen.


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Sicherheitswarnungen finden Sie auf den folgenden Seiten:

- [Sicherheitswarnungen (Referenzhandbuch)](alerts-reference.md): Enthält Informationen zu den Sicherheitswarnungen, die im Threat Protection-Modul von Azure Security Center ggf. angezeigt werden.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier wird beschrieben, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Automatisieren von Reaktionen auf Security Center-Auslöser](workflow-automation.md): Verwenden der Automatisierung, um auf Security Center-Auslöser mit benutzerdefinierter Benachrichtigungslogik zu reagieren