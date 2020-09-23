---
title: Grundlegendes zur Versionsverwaltung von Gerätevorlagen für Ihre Azure IoT Central-Apps | Microsoft-Dokumentation
description: Durchlaufen Sie Ihre Gerätevorlagen, indem Sie neue Versionen erstellen, ohne Ihre gerade angeschlossenen Geräte zu beeinträchtigen.
author: sarahhubbard
ms.author: sahubbar
ms.date: 04/24/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: peterpr
ms.custom: device-developer
ms.openlocfilehash: 2025b127a428afa478cfe839c7619df2d7d688d3
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90015958"
---
# <a name="create-a-new-device-template-version"></a>Erstellen einer neuen Gerätevorlagenversion

*Dieser Artikel richtet sich an Lösungs- und Geräteentwickler.*

Eine Gerätevorlage enthält ein Schema, das beschreibt, wie ein Gerät mit IoT Central interagiert. Diese Interaktionen umfassen Telemetrie, Eigenschaften und Befehle. Sowohl das Gerät als auch die IoT Central-Anwendung stützen sich beim Informationsaustausch auf ein gemeinsames Verständnis dieses Schemas. Sie können nur begrenzte Änderungen am Schema vornehmen, ohne den Vertrag zu brechen, daher erfordern die meisten Schemaänderungen eine neue Version der Gerätevorlage. Durch die Versionsverwaltung der Gerätevorlage können ältere Geräte weiterhin die für sie verständliche Schemaversion verwenden, während neuere oder aktualisierte Geräte eine höhere Schemaversion verwenden.

Das Schema in einer Gerätevorlage ist im Gerätefunktionsmodell (DCM) und seinen Schnittstellen definiert. Gerätevorlagen enthalten weitere Informationen wie Cloudeigenschaften, Anzeigeanpassungen und Ansichten. Wenn Sie Änderungen an den Teilen der Gerätevorlage vornehmen, die nicht definieren, wie das Gerät Daten mit IoT Central austauscht, brauchen Sie die Vorlage nicht zu versionieren.

Sie müssen alle Änderungen an Gerätevorlagen veröffentlichen, unabhängig davon, ob sie eine Versionsaktualisierung erfordern oder nicht, bevor ein Operator sie verwenden kann. IoT Central hindert Sie daran, Änderungen an einer Gerätevorlage zu veröffentlichen, ohne die Vorlage zuvor zu versionieren.

> [!NOTE]
> Weitere Informationen zum Erstellen einer Gerätevorlage finden Sie unter [Einrichten und Verwalten einer Gerätevorlage](howto-set-up-template.md).

## <a name="versioning-rules"></a>Regeln zur Versionsverwaltung

In diesem Abschnitt werden die Regeln für die Versionsverwaltung zusammengefasst, die für Gerätevorlagen gelten. Sowohl DCMs als auch Schnittstellen verfügen über Versionsnummern. Der folgende Codeausschnitt zeigt das DCM für ein Umgebungssensorgerät. Das DCM verfügt über zwei Schnittstellen: **DeviceInformation** und **EnvironmentalSensor**. Sie können die Versionsnummern am Ende der `@id`-Felder sehen. Wählen Sie zum Anzeigen dieser Informationen auf der IoT Central-Benutzeroberfläche **Identität anzeigen** im Gerätevorlagen-Editor aus.

```json
{
  "@id": "urn:contoso:sample_device:1",
  "@type": "CapabilityModel",
  "implements": [
    {
      "@id": "urn:contoso:sample_device:deviceinfo:1",
      "@type": "InterfaceInstance",
      "name": "deviceinfo",
      "schema": {
        "@id": "urn:azureiot:DeviceManagement:DeviceInformation:1",
        "@type": "Interface",
        "displayName": {
          "en": "Device Information"
        },
        "contents": [...
        ]
      }
    },
    {
      "@id": "urn:contoso:sample_device:sensor:1",
      "@type": "InterfaceInstance",
      "name": "sensor",
      "schema": {
        "@id": "urn:contoso:EnvironmentalSensor:2",
        "@type": "Interface",
        "displayName": {
          "en": "Environmental Sensor"
        },
        "contents": [...
        ]
      }
    }
  ],
  "displayName": {
    "en": "Environment Sensor Capability Model"
  },
  "@context": [
    "http://azureiot.com/v1/contexts/IoTModel.json"
  ]
}
```

* Nachdem ein DCM veröffentlicht wurde, können Sie keine Schnittstellen entfernen, auch nicht in einer neuen Version der Gerätevorlage.
* Nachdem ein DCM veröffentlicht wurde, können Sie eine Schnittstelle hinzufügen, wenn Sie eine neue Version der Gerätevorlage erstellen.
* Nachdem ein DCM veröffentlicht wurde, können Sie eine Schnittstelle durch eine neuere Version ersetzen, wenn Sie eine neue Version der Gerätevorlage erstellen. Wenn die Gerätevorlage „Sensor v1“ z. B. die Schnittstelle „EnvironmentalSensor v1“ verwendet, können Sie eine Gerätevorlage „Sensor v2“ erstellen, die die Schnittstelle „EnvironmentalSensor v2“ verwendet.
* Nachdem eine Schnittstelle veröffentlicht wurde, können Sie keinen der Schnittstelleninhalte entfernen, auch nicht in einer neuen Version der Gerätevorlage.
* Nachdem eine Schnittstelle veröffentlicht wurde, können Sie Elemente zum Inhalt einer Schnittstelle hinzufügen, wenn Sie eine neue Version der Schnittstelle und der Gerätevorlage erstellen. Zu den Elementen, die Sie der Schnittstelle hinzufügen können, gehören Telemetrie, Eigenschaften und Befehle.
* Nachdem eine Schnittstelle veröffentlicht wurde, können Sie an vorhandenen Elementen in der Schnittstelle Änderungen vornehmen, die nicht dem Schema entsprechen, wenn Sie eine neue Version der Schnittstelle und der Gerätevorlage erstellen. Nicht zum Schema gehörende Teile eines Schnittstellenelements umfassen den Anzeigenamen und den semantischen Typ. Die Schemateile eines Schnittstellenelements, die Sie nicht ändern können, sind Name, Funktionstyp und Schema.

