---
title: Grundlegendes zu Apps und Bereitstellungen in Azure Spring Cloud
description: Der Unterschied zwischen Anwendung und Bereitstellung in Azure Spring Cloud.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 07/23/2020
ms.custom: devx-track-java
ms.openlocfilehash: c053a7830e02eb7c460bd030ca3c6a10c00ea78a
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91323636"
---
# <a name="understand-app-and-deployment-in-azure-spring-cloud"></a>Grundlegendes zu Apps und Bereitstellungen in Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

**App** und **Bereitstellung** sind die beiden wichtigsten Konzepte im Ressourcenmodell von Azure Spring Cloud. In Azure Spring Cloud ist eine *App* eine Abstraktion einer Geschäfts-App oder eines Microservice.  Eine Version des Codes oder der Binärdatei, die als die *App* bereitgestellt wird, wird in einer *Bereitstellung* ausgeführt.  Apps werden in einer *Azure Spring Cloud-Dienstinstanz* (kurz *Dienstinstanz*) ausgeführt, wie im Folgenden beschrieben.

 ![Apps und Bereitstellungen](./media/spring-cloud-app-and-deployment/app-deployment-rev.png)

Sie können über mehrere Dienstinstanzen innerhalb eines einzelnen Azure-Abonnements verfügen, es ist jedoch am einfachsten, wenn sich alle Apps, aus denen eine Geschäfts-App oder ein Microservice besteht, innerhalb einer einzelnen Azure Spring Cloud-Dienstinstanz befinden.

Der Standardtarif der Azure Spring Cloud ermöglicht, dass eine App eine Produktionsbereitstellung und eine Stagingbereitstellung hat, sodass Sie damit auf einfache Weise eine Blue/Green-Bereitstellung durchführen können.

## <a name="app"></a>App
Die folgenden Features/Eigenschaften werden auf App-Ebene definiert.

| Features | Beschreibung |
|:--|:----------------|
| Öffentlich</br>Endpunkt | Die URL für den Zugriff auf die App. |
| Benutzerdefiniert</br>Domain | Der CNAME-Eintrag, der die benutzerdefinierte Domäne sichert. |
| Dienst</br>Bindung | Standardmäßige Verbindung mit anderen Azure-Diensten |
| Verwaltet</br>Identity | Verwaltete Entität von Azure Active Directory, die es Ihrer App erlaubt, einfach auf andere Azure AD-geschützte Ressourcen wie Azure Key Vault zuzugreifen. |
| Beständig</br>Storage | Einstellung, die es ermöglicht, dass Daten nach dem Neustart der App beibehalten werden können. |

## <a name="deployment"></a>Bereitstellung

Die folgenden Features/Eigenschaften werden auf der Bereitstellungsebene definiert und beim Wechsel der Produktions-/Stagingbereitstellung ausgetauscht.

| Features | Beschreibung |
|:--|:----------------|
| CPU | Anzahl der virtuellen Kerne pro App-Instanz. |
| Arbeitsspeicher | GB an Arbeitsspeicher pro App-Instanz|
| Instanz</br>Anzahl | Die Anzahl der App-Instanzen, manuell oder automatisch festgelegt. |
| Automatische Skalierung | Anzahl der Skalierungsinstanzen, automatisch auf vordefinierten Regeln und Zeitplänen basierend. |
| JVM</br>Optionen | Festlegen von JVM-Optionen  |
| Environment</br>Variables | Festlegen von Umgebungsvariablen |
| Typ</br>Version | Java 8/Java 11|

## <a name="restrictions"></a>Beschränkungen

* **Eine App muss eine Produktionsbereitstellung besitzen**: Das Löschen einer Produktionsbereitstellung ist durch die API blockiert. Sie sollte vor dem Löschen zu Staging gewechselt werden.
* **Eine App kann höchstens zwei Bereitstellungen besitzen**: Das Erstellen von mehr als zwei Bereitstellungen ist durch die API blockiert. Stellen Sie Ihre neue Binärdatei entweder in der vorhandenen Produktions- oder der Stagingbereitstellung bereit.
* **Die Bereitstellungs Verwaltung ist im Tarif „Basic“ nicht verfügbar**: Verwenden Sie den Tarif „Standard“ für die Blue/Green-Bereitstellungsfunktion.

## <a name="see-also"></a>Siehe auch
* [Einrichten einer Stagingumgebung in Azure Spring Cloud](spring-cloud-howto-staging-environment.md)
