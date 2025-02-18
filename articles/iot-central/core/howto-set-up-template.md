---
title: Definieren eines neuen IoT-Gerätetyps in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie in Ihrer Azure IoT Central-Anwendung eine neue Azure IoT-Gerätevorlage erstellen. Sie definieren die Telemetriedaten, den Zustand, die Eigenschaften und die Befehle für den Typ.
author: dominicbetts
ms.author: dobett
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom:
- contperf-fy21q1
- device-developer
ms.openlocfilehash: d2754200cb41114aafbe1bea2b511ed743280b88
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110061190"
---
# <a name="define-a-new-iot-device-type-in-your-azure-iot-central-application"></a>Definieren eines neuen IoT-Gerätetyps in Ihrer Azure IoT Central-Anwendung

Eine Gerätevorlage ist eine Blaupause, die die Merkmale und das Verhalten eines Gerätetyps definiert, der eine Verbindung mit einer [Azure IoT Central-Anwendung](concepts-app-templates.md) herstellt.

Beispielsweise kann ein Hersteller eine Gerätevorlage für einen verbundenen Lüfter mit folgenden Merkmalen erstellen:

- Sendet Temperaturtelemetriedaten
- Sendet Standorteigenschaft
- Sendet Lüftermotor-Fehlerereignisse
- Sendet Lüfterbetriebszustand
- Bietet eine schreibbare Eigenschaft für die Lüftergeschwindigkeit
- Bietet einen Befehl zum Neustarten des Geräts
- Gibt Ihnen mithilfe einer Ansicht eine allgemeine Übersicht über das Gerät

