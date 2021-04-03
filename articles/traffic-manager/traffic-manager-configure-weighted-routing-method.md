---
title: 'Tutorial: Konfigurieren der gewichteten Roundrobin-Routingmethode für Datenverkehr mit Azure Traffic Manager'
description: In diesem Tutorial wird der Lastenausgleich des Datenverkehrs mit einer Roundrobinmethode in Traffic Manager beschrieben.
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2020
ms.author: duau
ms.openlocfilehash: abcfce43b90c7371d5b38aa5b7a6d478e9d6a0dd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92207830"
---
# <a name="tutorial-configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Tutorial: Konfigurieren der gewichteten Routingmethode für Datenverkehr in Traffic Manager

Ein gängiges Muster für das Routing von Datenverkehr besteht darin, eine Reihe identischer Endpunkte (die Clouddienste und Websites umfassen) bereitzustellen und gleichmäßig Datenverkehr an die einzelnen Endpunkte zu senden. Die folgenden Schritte beschreiben, wie diese Routingmethodentyp für Datenverkehr konfiguriert wird.

> [!NOTE]
> Beachten Sie, dass Azure Web-App bereits Roundrobin-Lastenausgleichsfunktionen für Websites in einer Azure-Region (die mehrere Datencenter enthalten kann) zur Verfügung stellt. Traffic Manager ermöglicht das Verteilen von Datenverkehr auf Websites in verschiedenen Datencentern.

In diesem Tutorial lernen Sie, wie die folgenden Aufgaben ausgeführt werden:
> [!div class="checklist"]
> - Erstellen eines Traffic Manager-Profils mit gewichtetem Routing.
> - Verwenden des Traffic Manager-Profils.
> - Löschen des Traffic Manager-Profils

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/).

## <a name="configure-the-weighted-traffic-routing-method"></a>Konfigurieren der gewichteten Routingmethode für Datenverkehr

1. Melden Sie sich im Browser beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie in der Suchleiste des Portals nach dem Namen des **Traffic Manager-Profils**, das Sie im vorhergehenden Abschnitt erstellt haben, und wählen Sie in den angezeigten Ergebnissen das Traffic Manager-Profil aus.

    :::image type="content" source="./media/traffic-manager-weighted-routing-method/search-traffic-manager-weighted-profile.png" alt-text="Suchen nach einem Traffic Manager-Profil":::

1. Wählen Sie **Konfiguration** und dann die folgenden Einstellungen aus, oder geben Sie sie ein:

    | Einstellung         | Wert                                              |
    | ---             | ---                                                |
    | Routingmethode            | Wählen Sie **Gewichtet** aus. |    
    | DNS-Gültigkeitsdauer (TTL) | Dieser Wert für die DNS-Gültigkeitsdauer bestimmt, wie häufig der lokale Cachenamenserver des Clients aktualisierte DNS-Einträge aus dem Traffic Manager-System abfragt. Für jede Änderung in Traffic Manager, beispielsweise eine Änderung der Datenverkehrs-Routingmethode oder Änderungen bezüglich der Verfügbarkeit hinzugefügter Endpunkte, wird dieser Zeitraum benötigt, bis die Aktualisierung im globalen System der DNS-Server ausgeführt wurde. |
    | Protokoll    | Wählen Sie ein Protokoll für die Endpunktüberwachung aus. *Optionen: HTTP, HTTPS und TCP* |
    | Port | Geben Sie die Portnummer an. |
    | `Path` | Sie müssen einen Pfad und einen Dateinamen angeben, um Endpunkte zu überwachen. Ein Schrägstrich „/“ ist ein gültiger Eintrag für den relativen Pfad und bedeutet, dass sich die Datei im Stammverzeichnis (Standardwert) befindet. |
    | Benutzerdefinierte Headereinstellungen | Konfigurieren Sie die benutzerdefinierten Header im Format „host:contoso.com,newheader:newvalue“. Maximal unterstütztes Paar ist 8. Anwendbar für das HTTP- und das HTTPS-Protokoll. Anwendbar für alle Endpunkte im Profil. |
    | Erwartete Statuscodebereiche (Standardwert: 200) | Konfigurieren Sie die Statuscodebereiche im Format „200–299,301–301“. Maximal unterstützter Bereich ist 8. Anwendbar für das HTTP- und das HTTPS-Protokoll. Anwendbar für alle Endpunkte im Profil. |
    | Testintervall | Konfigurieren Sie das Zeitintervall zwischen Endpunktintegritätstests. Sie können 10 oder 30 Sekunden auswählen. |
    | Tolerierte Anzahl von Fehlern | Konfigurieren Sie die Anzahl von Fehlern beim Integritätstest, nach deren Erreichen ein Endpunktfehler ausgelöst wird. Geben Sie eine Zahl zwischen 0 und 9 ein. | 
    | Testtimeout | Konfigurieren Sie die erforderliche Zeit, die verstreichen muss, damit es bei Integritätstests eines Endpunkts zum Timeout kommt. Dieser Wert muss mindestens 5 und kleiner als der Wert des Testintervalls sein. |

1. Klicken Sie auf **Speichern**, um die Konfiguration abzuschließen.

    :::image type="content" source="./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-configuration.png" alt-text="Traffic Manager: gewichtete Konfiguration"::: 

1. Wählen Sie **Endpunkt** aus, und konfigurieren Sie die Gewichtung der einzelnen Endpunkte. Die Gewichtung kann zwischen 1 und 1000 liegen. Je höher die Gewichtung, desto höher die Priorität.  

    :::image type="content" source="./media/traffic-manager-weighted-routing-method/traffic-manager-configure-endpoints-weighted.png" alt-text="Traffic Manager: gewichtete Endpunktkonfiguration"::: 

## <a name="use-the-traffic-manager-profile"></a>Verwenden des Traffic Manager-Profils

Unter **Traffic Manager-Profil** wird der DNS-Name Ihres neu erstellten Traffic Manager-Profils angezeigt. Der Name kann von beliebigen Clients (etwa durch Navigation in einem Webbrowser) für das Routing zum richtigen Endpunkt nach Maßgabe des Routingtyps verwendet werden. In diesem Fall werden alle Anforderungen in einem Roundrobinverfahren an die einzelnen Endpunkte weitergeleitet.

:::image type="content" source="./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-overview.png" alt-text="Traffic Manager: gewichtete Übersicht"::: 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie das Traffic Manager-Profil nicht mehr benötigen, suchen Sie das Profil, und wählen Sie **Profil löschen** aus.

:::image type="content" source="./media/traffic-manager-weighted-routing-method/delete-traffic-manager-weighted-profile.png" alt-text="Traffic Manager: gewichtetes Profil löschen":::

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie mehr zur gewichteten Routingmethode:

> [!div class="nextstepaction"]
> [Gewichtete Methode für das Datenverkehrsrouting](traffic-manager-routing-methods.md#weighted)