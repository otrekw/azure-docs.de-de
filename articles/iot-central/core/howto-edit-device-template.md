---
title: Bearbeiten einer Gerätevorlage in Ihrer Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Durchlaufen Ihrer Gerätevorlagen ohne Auswirkungen auf Ihre live verbundenen Geräte
author: dominicbetts
ms.author: dobett
ms.date: 04/26/2021
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.custom: device-developer
ms.openlocfilehash: fd6fb0cebe33d8511185c396c95faa8927941e6e
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110103972"
---
# <a name="edit-an-existing-device-template"></a>Bearbeiten einer vorhandenen Gerätevorlage

*Dieser Artikel richtet sich an Lösungs- und Geräteentwickler.*

Eine Gerätevorlage enthält ein Modell, das beschreibt, wie ein Gerät mit IoT Central interagiert. Dieses Modell definiert die Funktionen des Geräts und wie IoT Central damit interagiert. Geräte können Telemetrie- und Eigenschaftswerte an IoT Central senden, und IoT Central kann Eigenschaftenupdates und Befehle an ein Gerät senden. IoT Central verwendet das Modell auch zum Definieren von Interaktionen mit IoT Central-Features wie Aufträgen, Regeln und Exporten.

Änderungen am Modell in einer Gerätevorlage können sich auf ihre gesamte Anwendung auswirken, einschließlich aller verbundenen Geräte. Änderungen an einer Funktion, die von Regeln, Exporten, Gerätegruppen oder Aufträgen verwendet wird, können dazu führen, dass sie sich unerwartet verhalten oder überhaupt nicht funktionieren. Wenn Sie beispielsweise eine Telemetriedefinition aus einer Vorlage entfernen, geschieht Folgendes:

- IoT Central kann diesen Wert nicht mehr interpretieren. IoT Central zeigt Gerätedaten, die es nicht als **nicht modellierte Daten** interpretieren kann, auf der Seite **Rohdaten** des Geräts an.
- IoT Central bezieht den Wert nicht mehr in Datenexporte ein.

Damit Sie unbeabsichtigte Folgen der Bearbeitung einer Gerätevorlage besser vermeiden können, enthält dieser Artikel Empfehlungen, die auf Ihrer aktuellen Phase des Entwicklungslebenszyklus basieren. Im Allgemeinen gilt: Je früher Sie sich im Entwicklungslebenszyklus befinden, desto toleranter können Sie bei Änderungen an der Gerätevorlage sein.

Weitere Informationen zu Gerätevorlagen und deren Erstellung finden Sie unter [Was sind Gerätevorlagen?](concepts-device-templates.md) und [Einrichten einer Gerätevorlage](howto-set-up-template.md).

## <a name="modify-a-device-template"></a>Ändern einer Gerätevorlage

Additive Änderungen, z. B. das Hinzufügen einer Funktion oder Schnittstelle zu einem Modell, sind Nonbreaking Changes. Sie können additive Änderungen an einem Modell in jeder beliebigen Phase des Entwicklungslebenszyklus vornehmen.

Breaking Changes umfassen das Entfernen von Teilen eines Modells oder aber das Ändern eines Funktionsnamens oder Schematyps. Diese Änderungen können dazu führen, dass Anwendungsfeatures wie Regeln, Exporte oder Dashboards Fehlermeldungen anzeigen und nicht mehr funktionieren.

In frühen Phasen der Geräteentwicklung, während Sie Ihr Gerätemodell noch entwerfen und testen, ist die Toleranz bei direkt am Modell vorgenommenen Änderungen größer. Bevor Sie Produktionsgeräte mit einer Gerätevorlage verbinden, können Sie die Vorlage direkt bearbeiten. IoT Central wendet diese Änderungen automatisch auf Geräte an, wenn Sie die Gerätevorlage veröffentlichen.

Nachdem Sie Produktionsgeräte an eine Gerätevorlage angefügt haben, werten Sie die Auswirkung von Änderungen aus, bevor Sie eine Gerätevorlage bearbeiten. Sie sollten keine Breaking Changes an einer Gerätevorlage in der Produktion vornehmen. Wenn Sie solche Änderungen vornehmen möchten, erstellen Sie eine neue Version der Gerätevorlage. Testen Sie die neue Gerätevorlage, und migrieren Sie Ihre Produktionsgeräte dann bei einer geplanten Ausfallzeit zur neuen Vorlage.