Anhand dieser Gerätevorlage kann ein Bediener echte Lüftergeräte erstellen und verbinden. Alle diese Lüfter weisen Messungen, Eigenschaften und Befehle auf, die von Bedienern zum Überwachen und Verwalten verwendet werden. Bediener verwenden die [Geräteansichten](#add-views) und -formulare zum Interagieren mit den Lüftergeräten. Ein Geräteentwickler verwendet die Vorlage, um zu verstehen, wie das Gerät mit der Anwendung interagiert. Weitere Informationen finden Sie unter [Telemetrie-, Eigenschaften- und Befehlsnutzlasten](concepts-telemetry-properties-commands.md).

> [!NOTE]
> Nur Ersteller und Administratoren können Gerätevorlagen erstellen, bearbeiten und löschen. Auf der Seite **Geräte** kann jeder Benutzer Geräte anhand vorhandener Gerätevorlagen erstellen.

In einer IoT Central-Anwendung verwendet eine Gerätevorlage ein Gerätemodell, um die Funktionen eines Geräts zu beschreiben. Als Ersteller haben Sie mehrere Möglichkeiten zum Erstellen von Gerätevorlagen:

- Entwerfen Sie die Gerätevorlage in IoT Central, und [implementieren Sie dann das entsprechende Gerätemodell in Ihrem Gerätecode](concepts-telemetry-properties-commands.md).
- Importieren Sie eine Gerätevorlage aus dem [Azure Certified for IoT-Gerätekatalog](https://aka.ms/iotdevcat). Passen Sie die Gerätevorlage Ihren Anforderungen in IoT Central an.
> [!NOTE]
> Weil IoT Central das vollständige Modell mit allen referenzierten Schnittstellen in derselben Datei erfordert, verwenden Sie beim Importieren eines Modells aus dem Modellrepository das Schlüsselwort "Erweitert", um die Vollversion zu erhalten.
Beispiel: https://devicemodels.azure.com/dtmi/com/example/thermostat-1.expanded.json

- Erstellen Sie ein Gerätemodell mithilfe von [Digital Twins Definition Language (DTDL) – Version 2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md). Visual Studio Code weist eine Erweiterung auf, die das Erstellen von DTDL-Modellen unterstützt. Weitere Informationen finden Sie unter [Installieren und Verwenden der DTDL-Erstellungstools](../../iot-pnp/howto-use-dtdl-authoring-tools.md). Veröffentlichen Sie das Modell dann im öffentlichen Modellrepository. Weitere Informationen finden Sie unter [Gerätemodellrepository](../../iot-pnp/concepts-model-repository.md). Implementieren Sie den Gerätecode aus dem Modell, und verbinden Sie Ihr reales Gerät mit Ihrer IoT Central-Anwendung. IoT Central ermittelt das Gerätemodell, importiert es für Sie aus dem öffentlichen Repository und generiert eine Gerätevorlage. Dann können Sie alle Cloudeigenschaften, Anpassungen und Ansichten, die Ihre IoT Central-Anwendung benötigt, der Gerätevorlage hinzufügen.
- Erstellen Sie ein Gerätemodell per DTDL. Implementieren Sie Ihren Gerätecode aus dem Modell. Importieren Sie das Gerätemodell manuell in Ihre IoT Central-Anwendung, und fügen Sie dann alle Cloudeigenschaften, Anpassungen und Ansichten hinzu, die Ihre IoT Central-Anwendung benötigt.

> [!TIP]
> IoT Central erfordert das vollständige Modell mit allen referenzierten Schnittstellen in derselben Datei. Wenn Sie ein Modell aus dem Modellrepository importieren, verwenden Sie das Schlüsselwort *erweitert*, um die Vollversion zu erhalten.
> Beispiel: [https://devicemodels.azure.com/dtmi/com/example/thermostat-1.expanded.json](https://devicemodels.azure.com/dtmi/com/example/thermostat-1.expanded.json)

Sie können einer IoT Central-Anwendung Gerätevorlagen auch mithilfe der [REST-API](/learn/modules/manage-iot-central-apps-with-rest-api/) oder der [CLI](howto-manage-iot-central-from-cli.md) hinzufügen.

Einige [Anwendungsvorlagen](concepts-app-templates.md) enthalten bereits Gerätevorlagen, die in dem von der Anwendungsvorlage unterstützten Szenario hilfreich sind. Informationen hierzu finden Sie beispielsweise unter [Architektur der In-Store-Analyse](../retail/store-analytics-architecture.md).

## <a name="create-a-device-template-from-the-device-catalog"></a>Erstellen einer Gerätevorlage aus dem Gerätekatalog

Als Ersteller können Sie mit einem zertifizierten Gerät schnell mit der Erstellung Ihrer Lösung beginnen. Weitere Informationen finden Sie in der Liste im [Azure IoT-Gerätekatalog](https://devicecatalog.azure.com). IoT Central ist in den Gerätekatalog integriert, sodass Sie ein Gerätemodell von einem dieser zertifizierten Geräte importieren können. Zum Erstellen einer Gerätevorlage von einem dieser Geräte in IoT Central führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Gerätevorlagen**.
1. Wählen Sie **+ Neu** und dann im Katalog eines der zertifizierten Geräte aus. IoT Central erstellt eine Gerätevorlage basierend auf diesem Gerätemodell.
1. Fügen Sie der Gerätevorlage beliebige Cloudeigenschaften, Anpassungen oder Ansichten hinzu.
1. Wählen Sie **Veröffentlichen** aus, um Bedienern die Vorlage zum Anzeigen und Verbinden von Geräten zur Verfügung zu stellen.

## <a name="create-a-device-template-from-scratch"></a>Erstellen einer vollkommen neuen Gerätevorlage

Eine Gerätevorlage umfasst Folgendes:

- Ein _Gerätemodell_, das die Telemetrie, Eigenschaften und Befehle angibt, die das Gerät implementiert. Diese Funktionen sind in einer oder mehreren Komponenten organisiert.
- _Cloudeigenschaften_, die Informationen definieren, die Ihre IoT Central-Anwendung über Ihre Geräte speichert. Beispielsweise kann eine Cloudeigenschaft das Datum der letzten Wartung eines Geräts erfassen. Diese Informationen werden niemals für das Gerät freigegeben.
- Durch _Anpassungen_ kann der Ersteller einige der Definitionen im Gerätemodell überschreiben. Beispielsweise kann der Ersteller den Namen einer Geräteeigenschaft überschreiben. Eigenschaftsnamen werden in IoT Central-Ansichten und -Formularen angezeigt.
- Mithilfe von _Ansichten und Formularen_ kann der Ersteller eine Benutzeroberfläche erstellen, mit der Bediener die mit Ihrer Anwendung verbundenen Geräte überwachen und verwalten können.

Zum Erstellen einer Gerätevorlage in IoT Central führen Sie die folgenden Schritte aus:

1. Navigieren Sie in Ihrer IoT Central-Anwendung zur Seite **Gerätevorlagen**.
1. Wählen Sie **+ Neu** > **IoT-Gerät** aus. Wählen Sie anschließend **Next: Anpassen**.
1. Geben Sie einen Namen für die Vorlage ein, z. B. **Thermostat**. Wählen Sie anschließend **Next: Überprüfen** und anschließend **Erstellen** aus.
1. IoT Central erstellt eine leere Gerätevorlage und ermöglicht Ihnen, ein benutzerdefiniertes Modell von Grund auf neu zu erstellen oder ein DTDL-Modell zu importieren.

## <a name="manage-a-device-template"></a>Verwalten einer Gerätevorlage

Sie können eine Vorlage auf der jeweiligen Editor-Seite umbenennen oder löschen.

Nachdem Sie die Vorlage definiert haben, können Sie sie veröffentlichen. Bis die Vorlage veröffentlicht wurde, können Sie kein Gerät damit verbinden, und das Gerät wird auf der Seite **Geräte** nicht angezeigt.

Weitere Informationen zum Ändern von Gerätevorlagen finden Sie unter [Bearbeiten einer vorhandenen Gerätevorlage](howto-edit-device-template.md).

## <a name="create-a-capability-model"></a>Erstellen eines Funktionsmodells

Um ein Gerätemodell zu erstellen, haben Sie folgende Möglichkeiten:

- Verwenden Sie IoT Central, um ein benutzerdefiniertes Modell ohne Vorlage zu erstellen.
- Importieren Sie ein DTDL-Modell aus einer JSON-Datei. Der Ersteller eines Geräts hat möglicherweise Visual Studio Code verwendet, um ein Gerätemodell für Ihre Anwendung zu erstellen.
- Wählen Sie eines der Geräte aus dem Gerätekatalog aus. Mit dieser Option wird das Gerätemodell importiert, das der Hersteller für dieses Gerät veröffentlicht hat. Ein auf diese Weise importiertes Gerätemodell wird automatisch veröffentlicht.

## <a name="manage-a-capability-model"></a>Verwalten eines Funktionsmodells

Nach dem Erstellen eines Gerätemodells haben Sie folgende Möglichkeiten:

- Fügen Sie dem Modell Komponenten hinzu. Ein Modell muss mindestens eine Komponente aufweisen.
- Bearbeiten Sie Modellmetadaten, z. B. ID, Namespace und Name.
- Löschen Sie das Modell.

## <a name="create-a-component"></a>Erstellen einer Komponente

Ein Gerätemodell muss mindestens eine Standardkomponente aufweisen. Eine Komponente ist eine wiederverwendbare Sammlung von Funktionen.

So erstellen Sie eine Komponente:

1. Wechseln Sie zu Ihrem Gerätemodell, und wählen Sie **+ Komponente hinzufügen** aus.

1. Auf der Seite **Komponentenschnittstelle hinzufügen** können Sie folgende Aktionen ausführen:

    - Erstellen einer neuen benutzerdefinierten Komponente.
    - Importieren einer vorhandenen Komponente aus einer DTDL-Datei. Der Ersteller eines Geräts hat möglicherweise Visual Studio Code verwendet, um eine Komponentenschnittstelle für Ihr Gerät zu erstellen.

1. Nachdem Sie eine Komponente erstellt haben, wählen Sie **Identität bearbeiten** aus, um den Anzeigenamen der Komponente zu ändern.

1. Wenn Sie eine benutzerdefinierte Komponente ohne Vorlage erstellen möchten, können Sie die Funktionen Ihres Geräts hinzufügen. Gerätefunktionen sind Telemetrie, Eigenschaften und Befehle.

### <a name="telemetry"></a>Telemetrie

Telemetrie ist ein Wertedatenstrom, der vom Gerät gesendet wird, üblicherweise von einem Sensor. Beispielsweise kann ein Sensor die Umgebungstemperatur melden.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Telemetriefunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Telemetriewert, der in Ansichten und Formularen verwendet wird. |
| Name | Der Name des Felds in der Telemetrienachricht. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. Dieses Feld muss alphanumerisch sein. |
| Funktionstyp | Telemetrie. |
| Semantischer Typ | Der semantische Typ der Telemetriedaten, z. B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Telemetriedatentyp, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. Schema ist für die semantischen Typen „Ereignis“ und „Zustand“ nicht verfügbar. |
| severity | Nur für den semantischen Typ „Ereignis“ verfügbar. Die Schweregrade lauten **Fehler**, **Information** und **Warnung**. |
| Zustandswerte | Nur für den semantischen Typ „Zustand“ verfügbar. Definieren Sie die möglichen Zustandswerte, die jeweils einen Anzeigenamen, Namen, Enumerationstyp und Wert umfassen. |
| Einheit | Eine Einheit für den Telemetriewert, z. B. **km/h**, **%** oder **&deg;C**. |
| Anzeigeeinheit | Eine Anzeigeeinheit zur Verwendung in Ansichten und Formularen. |
| Comment | Beliebige Kommentare zur Telemetriefunktion. |
| BESCHREIBUNG | Eine Beschreibung der Telemetriefunktion. |

### <a name="properties"></a>Eigenschaften

Eigenschaften stellen Zeitpunktwerte dar. Ein Gerät kann beispielsweise eine Eigenschaft verwenden, um die Zieltemperatur zu melden, die es zu erreichen versucht. Sie können schreibbare Eigenschaften über IoT Central festlegen.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Eigenschaftsfunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Eigenschaftswert, der in Ansichten und Formularen verwendet wird. |
| Name | Der Name der Eigenschaft. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. Dieses Feld muss alphanumerisch sein. |
| Funktionstyp | Eigenschaft. |
| Semantischer Typ | Der semantische Typ der Eigenschaft, z. B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Eigenschaftsdatentyp, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. Schema ist für die semantischen Typen „Ereignis“ und „Zustand“ nicht verfügbar. |
| Schreibbar | Wenn die Eigenschaft nicht schreibbar ist, kann das Gerät Eigenschaftswerte an IoT Central melden. Ist die Eigenschaft schreibbar, kann das Gerät Eigenschaftswerte an IoT Central melden, und IoT Central kann Aktualisierungen der Eigenschaft an das Gerät senden.
| severity | Nur für den semantischen Typ „Ereignis“ verfügbar. Die Schweregrade lauten **Fehler**, **Information** und **Warnung**. |
| Zustandswerte | Nur für den semantischen Typ „Zustand“ verfügbar. Definieren Sie die möglichen Zustandswerte, die jeweils einen Anzeigenamen, Namen, Enumerationstyp und Wert umfassen. |
| Einheit | Eine Einheit für den Eigenschaftswert, z. B. **km/h**, **%** oder **&deg;C**. |
| Anzeigeeinheit | Eine Anzeigeeinheit zur Verwendung in Ansichten und Formularen. |
| Comment | Beliebige Kommentare zur Eigenschaftsfunktion. |
| BESCHREIBUNG | Eine Beschreibung der Eigenschaftsfunktion. |

### <a name="commands"></a>Befehle

Sie können Gerätebefehle über IoT Central aufrufen. Befehle übergeben optional Parameter an das Gerät und empfangen eine Antwort vom Gerät. Beispielsweise können Sie einen Befehl zum Neustarten eines Geräts in 10 Sekunden aufrufen.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Befehlsfunktion angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Befehl, der in Ansichten und Formularen verwendet wird. |
| Name | Der Name des Befehls. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. Dieses Feld muss alphanumerisch sein. |
| Funktionstyp | Befehl. |
| Warteschlange (wenn offline) | Wenn diese Option aktiviert ist, können Sie den Befehl sogar dann aufrufen, wenn das Gerät offline ist. Wenn sie nicht aktiviert ist, können Sie den Befehl nur aufrufen, wenn das Gerät online ist. |
| Comment | Beliebige Kommentare zur Befehlsfunktion. |
| BESCHREIBUNG | Eine Beschreibung der Befehlsfunktion. |
| Anforderung | Wenn aktiviert, eine Definition des Anforderungsparameters, einschließlich Name, Anzeigename, Schema, Einheit und Anzeigeeinheit. |
| Antwort | Wenn aktiviert, eine Definition der Befehlsantwort, einschließlich Name, Anzeigename, Schema, Einheit und Anzeigeeinheit. |

Weitere Informationen zur Implementierung von Befehlen durch Geräte finden Sie unter [Telemetrie-, Eigenschaften- und Befehlsnutzlasten – Befehle](concepts-telemetry-properties-commands.md#commands).

#### <a name="offline-commands"></a>Offlinebefehle

Wenn ein Gerät zurzeit offline ist, können Sie Warteschlangenbefehle auswählen, indem Sie in der Gerätevorlage die Option **Warteschlange (falls offline)** für einen Befehl aktivieren.

Bei dieser Option werden Benachrichtigungen mithilfe von IoT Hub-Cloud-zu-Gerät (C2D)-Nachrichten an Geräte gesendet. Weitere Informationen finden Sie im IoT Hub-Artikel [Senden von C2D-Nachrichten](../../iot-hub/iot-hub-devguide-messages-c2d.md).

Cloud-zu-Gerät-Nachrichten:

- Sind unidirektionale Benachrichtigungen aus Ihrer Lösung an das Gerät.
- Gewährleisten mindestens einmal eine Nachrichtenübermittlung. IoT Hub speichert Cloud-zu-Gerät-Nachrichten in Warteschlangen pro Gerät, und gewährleistet so Resilienz bei Verbindungs- und Gerätefehlern.
- Erfordert, dass das Gerät einen Meldungshandler zur Verarbeitung der Cloud-zu-Gerät-Nachricht implementiert.

> [!NOTE]
> Diese Option steht nur in der IoT Central-Webbenutzeroberfläche zur Verfügung. Diese Einstellung ist nicht enthalten, wenn Sie ein Modell oder eine Komponente aus der Gerätevorlage exportieren.

## <a name="manage-a-component"></a>Verwalten einer Komponente

Verwenden Sie Komponenten, um eine Gerätevorlage aus anderen Schnittstellen zusammenzustellen. Die Gerätevorlage für einen Temperaturcontroller beispielsweise könnte mehrere Thermostatkomponenten enthalten. Komponenten können direkt in der Gerätevorlage bearbeitet oder als JSON-Dateien exportiert und importiert werden. Geräte können mit Komponenteninstanzen interagieren. Beispielsweise kann ein Gerät mit zwei Thermostaten Telemetriedaten von jedem Thermostat an separate Komponenten in Ihrer IoT Central-Anwendung senden.

## <a name="inheritance"></a>Vererbung

Sie können eine Schnittstelle mithilfe von Vererbung erweitern. Mithilfe von Vererbung können Sie auch vorhandenen Schnittstellen Funktionen hinzufügen. Geerbte Schnittstellen sind für Geräte transparent.

## <a name="add-cloud-properties"></a>Hinzufügen von Cloudeigenschaften

Verwenden Sie Cloudeigenschaften, um Informationen zu Geräten in IoT Central zu speichern. Cloudeigenschaften werden niemals an ein Gerät gesendet. Sie können Cloudeigenschaften beispielsweise verwenden, um den Namen des Kunden, der das Gerät installiert hat, oder das Datum der letzten Wartung des Geräts zu speichern.

In der folgenden Tabelle sind die Konfigurationseinstellungen für eine Cloudeigenschaft angegeben:

| Feld | BESCHREIBUNG |
| ----- | ----------- |
| Anzeigename | Der Anzeigename für den Cloudeigenschaftswert, der in Ansichten und Formularen verwendet wird. |
| Name | Der Name der Cloudeigenschaft. IoT Central generiert einen Wert für dieses Feld aus dem Anzeigenamen, Sie können aber ggf. einen eigenen Wert auswählen. |
| Semantischer Typ | Der semantische Typ der Eigenschaft, z. B. Temperatur, Zustand oder Ereignis. Die Auswahl des semantischen Typs bestimmt, welches der folgenden Felder verfügbar ist. |
| Schema | Der Datentyp der Cloudeigenschaft, z.B. „double“, „string“ oder „vector“. Die verfügbaren Optionen werden durch den semantischen Typ bestimmt. |

## <a name="add-customizations"></a>Hinzufügen von Anpassungen

Verwenden Sie Anpassungen, wenn Sie eine importierte Komponente ändern oder IoT Central-spezifische Features einer Funktion hinzufügen müssen. Sie können jeden beliebigen Teil der Funktionen einer vorhandenen Gerätevorlage anpassen.

### <a name="generate-default-views"></a>Generieren von Standardansichten

Das Generieren von Standardansichten ist eine schnelle Möglichkeit, Ihre wichtigen Geräteinformationen zu visualisieren. Sie können für Ihre Gerätevorlage bis zu drei Standardansichten generieren:

- **Befehle:** Eine Ansicht der Gerätebefehle, die dem Bediener ermöglicht, diese an Ihr Gerät auszugeben.
- **Übersicht**: Eine Ansicht mit Gerätetelemetrie, in der Diagramme und Metriken angezeigt werden.
- **Info**: Eine Ansicht mit Geräteinformationen und -eigenschaften.

Nachdem Sie **Standardansichten generieren** ausgewählt haben, werden diese automatisch im Abschnitt **Ansichten** Ihrer Gerätevorlage hinzugefügt.

## <a name="add-views"></a>Hinzufügen von Ansichten

Fügen Sie einer Gerätevorlage Ansichten hinzu, um Bedienern die Visualisierung eines Geräts mithilfe von Diagrammen und Metriken zu ermöglichen. Bei einer Gerätevorlage kann es mehrere Ansichten geben.

So fügen Sie einer Gerätevorlage eine Ansicht hinzu:

1. Navigieren Sie zu Ihrer Gerätevorlage, und wählen Sie **Ansichten** aus.
1. Wählen Sie **Gerät visualisieren** aus.
1. Geben Sie in **Anzeigename** einen Namen für Ihre Ansicht ein.
1. Fügen Sie der Ansicht Kacheln aus der Liste mit statischen Kacheln sowie mit Kacheln für Eigenschaften, Cloudeigenschaften, Telemetrie und Befehle hinzu. Verschieben Sie die Kacheln, die Sie Ihrer Ansicht hinzufügen möchten, per Drag & Drop.
1. Wenn Sie mehrere Telemetriewerte auf einer einzelnen Diagrammkachel darstellen möchten, wählen Sie die Telemetriewerte und dann **Kombinieren** aus.
1. Konfigurieren Sie jede Kachel, die Sie hinzufügen, um die Anzeige von Daten auf ihr anzupassen. Greifen Sie auf diese Option zu, indem Sie das Zahnradsymbol auswählen, oder wählen Sie auf der Diagrammkachel **Konfiguration ändern** aus.
1. Ordnen Sie die Kacheln auf Ihrer Ansicht an, und ändern Sie die Kachelgröße.
1. Speichern Sie die Änderungen.

### <a name="configure-preview-device-to-view"></a>Konfigurieren eines Vorschaugeräts zum Anzeigen der Ansicht

Wählen Sie **Vorschaugerät konfigurieren** aus, um Ihre Ansicht anzuzeigen und zu testen. Mit diesem Feature können Sie die Ansicht so anzeigen, wie sie dem Bediener nach ihrer Veröffentlichung angezeigt wird. Überprüfen Sie mit diesem Feature, ob in Ihren Ansichten die richtigen Daten angezeigt werden. Sie können eine der folgenden Optionen auswählen:

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

Bevor Sie eine Verbindung mit einem Gerät herstellen können, das Ihr Gerätemodell implementiert, müssen Sie die Gerätevorlage veröffentlichen.

Weitere Informationen zum Ändern einer Gerätevorlage nach deren Veröffentlichung finden Sie unter [Bearbeiten einer vorhandenen Gerätevorlage](howto-edit-device-template.md).

Wenn Sie eine Gerätevorlage veröffentlichen möchten, navigieren Sie zu der Gerätevorlage, und wählen Sie **Veröffentlichen** aus.

Nachdem Sie eine Gerätevorlage veröffentlicht haben, kann ein Bediener zur Seite **Geräte** wechseln und echte oder simulierte Geräte hinzufügen, die Ihre Gerätevorlage verwenden. Sie können Ihre Gerätevorlage weiter bearbeiten und speichern, während Sie Änderungen vornehmen. Immer wenn Sie diese Änderungen auf der Seite **Geräte** an die Bediener übermitteln möchten, müssen Sie **Veröffentlichen** auswählen.

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes sollten Sie sich zum Thema [Vornehmen von Änderungen an einer vorhandenen Gerätevorlage](howto-edit-device-template.md) informieren.