In den folgenden Abschnitten werden einige Beispiele für das Ändern von Gerätevorlagen in IoT Central erläutert.

## <a name="customize-the-device-template-without-versioning"></a>Anpassen der Gerätevorlage ohne Versionsverwaltung

Bestimmte Elemente Ihrer Gerätefunktionen können bearbeitet werden, ohne dass Sie eine neue Version Ihrer Gerätevorlage und Schnittstellen erstellen müssen. Hierzu gehören beispielsweise folgende Felder: Anzeigename, Semantiktyp, Mindest- und Höchstwerte, Dezimalstellen, Farbe, Einheit, Anzeigeeinheit, Kommentar und Beschreibung. So fügen Sie diese Anpassungen hinzu:

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie das Gerät aus, das Sie anpassen möchten.
1. Wählen Sie die Registerkarte **Anpassen** aus.
1. Alle in Ihrem Gerätefunktionsmodell definierten Funktionen sind hier aufgelistet. Sie können alle diese Felder bearbeiten, speichern und verwenden, ohne dass Sie Ihre Gerätevorlage versionieren müssen. Wenn Sie schreibgeschützte Felder bearbeiten möchten, müssen Sie eine neue Version der Gerätevorlage erstellen, um diese Felder ändern zu können. Wählen Sie ein Feld aus, das Sie bearbeiten möchten, und geben Sie neue Werte ein.
1. Klicken Sie auf **Speichern**. Jetzt überschreiben diese Werte alle Werte, die ursprünglich in Ihrer Gerätevorlage gespeichert waren, und werden in der gesamten Anwendung verwendet.

## <a name="version-a-device-template"></a>Versionieren einer Gerätevorlage

Wenn Sie eine neue Version Ihrer Gerätevorlage erstellen, wird eine Entwurfsversion der Vorlage erstellt, in der das Gerätefunktionsmodell bearbeitet werden kann. Alle veröffentlichten Schnittstellen bleiben veröffentlicht, bis für sie eine individuelle Version erstellt wurde. Um eine veröffentlichte Schnittstelle zu ändern, erstellen Sie zunächst eine neue Gerätevorlageversion.

Versionieren Sie die Gerätevorlage nur dann, wenn Sie versuchen, einen Teil des Gerätefunktionsmodells zu bearbeiten, den Sie im Abschnitt „Anpassungen“ nicht bearbeiten können.

So versionieren Sie eine Gerätevorlage

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie die Gerätevorlage aus, für die Sie eine neue Version erstellen möchten.
1. Klicken Sie oben auf der Seite auf die Schaltfläche **Version**, und geben Sie der Vorlage einen neuen Namen. IoT Central schlägt einen neuen Namen vor, den Sie bearbeiten können.
1. Klicken Sie auf **Erstellen**.
1. Jetzt befindet sich Ihre Gerätevorlage im Entwurfsmodus. Sie können sehen, dass Ihre Schnittstellen immer noch gesperrt sind. Erstellen Sie für alle Schnittstellen neue Versionen, die Sie ändern möchten.

## <a name="version-an-interface"></a>Versionieren einer Schnittstelle

Indem Sie eine neue Version für eine Schnittstelle erstellen, können Sie Funktionen in dieser bereits erstellten Schnittstelle hinzufügen, aktualisieren und entfernen.

So erstellen Sie eine neue Version einer Schnittstelle

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie Ihre Gerätevorlage aus, die sich im Entwurfsmodus befindet.
1. Wählen Sie die zu bearbeitende Schnittstelle aus, die sich im veröffentlichten Modus befindet.
1. Klicken Sie oben auf der Seite der Schnittstelle auf die Schaltfläche **Version**.
1. Klicken Sie auf **Erstellen**.
1. Jetzt befindet sich Ihre Schnittstelle im Entwurfsmodus. Sie können in dieser Schnittstelle Funktionen hinzufügen oder bearbeiten, ohne vorhandene Anpassungen und Ansichten zu beeinträchtigen.

## <a name="migrate-a-device-across-versions"></a>Migrieren eines Geräts zwischen verschiedenen Versionen

Es lassen sich mehrere Versionen der Gerätevorlage erstellen. Im Laufe der Zeit werden Sie über mehrere angeschlossene Geräte verfügen, die diese Gerätevorlagen verwenden. Sie können Geräte von einer Version Ihrer Gerätevorlage zu einer anderen migrieren. Die folgenden Schritte beschreiben, wie Sie ein Gerät migrieren:

1. Wechseln Sie zur **Device Explorer**-Seite.
1. Wählen Sie das Gerät aus, das Sie zu einer anderen Version migrieren möchten.
1. Wählen Sie **Migrieren** aus.
1. Wählen Sie die Gerätevorlage mit der Versionsnummer aus, zu der Sie das Gerät migrieren möchten, und klicken Sie auf **Migrieren**.

![Migrieren eines Geräts](media/howto-version-device-template/pick-version.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Operator oder Lösungsentwickler sind, wird als nächster Schritt vorgeschlagen, zu lernen, [wie Sie Ihre Geräte verwalten können](./howto-manage-devices.md).

Wenn Sie ein Geräteentwickler sind, sollten Sie sich als Nächstes über [Azure IoT Edge-Geräte und Azure IoT Central](./concepts-iot-edge.md) informieren.
