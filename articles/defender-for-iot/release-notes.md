---
title: Neuerungen in Azure Defender für IoT
description: In diesem Artikel wird beschrieben, welche Neuerungen das aktuelle Release von Defender für IoT enthält.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/06/2021
ms.author: shhazam
ms.openlocfilehash: 72a6e50134647194679055a886b50b01f42e212d
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629978"
---
# <a name="whats-new"></a>Was gibt es Neues?

Defender für IoT 10.0 verfügt über Featureverbesserungen, mit denen Aspekte wie die Sicherheit, Verwaltung und Benutzerfreundlichkeit verbessert werden.

## <a name="security"></a>Sicherheit

Für dieses Release wurden Verbesserungen bei der Wiederherstellung von Zertifikaten und Kennwörtern vorgenommen.

### <a name="certificates"></a>Zertifikate
  
Diese Version ermöglicht Folgendes:

- Direktes Hochladen von SSL-Zertifikaten auf die Sensoren und lokalen Verwaltungskonsolen
- Durchführen der Überprüfung zwischen der lokalen Verwaltungskonsole und den verbundenen Sensoren sowie zwischen einer Verwaltungskonsole und einer Verwaltungskonsole für Hochverfügbarkeit. Die Überprüfung basiert auf Ablaufdaten, der Echtzeit der Stammzertifizierungsstelle und auf Zertifikatssperrlisten.  Wenn bei der Überprüfung ein Fehler auftritt, wird die Sitzung nicht fortgesetzt.

Für Upgrades:

- Änderungen in Bezug auf die Funktionen für SSL-Zertifikate oder die Überprüfung während des Upgrades wurden nicht vorgenommen.
- Nach dem Upgrade können Administratorbenutzer der Sensoren und der lokalen Verwaltungskonsole SSL-Zertifikate ersetzen oder die Überprüfung von SSL-Zertifikaten über das Fenster „Systemeinstellungen“ > „SSL-Zertifikat“ aktivieren.  

Für Neuinstallationen:

- Bei der erstmaligen Anmeldung müssen Benutzer entweder ein SSL-Zertifikat (empfohlen) oder ein lokal generiertes selbstsigniertes Zertifikat (nicht empfohlen) verwenden.
- Für Neuinstallationen ist die Zertifikatüberprüfung standardmäßig aktiviert.

### <a name="password-recovery"></a>Kennwortwiederherstellung
  
Administratorbenutzer der Sensoren und der lokalen Verwaltungskonsole können jetzt im Azure Defender für IoT-Portal Kennwörter wiederherstellen. Bisher musste für die Kennwortwiederherstellung das Supportteam hinzugezogen werden.

## <a name="onboarding"></a>Onboarding

### <a name="on-premises-management-console---committed-devices"></a>Lokale Verwaltungskonsole: Committete Geräte

Nach der erstmaligen Anmeldung bei der lokalen Verwaltungskonsole müssen Benutzer jetzt eine Aktivierungsdatei hochladen. Die Datei enthält die aggregierte Anzahl von Geräten, die im Netzwerk der Organisation überwacht werden sollen. Diese Zahl wird als Anzahl von committeten Geräten bezeichnet.
Committete Geräte werden beim Onboardingprozess im Azure Defender für IoT-Portal definiert, bei dem die Aktivierungsdatei generiert wird.
Erstmalige Benutzer und Benutzer, die ein Upgrade durchführen, müssen die Aktivierungsdatei hochladen.
Nach der ersten Aktivierung kann es vorkommen, dass die Anzahl von im Netzwerk erkannten Geräten die Anzahl von committeten Geräten übersteigt. Dies kann beispielsweise der Fall sein, wenn Sie weitere Sensoren mit der Verwaltungskonsole verbinden. Bei einer Abweichung zwischen der Anzahl erkannter Geräte und der Anzahl committeter Geräte wird in der Verwaltungskonsole eine Warnung angezeigt. In diesem Fall sollten Sie eine neue Aktivierungsdatei hochladen.

### <a name="pricing-page-options"></a>Optionen auf der Preisseite

Auf der Preisseite können Sie für neue Abonnements das Onboarding für Azure Defender für IoT durchführen und committete Geräte in Ihrem Netzwerk definieren.  
Darüber hinaus können Sie auf der Preisseite jetzt vorhandene Abonnements verwalten, die einem Sensor zugeordnet sind, und die committeten Geräte aktualisieren.

### <a name="view-and-manage-onboarded-sensors"></a>Anzeigen und Verwalten von integrierten Sensoren

Auf einer neuen Portalseite für Standorte und Sensoren können Sie folgende Aktionen durchführen:

- Hinzufügen von beschreibenden Informationen zum Sensor. Dies können beispielsweise Zonen sein, die dem Sensor zugeordnet sind, oder Freitext-Tags.
- Anzeigen und Filtern von Sensorinformationen. Sie können beispielsweise Details zu Sensoren, die mit der Cloud verbunden sind oder lokal verwaltet werden, oder Informationen zu Sensoren in einer bestimmten Zone anzeigen.  

## <a name="usability"></a>Benutzerfreundlichkeit

### <a name="azure-sentinel-new-connector-page"></a>Azure Sentinel-Seite „Neuer Connector“

Die Azure Defender für IoT-Seite für den Datenconnector in Azure Sentinel wurde überarbeitet. Der Datenconnector basiert jetzt nicht mehr auf IoT-Hubs, sondern auf Abonnements, um Kunden eine bessere Verwaltung ihrer Konfigurationsverbindung in Azure Sentinel zu ermöglichen.

### <a name="azure-portal-permission-updates"></a>Updates für Berechtigungen im Azure-Portal  

Die Unterstützung für „Sicherheitsleseberechtigte“ und „Sicherheitsadministrator“ wurde hinzugefügt.

## <a name="other-updates"></a>Weitere Updates

### <a name="access-group---zone-permissions"></a>Zugriffsgruppe: Zonenberechtigungen
  
Die Zugriffsgruppenregeln für die lokale Verwaltungskonsole enthalten nicht die Option zum Gewähren des Zugriffs auf eine bestimmte Zone. Es wurde keine Änderung für das Definieren von Regeln vorgenommen, in denen Standorte, Regionen und Geschäftseinheiten verwendet werden.   Nach dem Upgrade werden Zugriffsgruppen, für die über Regeln der Zugriff auf bestimmte Zonen zulässig war, so geändert, dass der Zugriff auf deren übergeordneten Standort, einschließlich aller Zonen, möglich ist.

### <a name="terminology-changes"></a>Terminologieänderungen

Anstelle des Ausdrucks „Ressource“ wird für den Sensor und die lokale Verwaltungskonsole, in den Berichten und in anderen Lösungsbenutzeroberflächen jetzt „Gerät“ verwendet.
Anstelle von „Ereignis verwalten“ wird in Warnungen für den Sensor und in der lokalen Verwaltungskonsole jetzt „Problembehandlungsschritte“ verwendet.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte mit Azure Defender für IoT](getting-started.md)
