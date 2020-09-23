---
title: 'Schnellstart: Exportieren von Daten für Microsoft Azure Data Box'
description: Hier erfahren Sie, wie Sie Ihre Azure Data Box-Daten schnell über das Azure-Portal exportieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: quickstart
ms.date: 07/17/2020
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 01fdaa95dd396e5545b65bada2a9d6410169230b
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90052459"
---
# <a name="quickstart-get-started-with-azure-data-box-to-export-data-from-azure"></a>Schnellstart: Erste Schritte mit Azure Data Box zum Exportieren von Daten aus Azure

In dieser Schnellstartanleitung wird beschrieben, wie Sie mithilfe des Azure-Portals Daten aus Azure an Ihren Standort exportieren. Die Schritte umfassen das Verkabeln, Konfigurieren und Kopieren von Daten aus Azure. Der Schnellstart erfolgt im Azure-Portal und auf der lokalen Webbenutzeroberfläche des Geräts.

Eine ausführliche Anleitung zur Schritt-für-Schritt-Bereitstellung und zur Nachverfolgung finden Sie unter [Tutorial: Erstellen eines Exportauftrags für Azure Data Box](data-box-deploy-export-ordered.md).

## <a name="prerequisites"></a>Voraussetzungen

Vorbereitungen

* Sie verwenden ein Abonnement der folgenden Typen für den Data Box-Dienst:
  * Microsoft Enterprise Agreement (EA). Erfahren Sie mehr über [EA-Abonnements](https://azure.microsoft.com/pricing/enterprise-agreement/).
  * Cloud Solution Provider (CSP). Erfahren Sie mehr über das [Azure CSP-Programm](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview).
  * Microsoft Azure Sponsorship. Erfahren Sie mehr über das [Azure Sponsorship-Programm](https://azure.microsoft.com/offers/ms-azr-0036p/).

* Sie haben Zugriff als Besitzer oder Mitwirkender auf das Abonnement (zum Erstellen einer Data Box-Bestellung erforderlich).
* Überprüfen Sie die [Sicherheitsrichtlinien für Ihre Data Box](data-box-safety.md).
* Sie besitzen ein Azure Data Box-Gerät, um Daten an Ihren Standort zu verschieben. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
  * Es muss ein [unterstütztes Betriebssystem](data-box-system-requirements.md) ausgeführt werden.
  * Er muss mit einem Hochgeschwindigkeitsnetzwerk verbunden sein. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, kann eine 1-GbE-Datenverbindung verwendet werden, wodurch aber die Geschwindigkeit der Kopiervorgänge leidet.
* Sie verfügen über eine geeignete ebene Fläche, auf der Sie die Data Box aufstellen können. Wenn Sie das Gerät in einem standardmäßigen Rackregal einbauen möchten, benötigen Sie einen 7HE-Steckplatz in Ihrem Rack im Rechenzentrum. Sie können das Gerät waagerecht oder senkrecht in das Rack einbauen.
* Sie haben die folgenden Kabel zur Hand, um Ihre Data Box mit dem Hostcomputer zu verbinden.
  * Zwei SFP+-Twinax-Kupferkabel mit 10 GbE (zur Verwendung mit den Netzwerkschnittstellen DATA 1 und DATA 2)
  * Ein Netzwerkkabel des Typs RJ-45 CAT 6 (zur Verwendung mit der Verwaltungsnetzwerkschnittstelle (MGMT))
  * Ein Netzwerkkabel des Typs RJ-45 CAT 6A oder RJ-45 CAT 6 (zur Verwendung mit der Netzwerkschnittstelle DATA 3, die mit 10 GBit/s bzw. 1 GBit/s konfiguriert ist)

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="order"></a>Order

Dieser Schritt dauert ungefähr 10 Minuten.

1. Erstellen Sie im Azure-Portal eine neue Azure Data Box-Ressource.
2. Wählen Sie ein vorhandenes Abonnement, das für diesen Dienst aktiviert ist, und als Übertragungstyp **Nach Azure exportieren** aus. Geben Sie unter **Azure-Quellregion** an, wo sich die Daten befinden, und legen Sie das **Zielland** für die Datenübertragung fest.
3. Wählen Sie **Data Box** aus. Die maximal nutzbare Kapazität ist 80 TB. Für größere Datenmengen können Sie mehrere Aufträge erteilen.
4. Wählen Sie **Speicherkonto und Exporttyp hinzufügen** und dann **Exportoption auswählen** aus.
5. Geben Sie die Auftragsdetails und Versandinformationen ein. Wenn der Dienst in Ihrer Region verfügbar ist, können Sie E-Mail-Adressen für Benachrichtigungen angeben, die Zusammenfassung prüfen und anschließend den Auftrag erstellen.

Nachdem der Auftrag erstellt wurde, wird das Gerät für den Versand vorbereitet.

## <a name="cable"></a>Kabel

Dieser Schritt dauert ungefähr 10 Minuten.

Nach Erhalt der Data Box führen Sie die folgenden Schritte aus, um das Gerät zu verkabeln, anzuschließen und einzuschalten. Dieser Schritt dauert ungefähr 10 Minuten.

1. Wenn es Anzeichen gibt, dass das Gerät manipuliert oder beschädigt wurde, fahren Sie nicht fort. Wenden Sie sich an den Microsoft-Support mit der Bitte, Ihnen ein Austauschgerät zu senden.
2. Bevor Sie Ihr Gerät verkabeln, vergewissern Sie sich, dass Sie die folgenden Kabel haben:

   * Geerdetes Netzkabel (im Lieferumfang enthalten) mit einer Nennstromstärke von mindestens 10 A mit einem IEC60320 C-13-Stecker an einem Ende zum Anschluss an das Gerät
   * Ein Netzwerkkabel des Typs RJ-45 CAT 6 (zur Verwendung mit der Verwaltungsnetzwerkschnittstelle (MGMT))
   * Zwei SFP+ Twinax-Kupferkabel mit 10 GbE (zur Verwendung mit den 10-GBit/s-Netzwerkschnittstellen DATA 1 und DATA 2)
   * Ein Netzwerkkabel des Typs RJ-45 CAT 6A oder RJ-45 CAT 6 (zur Verwendung mit der Netzwerkschnittstelle DATA 3, die mit 10 GBit/s bzw. 1 GBit/s konfiguriert ist)

3. Nehmen Sie das Gerät heraus, und stellen Sie es auf eine ebene Fläche.

4. Verkabeln Sie das Gerät wie unten gezeigt.  

    ![Verkabelte Rückwandplatine der Data Box](media/data-box-deploy-set-up/data-box-cabled-dhcp.png)  

    1. Schließen Sie das Netzkabel an das Gerät an.
    2. Verwenden Sie das RJ-45 CAT 6-Netzwerkkabel, um Ihren Hostcomputer mit dem MGMT-Port des Geräts zu verbinden.
    3. Verwenden Sie das SFP+ Twinax-Kupferkabel, um mindestens eine Netzwerkschnittstelle (DATA 1 oder DATA 2) mit 10 GBit/s (bevorzugt gegenüber 1 GBit/s) für die Datenübertragung anzuschließen.
    4. Schalten Sie das Gerät ein. Der Netzschalter befindet sich auf der Vorderseite des Geräts.

## <a name="connect"></a>Verbinden

Dieser Schritt dauert ungefähr 5 bis 7 Minuten.

1. Zum Abrufen des Gerätekennworts wechseln Sie im [Azure-Portal](https://portal.azure.com) zu **Allgemein** > **Gerätedetails**.
2. Weisen Sie dem Ethernet-Adapter auf dem Computer, über den Sie eine Verbindung mit der Data Box herstellen, die statische IP-Adresse 192.168.100.5 und das Subnetz 255.255.255.255.0 zu. Greifen Sie unter `https://192.168.100.10` auf die lokale Webbenutzeroberfläche des Geräts zu. Der Verbindungsaufbau kann nach dem Einschalten des Geräts bis zu 5 Minuten dauern.
3. Melden Sie sich mit dem aus dem Azure-Portal abgerufenen Kennwort an. Sie sehen eine Fehlermeldung, die auf ein Problem mit dem Sicherheitszertifikat der Website hinweist. Befolgen Sie die browserspezifischen Anweisungen, um zur Webseite zu gelangen.
4. Standardmäßig werden die Netzwerkeinstellungen für die Datenschnittstelle mit 10 GBit/s (oder 1 GBit/s) als DHCP konfiguriert. Bei Bedarf können Sie diese Schnittstelle als statisch konfigurieren und eine IP-Adresse angeben.

## <a name="copy-data"></a>Kopieren von Daten

Die Dauer dieses Vorgangs hängt von der Datenmenge und der Netzwerkgeschwindigkeit ab.

1. Wenn Sie mit einem Windows-Client arbeiten, verwenden Sie ein SMB-kompatibles Dateikopierprogramm wie Robocopy. Verwenden Sie bei einem NFS-Host den Befehl `cp` oder `rsync` zum Kopieren der Daten. Weitere Informationen zur Verwendung von Robocopy zum Kopieren von Daten finden Sie unter [Robocopy](https://technet.microsoft.com/library/ee851678.aspx).
2. Stellen Sie eine Verbindung mit den Gerätefreigaben her, und beginnen Sie mit dem Kopieren von Daten auf den Hostcomputer.
<!-- 1. Connect to the device shares using the path:`\\<IP address of your device>\ShareName`. To get the share access credentials, go to the **Connect & copy** page in the local web UI of the Data Box. --> 

## <a name="ship-to-azure"></a>Senden an Azure

Dieser Vorgang dauert ca. 10-15 Minuten.

1. Wechseln Sie auf der lokalen Webbenutzeroberfläche zur Seite **Für den Versand vorbereiten**, und beginnen Sie mit der Versandvorbereitung.
2. Schalten Sie das Gerät auf der lokalen Webbenutzeroberfläche aus. Ziehen Sie die Kabel vom Gerät ab.
3. Wickeln Sie das mit dem Gerät gelieferte Netzkabel auf, und befestigen Sie es sicher an der Rückseite des Geräts.
4. Das Rücksendeetikett sollte auf der E-Ink-Anzeige sichtbar sein. Wird das Etikett in der E-Ink-Anzeige nicht angezeigt, navigieren Sie im Azure-Portal zu **Übersicht** > **Versandetikett herunterladen**. Laden Sie das Versandetikett herunter, und bringen Sie es am Gerät an.
5. Planen Sie eine Abholung mit Ihrem regionalen Paketdienst, wenn Sie das Gerät zurückgeben.
6. Nachdem die Data Box vom Kurierdienst abgeholt und eingescannt wurde, wird der Status der Bestellung im Portal in **Abgeholt** geändert. Außerdem wird eine Nachverfolgungs-ID angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Dieser Schritt dauert zwei bis drei Minuten.

* Im Azure-Portal können Sie den Data Box-Auftrag stornieren, bevor er verarbeitet wurde. Nachdem der Auftrag verarbeitet wurde, kann er nicht mehr storniert werden. Der Auftrag wird abgearbeitet, bis er den Status „Abgeschlossen“ erreicht hat. Navigieren Sie zum Stornieren des Auftrags zu **Übersicht**, und wählen Sie auf der Befehlsleiste **Stornieren** aus.

* Sie können einen Auftrag löschen, wenn im Azure-Portal dafür der Status **Abgeschlossen** oder **Abgebrochen** angezeigt wird. Navigieren Sie zum Löschen des Auftrags zu **Übersicht**, und wählen Sie auf der Befehlsleiste **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen Exportauftrag von Azure in Azure Data Box bereitgestellt. Fahren Sie mit dem folgenden Tutorial fort, um sich weiter über die Azure Data Box-Verwaltung zu informieren:

> [!div class="nextstepaction"]
> [Verwalten von Data Box im Azure-Portal](data-box-portal-admin.md)
