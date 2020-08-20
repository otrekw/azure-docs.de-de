---
title: Bereitstellen von Sicherheitskontaktinformationen in Azure Security Center | Microsoft Docs
description: In diesem Dokument wird erläutert, wie Sie Sicherheitskontaktinformationen in Azure Security Center bereitstellen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/11/2020
ms.author: memildin
ms.openlocfilehash: c09dca8b5927e417714ef89393950cfb58c89020
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2020
ms.locfileid: "88042274"
---
# <a name="set-up-email-notifications-for-security-alerts"></a>Einrichten von E-Mail-Benachrichtigungen für Sicherheitswarnungen 

Um sicherzustellen, dass die richtigen Personen in Ihrer Organisation über Sicherheitswarnungen in Ihrer Umgebung informiert werden, müssen Sie deren E-Mail-Adressen auf der Seite mit den Einstellungen für **E-Mail-Benachrichtigungen** eingeben.

Beim Einrichten Ihrer Benachrichtigungen können Sie konfigurieren, dass die E-Mails an bestimmte Einzelpersonen oder an alle Personen mit einer bestimmten Azure-Rolle für ein Abonnement gesendet werden. 

Zur Vermeidung von Warnungsmüdigkeit wird die Menge der ausgehenden E-Mails von Security Center begrenzt. Für jedes Abonnement wird von Security Center Folgendes gesendet:

- Maximal **vier** E-Mails pro Tag für Warnungen mit **hohem Schweregrad**
- Maximal **zwei** E-Mails pro Tag für Warnungen mit **mittlerem Schweregrad**
- Maximal **eine** E-Mail pro Tag für Warnungen mit **niedrigem Schweregrad**

## <a name="availability"></a>Verfügbarkeit

|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar|
|Preise:|Free-Tarif|
|Erforderliche Rollen und Berechtigungen:|**Sicherheitsadministrator**<br>**Abonnementbesitzer** |
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) US Gov (teilweise)<br>![Nein](./media/icons/no-icon.png) China Gov/andere Gov|
|||




## <a name="set-up-email-notifications-for-alerts"></a>Einrichten von E-Mail-Benachrichtigungen für Warnungen <a name="email"></a>

Sie können E-Mail-Benachrichtigungen an Einzelpersonen oder an alle Benutzer mit bestimmten Azure-Rollen senden.

1. Wählen Sie in Security Center im Bereich **Preise und Einstellungen** das entsprechende Abonnement und dann die Option **E-Mail-Benachrichtigungen** aus.

1. Definieren Sie die Empfänger für Ihre Benachrichtigungen:

    - Wählen Sie in der Dropdownliste eine der verfügbaren Rollen aus.
    - Sie können auch bestimmte E-Mail-Adressen durch Kommas getrennt eingeben. Sie können beliebig viele E-Mail-Adressen eingeben.

1. Wählen Sie **Speichern** aus, um die Sicherheitskontaktinformationen für Ihr Abonnement zu übernehmen.


## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Sicherheitswarnungen finden Sie in den folgenden Artikeln:

* [Sicherheitswarnungen (Referenzhandbuch)](alerts-reference.md): Enthält Informationen zu den Sicherheitswarnungen, die im Threat Protection-Modul von Azure Security Center ggf. angezeigt werden.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Es wird beschrieben, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.