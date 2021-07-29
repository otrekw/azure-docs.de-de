---
title: 'Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Maßgeblicher Anwendungsfall'
description: Der folgende Anwendungsfall basiert auf einem realen Kundenszenario eines Unternehmens, das seine MySQL-Workload zu Azure Database for MySQL migriert hat.
ms.service: mysql
ms.subservice: migration-guide
ms.topic: how-to
author: arunkumarthiags
ms.author: arthiaga
ms.reviewer: maghan
ms.custom: ''
ms.date: 05/25/2021
ms.openlocfilehash: ccacc5caaa5307c64ba6c0e45ae05a0b02a6614f
ms.sourcegitcommit: ef950cf37f65ea7a0f583e246cfbf13f1913eb12
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111421632"
---
# <a name="mysql-on-premises-to-azure-database-for-mysql-migration-guide-representative-use-case"></a>Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Maßgeblicher Anwendungsfall

## <a name="overview"></a>Übersicht

Der folgende Anwendungsfall basiert auf einem realen Kundenszenario eines Unternehmens, das seine MySQL-Workload zu Azure Database for MySQL migriert hat.

World-Wide Importers (WWI) ist ein Hersteller und Großhändler von neuartigen Waren mit Sitz in San Francisco, Kalifornien. Das Unternehmen hat den Betrieb im Jahr 2002 aufgenommen und ein effektives Business-to-Business-Modell (B2B) für den direkten Vertrieb der selbst erzeugten Artikel an Einzelhandelskunden in den ganzen USA entwickelt. Zu den Kunden gehören Fachgeschäfte, Supermärkte, Computergeschäfte, touristische Geschäfte und einige Einzelpersonen. Dieses B2B-Modell ermöglicht ein optimiertes Verteilystem für die Produkte des Unternehmens, sodass das Unternehmen Kosten senken konnte und konkurrenzfähigere Preise für die hergestellten Produkte anbieten kann. Das Unternehmen verkauft seine Produkte auch über ein Netzwerk von Agenten, die die Produkte im Auftrag von WWI bewerben, an andere Händler.

Vor dem Einstieg in neue Bereiche möchte WWI sicherstellen, dass seine IT-Infrastruktur das erwartete Wachstum bewältigen kann. WWI hostet derzeit die gesamte IT-Infrastruktur lokal am Hauptsitz des Unternehmens und ist der Meinung, dass die Verlagerung dieser Ressourcen in die Cloud ein zukünftiges Wachstum möglich macht. Daher wurde der CIO beauftragt, die Migration des Kundenportals und der zugehörigen Datenworkloads in die Cloud zu beaufsichtigen.

WWI möchte weiterhin die vielen erweiterten Funktionen nutzen, die in der Cloud verfügbar sind, und ist daran interessiert, die Datenbanken und zugehörigen Workloads des Unternehmens nach Azure zu migrieren. Dies soll in kurzer Zeit geschehen, ohne Änderungen an den Anwendungen oder Datenbanken vornehmen zu müssen. Zunächst sollen die Java-basierten Webanwendung für das Kundenportal sowie die zugehörigen MySQL-Datenbanken und -Workloads in die Cloud migriert werden.

#### <a name="migration-goals"></a>Migrationsziele

Die wichtigsten Ziele für die Migration der Datenbanken und zugehörigen SQL-Workloads in die Cloud sind:

  - Verbessern des allgemeinen Sicherheitsstatus von ruhenden und übertragenen Daten.

  - Verbessern der Hochverfügbarkeits- und Notfallwiederherstellungsfunktionen.

  - Positionieren der Organisation für die Verwendung von cloudnativen Funktionen und Technologien wie Point-in-Time-Wiederherstellung.

  - Nutzen der Verwaltungs- und Leistungsoptimierungsfeatures von Azure Database for MySQL.

  - Erstellen einer skalierbaren Plattform, mit der das Unternehmen seine Geschäftstätigkeit in weitere geografische Regionen erweitern kann.

  - Ermöglichen einer besseren Einhaltung verschiedener rechtlicher Anforderungen, wenn personenbezogene Informationen gespeichert werden.

WWI hat das [Cloud Adoption Framework (CAF)](/azure/cloud-adoption-framework/) verwendet, um sein Team über die folgenden Leitlinien zu bewährten Methoden für die Cloudmigration zu schulen. Anschließend hat WWI die Migration mithilfe von CAF als übergeordneten Migrationsleitlinie in drei Hauptphasen angepasst. Schließlich wurden die Aktivitäten definiert, die in den einzelnen Phasen erledigt werden mussten, um eine erfolgreiche Lift & Shift-Cloudmigration sicherzustellen.

Diese Phasen umfassen:

| Phase | Name           | activities                                                                                                     |
|-------|----------------|----------------------------------------------------------------------------------------------------------------|
| 1     | Vor der Migration  | Bewertung, Planung, Auswertung der Migrationsmethode, Auswirkungen auf Anwendungen, Testpläne, Leistungsbaselines |
| 2     | Migration      | Ausführung der Migration, Ausführung der Testpläne                                                                          |
| 3     | Nach der Migration | Geschäftskontinuität, Notfallwiederherstellung, Verwaltung, Sicherheit, Leistungsoptimierung, Plattformmodernisierung |

WWI verfügt über mehrere Instanzen von MySQL mit unterschiedlichen Versionen von 5.5 bis 5.7. Die Instanzen sollen so schnell wie möglich auf die neueste Version umgestellt werden. Dabei muss aber sichergestellt sein, dass die Anwendungen bei der Umstellung auf die neueren Versionen weiterhin funktionieren. Das Unternehmen ist mit der Umstellung zur gleichen Version in der Cloud und dem anschließenden Upgrade vertraut. Sie würden jedoch lieber zwei Aufgaben gleichzeitig erledigen können.

Darüber hinaus möchte das Unternehmen sicherstellen, dass die Datenworkloads bei einem Störfall sicher und in mehreren geografischen Regionen verfügbar sind, und sich die verfügbaren Konfigurationsoptionen ansehen.

WWI möchte für die erste Migration mit einer einfachen Anwendung beginnen und dann in einer späteren Phase zu unternehmenskritischen Anwendungen übergehen. Dadurch erhält das Team die benötigten Kenntnisse und Erfahrungen, um zukünftige Migrationen vorzubereiten und zu planen.  

> [!div class="nextstepaction"]
> [Bewertung](./assessment.md)