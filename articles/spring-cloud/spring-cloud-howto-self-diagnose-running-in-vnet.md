---
title: Selbstdiagnose für das VNET von Azure Spring Cloud
description: Hier finden Sie Informationen zur Selbstdiagnose sowie zur Behebung von Problemen im Zusammenhang mit Azure Spring Cloud im VNET.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/25/2021
ms.custom: devx-track-java
ms.openlocfilehash: 5407213b62902326d53b73e42ee3af1ba9b11524
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102124995"
---
# <a name="self-diagnose-running-azure-spring-cloud-in-vnet"></a>Selbstdiagnose beim Ausführen von Azure Spring Cloud im VNET
Die Azure Spring Cloud-Diagnose unterstützt in virtuellen Netzwerken ausgeführte interaktive Problembehandlungs-Apps ohne Konfiguration. Durch die Azure Spring Cloud-Diagnose werden Probleme identifiziert und Informationen bereitgestellt, die Sie bei der Problembehandlung und -behebung unterstützen.

## <a name="navigate-to-the-diagnostics-page"></a>Navigieren zur Diagnoseseite
Mit dem folgenden Verfahren wird die Diagnose für vernetzte Anwendungen gestartet.
1. Melden Sie sich beim Azure-Portal an.
1. Navigieren Sie zur Seite Übersicht von Azure Spring Cloud.
1. Wählen Sie im linken Navigationsbereich im Menü die Option **Probleme diagnostizieren und beheben** aus.
1. Wählen Sie die dritte Kategorie (**Netzwerk**) aus.

   ![Selbstdiagnose: Titel](media/spring-cloud-self-diagnose-vnet/self-diagostic-title.png)

## <a name="view-a-diagnostic-report"></a>Anzeigen eines Diagnoseberichts
Nach dem Klicken auf die Kategorie **Netzwerk** können Sie zwei spezifische netzwerkbezogene Probleme im Zusammenhang mit Ihrer Azure Spring Cloud-Instanz mit VNET-Einschleusung anzeigen: **DNS-Auflösung** und **Erforderlicher ausgehender Datenverkehr**.

   ![Selbstdiagnose: Optionen](media/spring-cloud-self-diagnose-vnet/self-diagostic-dns-req-outbound-options.png)

Wählen Sie Ihr Zielproblem aus, um den Diagnosebericht anzuzeigen. Eine Zusammenfassung der Diagnose wird angezeigt. Beispiel: 

* *Die Ressource wurde entfernt.*
* *Die Ressource ist nicht in Ihrem eigenen virtuellen Netzwerk bereitgestellt.*

Einige Ergebnisse enthalten zugehörige Dokumentation. Für unterschiedliche Subnetze werden die Ergebnisse separat angezeigt.

## <a name="dns-resolution"></a>DNS-Auflösung 
Wenn Sie **DNS-Auflösung** auswählen, geben die Ergebnisse Aufschluss darüber, ob DNS-Probleme mit Anwendungen vorliegen.  Fehlerfreie Apps werden wie folgt aufgeführt:

* *DNS issues resolved with no issues in subnet 'subnet01'.* (DNS-Probleme behoben. Keine Probleme im Subnetz „subnet01“.)
* *DNS issues resolved with no issues in subnet 'subnet02'.* (DNS-Probleme behoben. Keine Probleme im Subnetz „subnet02“.)

Der folgende exemplarische Diagnosebericht gibt an, dass der Zustand der Anwendung unbekannt ist. Der Zeitrahmen für die Berichterstattung umfasst nicht die Zeit, zu der der Integritätsstatus gemeldet wurde.  Angenommen, die Endzeit des Kontexts ist *2021-03-03T04:20:00Z*. Der letzte Zeitstempel in den **Renderings für die DNS-Auflösungstabelle** ist *2021-03-03T03:39:00Z*, also der Vortag. Das Systemdiagnoseprotokoll wurde unter Umständen aufgrund eines blockierten Netzwerks nicht gesendet. 

Die Ergebnisse mit dem unbekannten Integritätsstatus enthalten eine entsprechende Dokumentation.  Sie können auf die öffnende spitze Klammer klicken, um die Dropdownanzeige einzublenden.

   ![DNS unbekannt](media/spring-cloud-self-diagnose-vnet/self-diagostic-dns-unknown.png)

Wenn Sie Ihren Datensatz für die private DNS-Zone falsch konfiguriert haben, erhalten Sie ein kritisches Ergebnis wie etwa: `Failed to resolve the Private DNS in subnet xxx`. 

Im Dropdownbereich mit den **Renderings für die DNS-Auflösungstabelle** finden Sie ausführliche Meldungsinformationen, die Sie zum Überprüfen Ihrer Konfiguration verwenden können.

## <a name="required-outbound-traffic"></a>Erforderlicher ausgehender Datenverkehr 

Wenn Sie **Erforderlicher ausgehender Datenverkehr** auswählen, geben die Ergebnisse Aufschluss darüber, ob bei Anwendungen Probleme mit ausgehendem Datenverkehr vorliegen.  Fehlerfreie Apps werden wie folgt aufgeführt:

* *Required outbound traffic resolved with no issues in subnet 'subnet01'. (Der erforderliche ausgehende Datenverkehr wurde ohne Probleme im Subnetz „subnet01“ aufgelöst.)
* *Required outbound traffic resolved with no issues in subnet 'subnet02'. (Der erforderliche ausgehende Datenverkehr wurde ohne Probleme im Subnetz „subnet02“ aufgelöst.)

Wenn ein Subnetz durch NSG- oder Firewallregeln blockiert wird und Sie das Protokoll nicht blockiert haben, finden Sie die folgenden Fehler. Sie können überprüfen, ob Sie [Kundenzuständigkeiten](spring-cloud-vnet-customer-responsibilities.md) übersehen haben.
    
   ![Endpunktfehler](media/spring-cloud-self-diagnose-vnet/self-diagostic-endpoint-failed.png)

Falls in `Required Outbound Traffic Table Renderings` innerhalb von 30 Minuten keine Daten vorhanden sind, lautet das Ergebnis `health status unknown`. Möglicherweise ist Ihr Netzwerk blockiert, oder der Protokolldienst ist ausgefallen.

   ![Unbekannter Diagnoseendpunkt](media/spring-cloud-self-diagnose-vnet/self-diagostic-endpoint-unknown.png)

## <a name="see-also"></a>Weitere Informationen
* [Selbstdiagnose und Lösung von Problemen in Azure Spring Cloud](spring-cloud-howto-self-diagnose-solve.md)