## <a name="update-an-iot-edge-device-template"></a>Aktualisieren einer IoT Edge-Gerätevorlage

IoT Edge-Gerätevorlagen enthalten zusätzlich zum Gerätemodell ein _Bereitstellungsmanifest_. Bei einem IoT Edge-Gerät gruppiert das Modell Funktionen nach Modulen, die den auf dem Gerät ausgeführten IoT Edge-Modulen entsprechen. Das Bereitstellungsmanifest ist ein separates JSON-Dokument, das ein IoT Edge Gerät informiert, welche Module installiert und wie sie konfiguriert werden sollen. Dieselben Anweisungen wie im vorhergehenden Abschnitt gelten für die Module im Gerätemodell. Außerdem muss jedes im Gerätemodell definierte Modul in das Bereitstellungsmanifest einbezogen werden. Nachdem eine IoT Edge-Gerätevorlage veröffentlicht wurde, müssen Sie eine neue Version erstellen, wenn Sie das Bereitstellungsmanifest ersetzen müssen. Damit IoT Edge-Geräte das neue Bereitstellungsmanifest erhalten, migrieren Sie sie zur neuen Vorlagenversion.

Weitere Informationen finden Sie unter [IoT Edge-Bereitstellungsmanifeste und IoT Central-Gerätevorlagen](concepts-iot-edge.md#iot-edge-deployment-manifests-and-iot-central-device-templates).

### <a name="edit-and-publish-actions"></a>Bearbeiten und Veröffentlichen von Aktionen

Die folgenden Aktionen sind hilfreich, wenn Sie eine Gerätevorlage bearbeiten:

- _Speichern_ Sie sie. Wenn Sie einen Teil Ihrer Gerätevorlage ändern, wird durch das Speichern der Änderungen ein Entwurf erstellt, zu dem Sie zurückkehren können. Diese Änderungen wirken sich noch nicht auf verbundene Geräte aus. Alle aus dieser Vorlage erstellten Geräte erhalten die gespeicherten Änderungen erst, wenn Sie die Vorlage veröffentlichen.
- _Veröffentlichung_: Wenn Sie die Gerätevorlage veröffentlichen, werden alle gespeicherten Änderungen auf vorhandene Geräteinstanzen angewendet. Neu erstellte Geräteinstanzen verwenden immer die neueste veröffentlichte Vorlage.
- _Versionieren einer Vorlage_. Wenn Sie eine Gerätevorlage versionieren, erstellt sie eine neue Vorlage mit allen zuletzt gespeicherten Änderungen. Vorhandene Geräteinstanzen werden durch Änderungen an einer neuen Version nicht verändert. Weitere Informationen finden Sie unter [Versionieren einer Gerätevorlage](#version-a-device-template).
- _Versionieren einer Schnittstelle_. Wenn Sie eine Schnittstelle versionieren, erstellt sie eine neue Schnittstelle mit allen zuletzt gespeicherten Funktionen. Sie können eine Schnittstelle an mehreren Stellen in einer Vorlage wiederverwenden. Deshalb ändert eine Änderung an einem Verweis auf eine Schnittstelle alle Stellen in der Vorlage, die diese Schnittstelle verwenden. Wenn Sie eine Schnittstelle versionieren, ändert sich dieses Verhalten, weil die neue Version jetzt eine separate Schnittstelle ist. Weitere Informationen finden Sie unter [Versionieren einer Schnittstelle](#version-an-interface).
- _Migrieren eines Geräts_. Wenn Sie ein Gerät migrieren, wird die Geräteinstanz von einer Gerätevorlage zu einer anderen ausgetauscht. Die Gerätemigration kann eine kurze Zeit dauern, während IoT Central die Änderungen verarbeitet. Weitere Informationen finden Sie unter [Migrieren eines Geräts zwischen Versionen](#migrate-a-device-across-versions).

### <a name="version-numbers"></a>Versionsnummern

Sowohl Gerätemodelle als auch Schnittstellen haben Versionsnummern. Bei unterschiedlichen Versionsnummern können Modelle oder Schnittstellen einen `@id`-Wert gemeinsam nutzen und gleichzeitig einen Verlauf von Updates bereitstellen. Versionsnummern werden nur erhöht, wenn Sie eine Versionierung der Vorlage oder Schnittstelle wählen oder aber die Versionsnummer absichtlich ändern. Sie sollten eine Versionsnummer ändern, wenn Sie eine größere Änderung an einer Vorlage oder Schnittstelle vornehmen.

Der folgende Codeausschnitt zeigt das Gerätemodell für ein Thermostatgerät. Das Gerätemodell hat eine einzige Schnittstelle. Die Versionsnummer, `1`, wird am Ende des Felds `@id` angezeigt.

```json
{
  "@context": "dtmi:dtdl:context;2",
  "@id": "dtmi:com:example:Thermostat;1",
  "@type": "Interface",
  "displayName": "Thermostat",
  "description": "Reports current temperature and provides desired temperature control.",
  "contents": [
    // ...
  ]
}
```

Wählen Sie zum Anzeigen dieser Informationen auf der IoT Central-Benutzeroberfläche im Gerätevorlagen-Editor die Option **Identität anzeigen** aus:

:::image type="content" source="media/howto-edit-device-template/view-identity.png" alt-text="Anzeigen der Identität einer Schnittstelle, um die Versionsnummer anzuzeigen":::

## <a name="version-a-device-template"></a>Versionieren einer Gerätevorlage

So versionieren Sie eine Gerätevorlage

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie die Gerätevorlage aus, die Sie versionieren möchten.
1. Wählen Sie oben auf der Seite **Version** aus, und geben Sie der Vorlage einen neuen Namen. IoT Central schlägt einen neuen Namen vor, den Sie bearbeiten können.
1. Klicken Sie auf **Erstellen**.

Sie haben jetzt eine neue Vorlage mit einer eindeutigen Identität erstellt, die an vorhandene Geräte nicht angefügt wird.

## <a name="version-an-interface"></a>Versionieren einer Schnittstelle

So erstellen Sie eine neue Version einer Schnittstelle

1. Wechseln Sie zur Seite **Gerätevorlagen**.
1. Wählen Sie Ihre Gerätevorlage aus, die sich im Entwurfsmodus befindet.
1. Wählen Sie die veröffentlichte Schnittstelle aus, die Sie versionieren und bearbeiten möchten.
1. Wählen Sie oben auf der Schnittstellenseite **Version** aus.
1. Klicken Sie auf **Erstellen**.

Sie haben jetzt eine neue Schnittstelle mit einer eindeutigen Identität erstellt, die mit der vorherigen Schnittstellenversion nicht synchronisiert wird.

## <a name="migrate-a-device-across-versions"></a>Migrieren eines Geräts zwischen verschiedenen Versionen

Es lassen sich mehrere Versionen der Gerätevorlage erstellen. Im Laufe der Zeit werden Sie über mehrere angeschlossene Geräte verfügen, die diese Gerätevorlagen verwenden. Sie können Geräte von einer Version Ihrer Gerätevorlage zu einer anderen migrieren. Die folgenden Schritte beschreiben, wie Sie ein Gerät migrieren:

1. Wechseln Sie zur Seite **Geräte**.
1. Wählen Sie das Gerät aus, das Sie zu einer anderen Version migrieren möchten.
1. Wählen Sie **Migrieren** aus:

    :::image type="content" source="media/howto-edit-device-template/migrate-device.png" alt-text="Auswählen der Option zum Starten der Migration eines Geräts":::

1. Wählen Sie die Gerätevorlage mit der Versionsnummer, zu der Sie das Gerät migrieren möchten, und dann **Migrieren** aus.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Operator oder Lösungsentwickler sind, wird als nächster Schritt vorgeschlagen, zu lernen, [wie Sie Ihre Geräte verwalten können](./howto-manage-devices.md).

Wenn Sie ein Geräteentwickler sind, sollten Sie sich als Nächstes über [Azure IoT Edge-Geräte und Azure IoT Central](./concepts-iot-edge.md) informieren.
