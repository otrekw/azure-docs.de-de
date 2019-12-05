---
title: Definieren eines neuen IoT-Gerätetyps in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung eine neue Azure IoT-Gerätevorlage erstellen. Sie definieren die Telemetriedaten, den Zustand, die Eigenschaften und die Befehle für den Typ.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 177caaa5400c10ed8de80b04a3305dce7cae77d6
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74407021"
---
# <a name="tutorial-define-a-new-iot-device-type-in-your-azure-iot-central-application-preview-features"></a>Tutorial: Definieren eines neuen IoT-Gerätetyps in Ihrer Azure IoT Central-Anwendung (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

Eine Gerätevorlage ist eine Blaupause, die die Merkmale und das Verhalten eines Gerätetyps definiert, der eine Verbindung mit einer Azure IoT Central-Anwendung herstellt.

Beispielsweise kann ein Hersteller eine Gerätevorlage für einen verbundenen Lüfter mit folgenden Merkmalen erstellen:

- Sendet Temperaturtelemetriedaten
- Sendet Standorteigenschaft
- Sendet Lüftermotor-Fehlerereignisse
- Sendet Lüfterbetriebszustand
- Bietet eine schreibbare Eigenschaft für die Lüftergeschwindigkeit
- Bietet einen Befehl zum Neustarten des Geräts
- Bietet eine allgemeine Übersicht über das Gerät mithilfe eines Dashboards

Anhand dieser Gerätevorlage kann ein Bediener echte Lüftergeräte erstellen und verbinden. Alle diese Lüfter weisen Messungen, Eigenschaften und Befehle auf, die von Bedienern zum Überwachen und Verwalten verwendet werden. Bediener verwenden die Gerätedashboards und -formulare, um mit den Lüftergeräten zu interagieren.

> [!NOTE]
> Nur Ersteller und Administratoren können Gerätevorlagen erstellen, bearbeiten und löschen. Auf der Seite **Geräte** kann jeder Benutzer Geräte anhand vorhandener Gerätevorlagen erstellen.

[IoT Plug & Play](../../iot-pnp/overview-iot-plug-and-play.md) ermöglicht IoT Central die Integration von Geräten, ohne dass Sie eingebetteten Gerätecode schreiben müssen. Das Herzstück von IoT Plug & Play ist ein Gerätefunktionsmodell-Schema, das Gerätefunktionen beschreibt. In einer IoT Central Preview-Anwendung verwenden Gerätevorlagen diese IoT Plug & Play-Gerätefunktionsmodelle.

Als Ersteller haben Sie mehrere Möglichkeiten zum Erstellen von Gerätevorlagen:

- Entwerfen Sie die Gerätevorlage in IoT Central, und implementieren Sie dann das entsprechende Gerätefunktionsmodell in Ihrem Gerätecode.
- Importieren Sie ein Gerätefunktionsmodell aus dem [Azure Certified for IoT-Gerätekatalog](https://aka.ms/iotdevcat). Fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die Ihre IoT Central-Anwendung benötigt.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie Ihren Gerätecode aus dem Modell. Importieren Sie das Gerätefunktionsmodell manuell in Ihre IoT Central-Anwendung, und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Dashboards hinzu, die Ihre IoT Central-Anwendung benötigt.
- Erstellen Sie ein Gerätefunktionsmodell mit Visual Studio Code. Implementieren Sie den Gerätecode aus dem Modell, und verbinden Sie das echte Gerät mithilfe einer Geräte bevorzugenden Verbindung mit Ihrer IoT Central-Anwendung. IoT Central ermittelt und importiert das Gerätefunktionsmodell automatisch aus dem öffentlichen Repository. Sie können dann alle Cloudeigenschaften, Anpassungen und Dashboards, die Ihre IoT Central-Anwendung benötigt, der Gerätevorlage hinzufügen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial müssen Sie [eine Azure IoT Central-Anwendung erstellen](quick-deploy-iot-central.md).

## <a name="create-a-device-template-from-the-device-catalog"></a>Erstellen einer Gerätevorlage aus dem Gerätekatalog

Als Ersteller können Sie mit einem IoT Plug & Play-zertifizierten Gerät schnell mit der Erstellung Ihrer Lösung beginnen. Weitere Informationen finden Sie in der Liste im [Azure IoT-Gerätekatalog](https://catalog.azureiotsolutions.com/alldevices). IoT Central ist in den Gerätekatalog integriert, sodass Sie ein Gerätefunktionsmodell von einem dieser IoT Plug & Play-zertifizierten Geräte importieren können. Zum Erstellen einer Gerätevorlage von einem dieser Geräte in IoT Central führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Gerätevorlagen**.
1. Wählen Sie **+ Neu** und dann im Katalog eines der IoT Plug & Play-zertifizierten Geräte aus. IoT Central erstellt eine Gerätevorlage basierend auf diesem Gerätefunktionsmodell.
1. Fügen Sie der Gerätevorlage beliebige Cloudeigenschaften, Anpassungen oder Ansichten hinzu.
1. Wählen Sie **Veröffentlichen** aus, um Bedienern die Vorlage zum Anzeigen und Verbinden von Geräten zur Verfügung zu stellen.

## <a name="create-a-device-template-from-scratch"></a>Erstellen einer vollkommen neuen Gerätevorlage

Eine Gerätevorlage umfasst Folgendes:

- Ein _Gerätefunktionsmodell_, das die Telemetrie, Eigenschaften und Befehle angibt, die das Gerät implementiert. Diese Funktionen sind in einer oder mehreren Schnittstellen organisiert.
- _Cloudeigenschaften_, die Informationen definieren, die Ihre IoT Central-Anwendung über Ihre Geräte speichert. Beispielsweise kann eine Cloudeigenschaft das Datum der letzten Wartung eines Geräts erfassen. Diese Informationen werden niemals für das Gerät freigegeben.
- Durch _Anpassungen_ kann der Ersteller einige der Definitionen im Gerätefunktionsmodell überschreiben. Beispielsweise kann der Ersteller den Namen einer Geräteeigenschaft überschreiben. Eigenschaftsnamen werden in IoT Central-Dashboards und -Formularen angezeigt.
- Mithilfe von _Dashboards und Formularen_ kann der Ersteller eine Benutzeroberfläche erstellen, mit der Bediener die mit der Anwendung verbundenen Geräte überwachen und verwalten können.

Zum Erstellen einer Gerätevorlage in IoT Central führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Gerätevorlagen**.
1. Wählen Sie **+ Neu** > **Benutzerdefiniert** aus.
1. Geben Sie einen Namen für die Vorlage ein, z.B. **Umgebungssensor**.
1. Drücken Sie die **EINGABETASTE**. IoT Central erstellt eine leere Gerätevorlage.

## <a name="manage-a-device-template"></a>Verwalten einer Gerätevorlage

Sie können eine Vorlage auf der jeweiligen Startseite umbenennen oder löschen.

Nachdem Sie Ihrer Vorlage ein Gerätefunktionsmodell hinzugefügt haben, können Sie sie veröffentlichen. Erst nachdem Sie die Vorlage veröffentlicht haben, können Sie ein Gerät basierend auf dieser Vorlage verbinden, damit es den Bedienern auf der Seite **Geräte** angezeigt wird.

## <a name="create-a-capability-model"></a>Erstellen eines Funktionsmodells

Sie können ein Gerätefunktionsmodell auf folgende Arten erstellen:

- Verwenden Sie IoT Central, um ein benutzerdefiniertes Modell ohne Vorlage zu erstellen.
- Importieren Sie ein Modell aus einer JSON-Datei. Der Ersteller eines Geräts hat möglicherweise Visual Studio Code verwendet, um ein Gerätefunktionsmodell für Ihre Anwendung zu erstellen.
- Wählen Sie eines der Geräte aus dem Gerätekatalog aus. Mit dieser Option wird das Gerätefunktionsmodell importiert, das der Hersteller für dieses Gerät veröffentlicht hat. Ein auf diese Weise importiertes Gerätefunktionsmodell wird automatisch veröffentlicht.

## <a name="manage-a-capability-model"></a>Verwalten eines Funktionsmodells

Nach dem Erstellen eines Gerätefunktionsmodells haben Sie folgende Möglichkeiten:

- Fügen Sie dem Modell Schnittstellen hinzu. Ein Modell muss mindestens eine Schnittstelle aufweisen.
- Bearbeiten Sie Modellmetadaten, z. B. ID, Namespace und Name.
- Löschen Sie das Modell.

## <a name="create-an-interface"></a>Erstellen einer Schnittstelle

Eine Gerätefunktion muss mindestens eine Schnittstelle aufweisen. Eine Schnittstelle ist eine wiederverwendbare Sammlung von Funktionen.

Zum Erstellen einer Schnittstelle führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu Ihrem Gerätefunktionsmodell, und wählen Sie **+ Schnittstelle hinzufügen** aus.

1. Auf der Seite **Schnittstelle auswählen** können Sie folgende Aktionen ausführen:

    - Erstellen Sie eine benutzerdefinierte Schnittstelle ohne Vorlage.
    - Importieren Sie eine vorhandene Schnittstelle aus einer Datei. Der Ersteller eines Geräts hat möglicherweise Visual Studio Code verwendet, um eine Schnittstelle für Ihr Gerät zu erstellen.
    - Wählen Sie eine der Standardschnittstellen aus, z. B. die Schnittstelle **Geräteinformationen**. Standardschnittstellen geben die Funktionen an, die viele Geräte gemeinsam haben. Diese Standardschnittstellen werden von Azure IoT veröffentlicht und können nicht mit einer Versionsangabe versehen oder bearbeitet werden.

1. Nachdem Sie eine Schnittstelle erstellt haben, wählen Sie **Identität bearbeiten** aus, um den Anzeigenamen der Schnittstelle zu ändern.

1. Wenn Sie eine benutzerdefinierte Schnittstelle ohne Vorlage erstellen möchten, können Sie die Funktionen Ihres Geräts hinzufügen. Gerätefunktionen sind Telemetrie, Eigenschaften und Befehle.

### <a name="telemetry"></a>Telemetrie

Telemetrie ist ein Wertedatenstrom, der vom Gerät gesendet wird, üblicherweise von einem Sensor. Beispielsweise kann ein Sensor die Umgebungstemperatur melden.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Telemetriefunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Telemetriewert, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name des Felds in der Telemetrienachricht. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Funktionstyp | Telemetrie. |
| Semantischer Typ | Der semantische Typ der Telemetriedaten, z. B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Telemetriedatentyp, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. Schema ist für die semantischen Typen „Ereignis“ und „Zustand“ nicht verfügbar. |
| severity | Nur für den semantischen Typ „Ereignis“ verfügbar. Die Schweregrade lauten **Fehler**, **Information** und **Warnung**. |
| Zustandswerte | Nur für den semantischen Typ „Zustand“ verfügbar. Definieren Sie die möglichen Zustandswerte, die jeweils einen Anzeigenamen, Namen, Enumerationstyp und Wert umfassen. |
| Unit | Eine Einheit für den Telemetriewert, z. B. **km/h**, **%** oder **&deg;C**. |
| Anzeigeeinheit | Eine Anzeigeeinheit zur Verwendung in Dashboards und Formularen. |
| Comment | Beliebige Kommentare zur Telemetriefunktion. |
| BESCHREIBUNG | Eine Beschreibung der Telemetriefunktion. |

### <a name="properties"></a>Properties

Eigenschaften stellen Zeitpunktwerte dar. Ein Gerät kann beispielsweise eine Eigenschaft verwenden, um die Zieltemperatur zu melden, die es zu erreichen versucht. Sie können schreibbare Eigenschaften über IoT Central festlegen.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Eigenschaftsfunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Eigenschaftswert, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name der Eigenschaft. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Funktionstyp | Eigenschaft. |
| Semantischer Typ | Der semantische Typ der Eigenschaft, z. B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Eigenschaftsdatentyp, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. Schema ist für die semantischen Typen „Ereignis“ und „Zustand“ nicht verfügbar. |
| Schreibbar | Wenn die Eigenschaft nicht schreibbar ist, kann das Gerät Eigenschaftswerte an IoT Central melden. Ist die Eigenschaft schreibbar, kann das Gerät Eigenschaftswerte an IoT Central melden, und IoT Central kann Aktualisierungen der Eigenschaft an das Gerät senden.
| severity | Nur für den semantischen Typ „Ereignis“ verfügbar. Die Schweregrade lauten **Fehler**, **Information** und **Warnung**. |
| Zustandswerte | Nur für den semantischen Typ „Zustand“ verfügbar. Definieren Sie die möglichen Zustandswerte, die jeweils einen Anzeigenamen, Namen, Enumerationstyp und Wert umfassen. |
| Unit | Eine Einheit für den Eigenschaftswert, z. B. **km/h**, **%** oder **&deg;C**. |
| Anzeigeeinheit | Eine Anzeigeeinheit zur Verwendung in Dashboards und Formularen. |
| Comment | Beliebige Kommentare zur Eigenschaftsfunktion. |
| BESCHREIBUNG | Eine Beschreibung der Eigenschaftsfunktion. |

### <a name="commands"></a>Befehle

Sie können Gerätebefehle über IoT Central aufrufen. Befehle übergeben optional Parameter an das Gerät und empfangen eine Antwort vom Gerät. Beispielsweise können Sie einen Befehl zum Neustarten eines Geräts in 10 Sekunden aufrufen.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Befehlsfunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Befehl, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name des Befehls. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Funktionstyp | Befehl. |
| Get-Help | `SynchronousExecutionType`. |
| Comment | Beliebige Kommentare zur Befehlsfunktion. |
| BESCHREIBUNG | Eine Beschreibung der Befehlsfunktion. |
| Anforderung | Wenn aktiviert, eine Definition des Anforderungsparameters, einschließlich Name, Anzeigename, Schema, Einheit und Anzeigeeinheit. |
| response | Wenn aktiviert, eine Definition der Befehlsantwort, einschließlich Name, Anzeigename, Schema, Einheit und Anzeigeeinheit. |

## <a name="manage-an-interface"></a>Verwalten einer Schnittstelle

Wenn Sie die Schnittstelle noch nicht veröffentlicht haben, können Sie die durch die Schnittstelle definierten Funktionen bearbeiten. Nach dem Veröffentlichen der Schnittstelle müssen Sie eine neue Version der Gerätevorlage erstellen und der Schnittstelle eine Versionsangabe zuweisen, wenn Sie Änderungen vornehmen möchten. Änderungen, für die keine Versionsangabe erforderlich ist (z. B. Anzeigenamen oder Einheiten), können Sie im Abschnitt **Anpassen** vornehmen.

Sie können die Schnittstelle auch als JSON-Datei exportieren, wenn Sie sie in einem anderen Funktionsmodell wiederverwenden möchten.

## <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Verwenden Sie Cloudeigenschaften, um Informationen zu Geräten in IoT Central zu speichern. Cloudeigenschaften werden niemals an ein Gerät gesendet. Sie können Cloudeigenschaften beispielsweise verwenden, um den Namen des Kunden, der das Gerät installiert hat, oder das Datum der letzten Wartung des Geräts zu speichern.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Cloudeigenschaft angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Cloudeigenschaftswert, der in Dashboards und Formularen verwendet wird. |
| NAME | Der Name der Cloudeigenschaft. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Semantischer Typ | Der semantische Typ der Eigenschaft, z. B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Datentyp der Cloudeigenschaft, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. |

## <a name="add-customizations"></a>Hinzufügen von Anpassungen

Verwenden Sie Anpassungen, wenn Sie eine importierte Schnittstelle ändern oder IoT Central-spezifische Features zu einer Funktion hinzufügen müssen. Sie können nur Felder anpassen, die die Schnittstellenkompatibilität nicht beeinträchtigen. Sie haben beispielsweise folgende Möglichkeiten:

- Anpassen des Anzeigenamens und der Einheiten einer Funktion
- Hinzufügen einer Standardfarbe, die beim Anzeigen des Werts in einem Diagramm verwendet werden soll
- Angeben der anfänglichen, minimalen und maximalen Werte für eine Eigenschaft

Der Funktionsname oder Funktionstyp kann nicht angepasst werden. Wenn Änderungen vorgenommen werden müssen, die im Abschnitt **Anpassen** nicht möglich sind, müssen Sie die Gerätevorlage und Schnittstelle mit einer Versionsangabe versehen, um die Funktion zu ändern.

### <a name="generate-default-views"></a>Generieren von Standardansichten

Das Generieren von Standardansichten ist eine schnelle Möglichkeit, Ihre wichtigen Geräteinformationen zu visualisieren. Sie können für Ihre Gerätevorlage bis zu drei Standardansichten generieren:

- **Befehle** bietet eine Ansicht der Gerätebefehle und ermöglicht es dem Bediener, diese an Ihr Gerät auszugeben.
- **Übersicht** stellt eine Ansicht mit Gerätetelemetrie bereit, in der Diagramme und Metriken angezeigt werden.
- **Info** stellt eine Ansicht mit Geräteinformationen und Geräteeigenschaften bereit.

Nachdem Sie **Standardansichten generieren** ausgewählt haben, werden diese automatisch im Abschnitt **Ansichten** Ihrer Gerätevorlage hinzugefügt.

## <a name="add-dashboards"></a>Hinzufügen von Dashboards

Fügen Sie einer Gerätevorlage Dashboards hinzu, um Bedienern die Visualisierung eines Geräts mithilfe von Diagrammen und Metriken zu ermöglichen. Sie können über mehrere Dashboards für eine Gerätevorlage verfügen.

Zum Hinzufügen eines Dashboards zu einer Gerätevorlage führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu Ihrer Gerätevorlage, und wählen Sie **Ansichten** aus.
1. Wählen Sie **Gerät visualisieren** aus.
1. Geben Sie im Feld **Dashboardname** einen Namen für das Dashboard ein.
1. Fügen Sie dem Dashboard Kacheln aus der Liste mit statischen Kacheln und Kacheln für Eigenschaften, Cloudeigenschaften, Telemetrie und Befehle hinzu. Ziehen Sie die Kacheln, die Sie dem Dashboard hinzufügen möchten, per Drag & Drop.
1. Wenn Sie mehrere Telemetriewerte auf einer einzelnen Diagrammkachel darstellen möchten, wählen Sie die Telemetriewerte und dann **Kombinieren** aus.
1. Konfigurieren Sie jede Kachel, die Sie hinzufügen, um die Anzeige von Daten auf ihr anzupassen. Wählen Sie hierzu das Zahnradsymbol aus, oder wählen Sie auf der Diagrammkachel **Konfiguration ändern** aus.
1. Ordnen Sie die Kacheln auf Ihrem Dashboard an, und ändern Sie deren Größe.
1. Speichern Sie die Änderungen.

### <a name="configure-preview-device-to-view-dashboard"></a>Konfigurieren eines Vorschaugeräts zum Anzeigen des Dashboards

Wählen Sie **Vorschaugerät konfigurieren** aus, um das Dashboard anzuzeigen und zu testen. Dadurch können Sie das Dashboard so anzeigen, wie es nach der Veröffentlichung dem Bediener angezeigt wird. Überprüfen Sie mit dieser Option, ob in Ihren Ansichten die richtigen Daten angezeigt werden. Sie können zwischen folgenden Möglichkeiten auswählen:

- Kein Vorschaugerät
- Das tatsächliche Testgerät, das Sie für Ihre Gerätevorlage konfiguriert haben
- Ein vorhandenes Gerät in Ihrer Anwendung mithilfe der Geräte-ID

## <a name="add-forms"></a>Hinzufügen von Formularen

Fügen Sie Formulare zu einer Gerätevorlage hinzu, damit Bediener ein Gerät durch Anzeigen und Festlegen von Eigenschaften verwalten können. Bediener können nur Cloudeigenschaften und schreibbare Geräteeigenschaften bearbeiten. Sie können über mehrere Formulare für eine Gerätevorlage verfügen.

Zum Hinzufügen eines Formulars zu einer Gerätevorlage führen Sie die folgenden Schritte aus:

1. Navigieren Sie zu Ihrer Gerätevorlage, und wählen Sie **Ansichten** aus.
1. Wählen Sie **Geräte- und Clouddaten bearbeiten** aus.
1. Geben Sie im Feld **Formularname** einen Namen für das Formular ein.
1. Wählen Sie die Anzahl der Spalten aus, die für das Layout des Formulars verwendet werden sollen.
1. Fügen Sie einem vorhandenen Abschnitt im Formular Eigenschaften hinzu, oder wählen Sie Eigenschaften und dann **Abschnitt hinzufügen** aus. Verwenden Sie Abschnitte, um Eigenschaften auf dem Formular zu gruppieren. Sie können einem Abschnitt einen Titel hinzufügen.
1. Konfigurieren Sie die einzelnen Eigenschaften auf dem Formular, um deren Verhalten anzupassen.
1. Ordnen Sie die Eigenschaften auf dem Formular an.
1. Speichern Sie die Änderungen.

## <a name="publish-a-device-template"></a>Veröffentlichen einer Gerätevorlage

Bevor Sie ein Gerät verbinden können, das Ihr Gerätefunktionsmodell implementiert, müssen Sie die Gerätevorlage veröffentlichen.

Nachdem Sie eine Gerätevorlage veröffentlicht haben, können Sie nur eingeschränkte Änderungen am Gerätefunktionsmodell vornehmen. Zum Ändern einer Schnittstelle müssen Sie [eine neue Version erstellen und veröffentlichen](./howto-version-device-template.md).

Wenn Sie eine Gerätevorlage veröffentlichen möchten, navigieren Sie zu der Gerätevorlage, und wählen Sie **Veröffentlichen** aus.

Nachdem Sie eine Gerätevorlage veröffentlicht haben, kann ein Bediener zur Seite **Geräte** wechseln und echte oder simulierte Geräte hinzufügen, die Ihre Gerätevorlage verwenden. Sie können Ihre Gerätevorlage weiter bearbeiten und speichern, während Sie Änderungen vornehmen. Immer wenn Sie diese Änderungen auf der Seite **Geräte** an die Bediener übermitteln möchten, müssen Sie **Veröffentlichen** auswählen.

## <a name="define-a-new-iot-gateway-device-type-preview-features"></a>Definieren eines neuen IoT-Gatewaygerätetyps (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In diesem Tutorial für Ersteller erfahren Sie, wie Sie in Ihrer IoT Central-Anwendung mithilfe einer Gatewaygerätevorlage einen neuen Typ von IoT-Geräten definieren. 

In diesem Abschnitt wird die Gerätevorlage **Smart Building** erstellt. Ein Gatewaygerät für intelligente Gebäude zeichnet sich durch Folgendes aus:

* Es sendet Telemetriedaten wie Temperatur und Auslastung.
* Es reagiert auf schreibbare Eigenschaften, wenn diese in der Cloud aktualisiert werden, z. B. Sendeintervall für Telemetriedaten.
* Es reagiert auf Befehle, z. B. Zurücksetzen der Temperatur.
* Es lässt Beziehungen mit anderen Gerätefunktionsmodellen zu.

### <a name="create-iot-device-templates"></a>Erstellen von IoT-Gerätevorlagen

IoT-Gerätevorlagen werden wir folgt erstellt: 

1. Wählen Sie im linken Navigationsbereich **Gerätevorlagen** aus. Wählen Sie dann **+ Neu**, anschließend die Kachel **IoT-Gerät** und dann die Kachel für den Auslastungssensor aus. Klicken Sie auf **Weiter: Anpassen**.

   ![Screenshot der Seite „Gerätevorlagen“ mit Optionen](./media/tutorial-define-iot-device-type/gateway-downstream-new.png)

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus. 

   ![Screenshot der Seite „Überprüfen“](./media/tutorial-define-iot-device-type/gateway-downstream-review.png)

1. Eine neue Gerätevorlage wird erstellt. 

   ![Screenshot einer neuen Gerätevorlage](./media/tutorial-define-iot-device-type/occupancy-sensor.png)

Eine Gerätevorlage für den S1-Sensor wird wie folgt erstellt:

1. Wählen Sie im linken Navigationsbereich **Gerätevorlagen** aus. Wählen Sie dann **+ Neu**, anschließend die Kachel **IoT-Gerät** und dann die Kachel für den Auslastungssensor aus. Klicken Sie auf **Weiter: Anpassen**.

   ![Screenshot der Seite „Gerätevorlagen“ mit Optionen](./media/tutorial-define-iot-device-type/s1-sensor.png)

1. Wählen Sie auf der Seite **Überprüfen** die Option **Erstellen** aus. 

   ![Screenshot der Seite „Überprüfen“](./media/tutorial-define-iot-device-type/s1-review.png)

1. Eine neue Gerätevorlage wird erstellt. 

   ![Screenshot einer neuen Gerätevorlage](./media/tutorial-define-iot-device-type/s1-template.png)

## <a name="create-an-iot-gateway-device-template"></a>Erstellen einer IoT-Gatewaygerätevorlage

Sie haben die Möglichkeit, eine IoT-Gatewaygerätevorlage zu erstellen. Das Gatewaygerät weist Beziehungen mit nachgeschalteten Geräten auf, die über das Gatewaygerät eine Verbindung mit IoT Central herstellen. 

### <a name="downstream-device-relationships-with-gateway-device"></a>Beziehungen nachgeschalteter Geräte mit dem Gatewaygerät

IoT-Geräte können Verbindungen mit IoT-Gatewaygeräten herstellen.

![Diagramm der Beziehung zwischen Gatewaygerät und nachgeschalteten Geräten](./media/tutorial-define-iot-device-type/gatewaypattern.png)

Als Ersteller können Sie IoT-Gatewaygerätevorlagen in Ihrer Anwendung erstellen und bearbeiten. Nachdem Sie eine Gerätevorlage veröffentlicht haben, können Sie eine Verbindung mit echten Geräten herstellen, die die Gerätevorlage implementieren.

### <a name="select-a-device-template-type"></a>Auswählen eines Gerätevorlagentyps 

So fügen Sie Ihrer Anwendung eine neue Gerätevorlage hinzu

1. Wählen Sie im linken Bereich die Registerkarte **Gerätevorlagen** aus.

   ![Screenshot der Seite „Gerätevorlagen“](./media/tutorial-define-iot-device-type/devicetemplate.png)

1. Wählen Sie **+ Neu** aus, um mit der Erstellung einer neuen Gerätevorlage zu beginnen.

   ![Screenshot der Seite „Gerätevorlagen“ mit markierter Option „Neu“](./media/tutorial-define-iot-device-type/devicetemplatenew.png)

   ![Screenshot der Seite „Gerät anpassen“](./media/tutorial-define-iot-device-type/gateway-review.png)

1. Wählen Sie auf der Seite **Vorlagentyp auswählen** die Option **Azure IoT** aus und dann **Weiter: Anpassen**.

   ![Screenshot der Seite „Vorlagentyp auswählen“](./media/tutorial-define-iot-device-type/gateway-customize.png)

1. Aktivieren Sie das Kontrollkästchen „Gateway“, und wählen Sie **Erstellen** aus.

   ![Screenshot der Seite „Gerät anpassen“ mit hervorgehobenem Gateway](./media/tutorial-define-iot-device-type/gateway-review.png)

1. Wählen Sie auf der Seite „Überprüfen“ die Option **Erstellen** aus. 

1. Geben Sie für die Gatewayvorlage den Namen **Smart Building Gateway Template** ein. Wählen Sie die Kachel **Benutzerdefiniert** aus.

1. Fügen Sie eine Standardschnittstelle vom Typ **Geräteinformationen** hinzu.

### <a name="add-relationships"></a>Hinzufügen von Beziehungen

Sie können für Geräte, die Sie mit einem Gatewaygerät verbinden, Downstreambeziehungen mit Gerätefunktionsmodellen hinzufügen.

Erstellen Sie Beziehungen mit nachgeschalteten Gerätefunktionsmodellen. Wählen Sie **Speichern** aus.

![Screenshot der Vorlage „Smart Building Gateway Template“ mit verschiedenen markierten Optionen](./media/tutorial-define-iot-device-type/gateway-occupancy-s1-rel.png)

### <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Eine Gerätevorlage kann Cloudeigenschaften enthalten. Cloudeigenschaften sind nur in der IoT Central-Anwendung vorhanden und werden niemals an ein Gerät gesendet oder von einem Gerät empfangen.

1. Wählen Sie **Cloudeigenschaften** >  **+ Cloudeigenschaft hinzufügen** aus. Verwenden Sie die Informationen in der folgenden Tabelle, um Ihrer Gerätevorlage eine Cloudeigenschaft hinzuzufügen.

    | `Display name`      | Semantischer Typ | Schema |
    | ----------------- | ------------- | ------ |
    | Datum der letzten Wartung | Keine          | Date   |
    | Kundenname     | Keine          | Zeichenfolge |

2. Wählen Sie **Speichern** aus.

### <a name="add-customizations"></a>Hinzufügen von Anpassungen

Verwenden Sie Anpassungen, um eine Schnittstelle zu ändern oder einer Funktion, für die Sie Ihr Gerätefunktionsmodell nicht mit einer Versionsangabe versehen müssen, IoT Central-spezifische Features hinzuzufügen. Sie können Felder anpassen, wenn sich das Funktionsmodell im Entwurfszustand oder veröffentlichten Zustand befindet. Sie können nur Felder anpassen, die die Schnittstellenkompatibilität nicht beeinträchtigen. Sie haben beispielsweise folgende Möglichkeiten:

- Anpassen des Anzeigenamens und der Einheiten einer Funktion
- Hinzufügen einer Standardfarbe, die beim Anzeigen des Werts in einem Diagramm verwendet werden soll
- Angeben der anfänglichen, minimalen und maximalen Werte für eine Eigenschaft

(Der Funktionsname oder -typ kann nicht angepasst werden.)

Wählen Sie nach Abschluss der Anpassung **Speichern** aus.

### <a name="create-views"></a>Erstellen von Ansichten

Als Ersteller können Sie die Anwendung so anpassen, dass relevante Informationen zum Umgebungssensorgerät einem Bediener angezeigt werden. Ihre Anpassungen ermöglichen dem Bediener die Verwaltung des mit der Anwendung verbundenen Umgebungssensors. Sie können zwei Arten von Ansichten erstellen, die von einem Bediener zum Interagieren mit Geräten verwendet werden:

* Formulare zum Anzeigen und Bearbeiten von Geräte- und Cloudeigenschaften
* Dashboards zum Visualisieren von Geräten

### <a name="generate-default-views"></a>Generieren von Standardansichten

Wenn Sie **Standardansichten generieren** auswählen, können Sie die Dashboards **Übersicht** und **Info** generieren. 

## <a name="publish-a-device-template"></a>Veröffentlichen einer Gerätevorlage

Bevor Sie einen simulierten Umgebungssensor erstellen oder einen echten Umgebungssensor verbinden können, müssen Sie Ihre Gerätevorlage veröffentlichen.

Veröffentlichen Sie wie folgt eine Gerätevorlage:

1. Navigieren Sie über die Seite **Gerätevorlagen** zu Ihrer Gerätevorlage.

2. Wählen Sie **Veröffentlichen**.

3. Wählen Sie im Dialogfeld **Publish a Device Template** (Gerätevorlage veröffentlichen) die Option **Veröffentlichen** aus.

Nachdem eine Gerätevorlage veröffentlicht wurde, wird sie auf der Seite **Geräte** und für den Bediener angezeigt. In einer veröffentlichten Gerätevorlage können Sie ein Gerätefunktionsmodell nicht bearbeiten, ohne eine neue Versionsnummer zu erstellen. Sie können jedoch Cloudeigenschaften, Anpassungen und Ansichten in einer veröffentlichten Gerätevorlage aktualisieren. Diese Aktualisierungen bewirken nicht das Erstellen einer neuen Version. Wählen Sie nach dem Vornehmen von Änderungen die Option **Veröffentlichen**, um diese Änderungen für Ihren Bediener bereitzustellen.

## <a name="create-a-gateway-simulated-device"></a>Erstellen eines simulierten Gatewaygeräts

Erstellen Sie im Geräte-Explorer ein simuliertes Gateway für intelligente Gebäude. 

![Screenshot des Dialogfelds „Neues Gerät erstellen“](./media/tutorial-define-iot-device-type/smartbuildingdevice.png)

## <a name="create-downstream-simulated-devices"></a>Erstellen nachgeschalteter simulierter Geräte

Erstellen Sie im Geräte-Explorer einen simulierten Auslastungssensor. 

![Screenshot des Dialogfelds „Neues Gerät erstellen“](./media/tutorial-define-iot-device-type/occupancydevice.png)

Erstellen Sie im Geräte-Explorer einen simulierten S1-Sensor. 

![Screenshot des Dialogfelds „Neues Gerät erstellen“](./media/tutorial-define-iot-device-type/s1device.png)

## <a name="add-downstream-devices-relationships-to-a-gateway-device"></a>Hinzufügen von Beziehungen nachgeschalteter Geräte mit einem Gatewaygerät

Wählen Sie den S1-Sensor und den Auslastungssensor und dann **Connect to gateway** (Mit Gateway verbinden) aus. 

![Screenshot des Auslastungssensors mit hervorgehobener Option „Connect to gateway“ (Mit Gateway verbinden)](./media/tutorial-define-iot-device-type/connecttogateway.png)

Wählen Sie eine Gatewaygerätevorlage und eine Gatewaygeräteinstanz und dann **Verbinden** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

* Erstellen eines neuen IoT-Gateways als Gerätevorlage
* Erstellen von Cloudeigenschaften
* Erstellen von Anpassungen
* Definieren einer Visualisierung für die Gerätetelemetriedaten
* Hinzufügen von Beziehungen
* Veröffentlichen Ihrer Gerätevorlage

Als Nächstes haben Sie folgende Möglichkeiten:

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einem Gerät](tutorial-connect-pnp-device.md)
