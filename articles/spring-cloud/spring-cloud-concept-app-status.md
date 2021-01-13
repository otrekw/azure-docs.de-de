---
title: Grundlegendes zum App-Status in Azure Spring Cloud
description: Erfahren Sie etwas über die App-Statuskategorien in Azure Spring Cloud
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: ec87c5523d6f608363ca615037f20365705609df
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90892852"
---
# <a name="understanding-app-status-in-azure-spring-cloud"></a>Grundlegendes zum App-Status in Azure Spring Cloud

**Dieser Artikel gilt für:** ✔️ Java ✔️ C#

Die Azure Spring Cloud-Benutzeroberfläche stellt Informationen zum Status ausgeführter Anwendungen bereit.  Für jede Ressourcengruppe in einem Abonnement gibt es die Option **Apps**, in der der allgemeine Status von Anwendungstypen angezeigt wird.  Für jeden Anwendungstyp werden **Anwendungsinstanzen** angezeigt.

## <a name="apps-status"></a>App-Status
Wählen Sie im linken Navigationsbereich einer Ressourcengruppe **Apps** aus, um den allgemeinen Status eines Anwendungstyps anzuzeigen. Im Ergebnis wird den Status der bereitgestellten App angezeigt:

* **Bereitstellungsstatus** zeigt den Bereitstellungsstatus der Bereitstellung an.
* **Running instance** (Ausgeführte Instanz) zeigt an, wie viele App-Instanzen ausgeführt und wie viele gewünscht werden. Wenn die App beendet werden soll, wird in dieser Spalte *Beendet*angezeigt.
* **Registered Instance** (Registrierte Instanz) zeigt an, wie viele App-Instanzen bei Eureka registriert sind und wie viele gewünscht werden. Wenn die App beendet werden soll, wird in dieser Spalte *Beendet*angezeigt.


 ![App-Status](media/spring-cloud-concept-app-status/apps-ui-status.png)

**Der Bereitstellungsstatus wird als einer der folgenden Werte gemeldet:**

| Enumeration | Definition |
|:--:|:----------------:|
| Wird ausgeführt | Die Bereitstellung SOLLTE ausgeführt werden. |
| Beendet | Die Bereitstellung SOLLTE beendet werden. |

**Der Bereitstellungsstatus ist nur über die Befehlszeilenschnittstelle verfügbar.  Er wird als einer der folgenden Werte gemeldet:**

| Enumeration | Definition |
|:--:|:----------------:|
| Wird erstellt | Die Ressource wird erstellt. |
| Wird aktualisiert | Die Ressource wird aktualisiert. |
| Erfolgreich | Die Ressourcen wurden erfolgreich bereitgestellt, und die Binärdatei wird bereitgestellt. |
| Fehler | Fehler beim Erreichen des Ziels *Erfolgreich*. |
| Wird gelöscht | Die Ressource wird gelöscht. Dies verhindert den Vorgang, und die Ressource ist in diesem Status nicht verfügbar. |

## <a name="app-instances-status"></a>Status von App-Instanzen

Um den Status einer bestimmten Instanz einer bereitgestellten App anzuzeigen, klicken Sie auf den **Namen** der App in der Benutzeroberfläche **Apps**. Die Ergebnisse werden angezeigt:
* **Status:** Gibt an, ob die Instanz ausgeführt wird, oder ihren Zustand
* **DiscoveryStatus**: Der registrierte Status der App-Instanz in Eureka Server

 ![Status von App-Instanzen](media/spring-cloud-concept-app-status/apps-ui-instance-status.png)

**Der Instanzstatus wird als einer der folgenden Werte gemeldet:**

| Enumeration | Definition |
|:--:|:----------------:|
| Wird gestartet | Die Binärdatei wurde erfolgreich für die angegebene Instanz bereitgestellt. Beim Starten der JAR-Datei kann ein Fehler auftreten, da sie nicht ordnungsgemäß ausgeführt werden kann. |
| Wird ausgeführt | Die Instanz funktioniert. |
| Fehler | Die App-Instanz konnte die Binärdatei des Benutzers nach mehreren Wiederholungen nicht starten. |
| Abbrechen | Die App-Instanz wird heruntergefahren. |

**Der Erkennungsstatus der Instanz wird als einer der folgenden Werte gemeldet:**

| Enumeration | Definition |
|:--:|:----------------:|
| UP | Die App-Instanz ist bei Eureka registriert und für den Empfang von Datenverkehr bereit. |
| OUT_OF_SERVICE | Die App-Instanz ist bei Eureka registriert und kann Datenverkehr empfangen. Sie ist jedoch absichtlich für Datenverkehr gesperrt. |
| DOWN | Die App-Instanz ist nicht bei Eureka registriert, oder sie ist registriert, kann jedoch keinen Datenverkehr empfangen. |


## <a name="see-also"></a>Weitere Informationen:
* [Vorbereiten einer Spring- oder Steeltoe-Anwendung für die Bereitstellung in Azure Spring Cloud](spring-cloud-tutorial-prepare-app-deployment.md)