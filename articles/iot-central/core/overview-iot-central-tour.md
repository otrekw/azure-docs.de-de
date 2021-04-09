---
title: Kennenlernen der Benutzeroberfläche von Azure IoT Central | Microsoft-Dokumentation
description: Machen Sie sich mit den wichtigsten Bereichen der Benutzeroberfläche von Azure IoT Central vertraut, die Sie zum Erstellen, Verwalten und Nutzen Ihrer IoT-Lösung verwenden.
author: TheJasonAndrew
ms.author: v-anjaso
ms.date: 02/09/2021
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: 569a1365e73acbc2fdaf351f2e2cff21181241e1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100523464"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui"></a>Kennenlernen der Benutzeroberfläche von Azure IoT Central

In diesem Artikel wird die Benutzeroberfläche von Microsoft Azure IoT Central vorgestellt. Über die Benutzeroberfläche können Sie Azure IoT Central-Lösungen und die damit verbundenen Geräte erstellen, verwalten und verwenden.

Als _Lösungsentwickler_ verwenden Sie die Benutzeroberfläche von Azure IoT Central, um Ihre Azure IoT Central-Lösung zu definieren. Die Benutzeroberfläche bietet folgende Möglichkeiten:

* Definieren der Art von Gerät, das eine Verbindung mit Ihrer Lösung herstellt
* Konfigurieren der Regeln und Aktionen für Ihre Geräte 
* Anpassen der Benutzeroberfläche für einen _Bediener_, der Ihre Lösung verwendet

_Bediener_ verwenden die Benutzeroberfläche von Azure IoT Central, um ihre Azure IoT Central-Lösung zu verwalten. Die Benutzeroberfläche bietet folgende Möglichkeiten:

* Überwachen Ihrer Geräte
* Konfigurieren Ihrer Geräte
* Behandeln und Beheben von Problemen mit Ihren Geräten
* Bereitstellen von neuen Geräten

## <a name="iot-central-homepage"></a>IoT Central-Startseite

Auf der [IoT Central-Startseite](https://aka.ms/iotcentral-get-started) finden Sie weitere Informationen zu den aktuellen News und Features von IoT Central, zum Erstellen neuer Anwendungen und zum Anzeigen und Starten Ihrer vorhandenen Anwendung.

:::image type="content" source="media/overview-iot-central-tour/iot-central-homepage.png" alt-text="IoT Central-Startseite":::

### <a name="create-an-application"></a>Erstellen einer Anwendung

Im Abschnitt für die Erstellung können Sie die Liste mit den branchenrelevanten IoT Central-Vorlagen durchsuchen, um schnell einzusteigen. Alternativ können Sie auch von Grund auf neu beginnen, indem Sie eine Vorlage für eine benutzerdefinierte App verwenden.  

:::image type="content" source="media/overview-iot-central-tour/iot-central-build.png" alt-text="IoT Central-Seite für die Erstellung":::

Weitere Informationen finden Sie in der Schnellstartanleitung [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

### <a name="launch-your-application"></a>Starten Ihrer Anwendung

Sie können Ihre IoT Central-Anwendung starten, indem Sie auf die URL zugreifen, die von Ihnen bzw. Ihrem Lösungsentwickler bei der App-Erstellung ausgewählt wurde. Darüber hinaus können Sie im [App-Manager von IoT Central](https://aka.ms/iotcentral-apps) eine Liste mit allen Anwendungen anzeigen, auf die Sie Zugriff haben.

:::image type="content" source="media/overview-iot-central-tour/app-manager.png" alt-text="App-Manager von IoT Central":::

## <a name="navigate-your-application"></a>Navigieren in der Anwendung

In der IoT-Anwendung können Sie die Navigationselemente auf der linken Seite nutzen, um auf die verschiedenen Bereiche zuzugreifen. Sie können den linken Bereich erweitern oder reduzieren, indem Sie oben im Bereich das Symbol mit den drei Linien auswählen:

> [!NOTE]
> Welche Elemente im linken Bereich angezeigt werden, hängt von ihrer Benutzerrolle ab. Lesen Sie die weiteren Informationen zum [Verwalten von Benutzern und Rollen](howto-manage-users-roles.md). 

:::row:::
  :::column span="":::
      :::image type="content" source="media/overview-iot-central-tour/navigation-bar.png" alt-text="Navigationsbereich auf der linken Seite":::

  :::column-end:::
  :::column span="2":::
     Unter **Dashboard** wird Ihr Anwendungsdashboard angezeigt. Als *Lösungsentwickler* können Sie das globale Dashboard für Ihre Bediener anpassen. Je nach Benutzerrolle können Bediener auch eigene persönliche Dashboards erstellen.
     
     Unter **Geräte** können Sie Ihre verbundenen Geräte verwalten – real oder simuliert.

     Unter **Gerätegruppen** können Sie logische Sammlungen mit Geräten anzeigen und erstellen, die per Abfrage angegeben werden. Sie können diese Abfrage speichern und Gerätegruppen über die Anwendung nutzen, um Massenvorgänge durchzuführen.

     Unter **Regeln** können Sie Regeln zur Überwachung Ihrer Geräte erstellen und bearbeiten. Regeln werden anhand der Gerätetelemetrie ausgewertet und dienen zum Auslösen von anpassbaren Aktionen.

     Unter **Analyse** können Sie benutzerdefinierte Ansichten auf der Grundlage von Gerätedaten erstellen, um basierend auf Ihrer Anwendung Erkenntnisse zu gewinnen.

     Unter **Aufträge** können Sie Ihre Geräte bedarfsgesteuert verwalten, indem Sie Massenvorgänge durchführen.

     Die Option **Gerätevorlagen** ermöglicht die Erstellung und Verwaltung der Merkmale von Geräten, für die eine Verbindung mit Ihrer Anwendung hergestellt wird.

     Unter **Datenexport** können Sie den fortlaufenden Export in externe Dienste konfigurieren, z. B. Speicher und Warteschlangen.

     Die Option **Verwaltung** ermöglicht Ihnen das Verwalten der Bereiche Einstellungen, Anpassung, Abrechnung, Benutzer und Rollen Ihrer Anwendung.

     Mit **IoT Central** können *Administratoren* zurück zum App-Manager von IoT Central wechseln.
     
   :::column-end:::
:::row-end:::

### <a name="search-help-theme-and-support"></a>Suche, Hilfe, Design und Support

Das obere Menü wird auf jeder Seite angezeigt:

:::image type="content" source="media/overview-iot-central-tour/toolbar.png" alt-text="IoT Central-Symbolleiste":::

* Wenn Sie nach Gerätevorlagen und Geräten suchen möchten, geben Sie einen Wert für die **Suche** ein.
* Wählen Sie zum Ändern der Sprache der Benutzeroberfläche oder des Designs das Symbol **Einstellungen** aus. Informieren Sie sich über das [Verwalten Ihrer Anwendungseinstellungen](howto-manage-preferences.md).
* Wenn Sie Hilfe oder Unterstützung benötigen, klicken Sie auf das Dropdownmenü **Hilfe**, um eine Liste mit Ressourcen anzuzeigen. Sie können [Informationen zu Ihrer Anwendung](./howto-get-app-info.md) über den Link **Informationen zu Ihrer App** abrufen. Bei einer Anwendung im Tarif „Free“ beinhalten die Supportressourcen Zugriff auf den [Livechat](howto-show-hide-chat.md).
* Wählen Sie das Symbol **Konto** aus, um sich von der Anwendung abzumelden.

Sie können zwischen einem hellen und einem dunklen Benutzeroberflächendesign wählen:

> [!NOTE]
> Die Option zum Wählen zwischen hellen und dunklen Designs ist nicht verfügbar, wenn Ihr Administrator ein benutzerdefiniertes Design für die Anwendung konfiguriert hat.

:::image type="content" source="media/overview-iot-central-tour/themes.png" alt-text="Screenshot: „Design auswählen“ in IoT Central":::

### <a name="dashboard"></a>Dashboard

:::image type="content" source="Media/overview-iot-central-tour/dashboard.png" alt-text="Screenshot: IoT Central-Dashboard":::

* Nach der Anmeldung bei Ihrer Azure IoT Central-Anwendung wird als Erstes das Dashboard angezeigt. Als *Lösungsentwickler* können Sie mehrere globale Anwendungsdashboards für andere Benutzer erstellen und anpassen. Informieren Sie sich über das [Hinzufügen von Kacheln zu Ihrem Dashboard](howto-add-tiles-to-your-dashboard.md).

* Sofern Ihre Benutzerrolle als *Bediener* es zulässt, können Sie persönliche Dashboards erstellen, um die für Sie wichtigen Aspekte zu überwachen. Weitere Informationen finden Sie in der Anleitung zum [Erstellen und Verwalten mehrerer Dashboards](howto-create-personal-dashboards.md).

### <a name="devices"></a>Geräte

:::image type="content" source="Media/overview-iot-central-tour/devices.png" alt-text="Screenshot: Seite „Geräte“":::

Auf der Explorer-Seite werden die _Geräte_ in Ihrer Azure IoT Central-Anwendung angezeigt (gruppiert nach _Gerätevorlage_). 

* Eine Gerätevorlage definiert einen Gerätetyp, der eine Verbindung mit Ihrer Anwendung herstellen kann.
* Bei einem Gerät handelt es sich um ein echtes oder simuliertes Gerät in Ihrer Anwendung.

Weitere Informationen finden Sie in der Schnellstartanleitung [Überwachen Ihrer Geräte](./quick-monitor-devices.md). 

### <a name="device-groups"></a>Gerätegruppen

:::image type="content" source="Media/overview-iot-central-tour/device-groups.png" alt-text="Seite „Gerätegruppe“":::

Bei einer Gerätegruppe handelt es sich um eine Sammlung mit verwandten Geräten. Ein *Lösungsentwickler* definiert eine Abfrage, um die in einer Gerätegruppe enthaltenen Geräte zu identifizieren. Sie verwenden Gerätegruppen, um in Ihrer Anwendung Massenvorgänge durchzuführen. Weitere Informationen finden Sie im Artikel [Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung](tutorial-use-device-groups.md).

### <a name="rules"></a>Regeln
:::image type="content" source="Media/overview-iot-central-tour/rules.png" alt-text="Screenshot: Seite „Regeln“":::

Auf der Seite „Regeln“ können Sie Regeln basierend auf den Telemetriedaten, dem Status oder den Ereignissen von Geräten definieren. Bei Auslösung einer Regel können mehrere Aktionen ausgelöst werden, z. B. das Senden einer E-Mail, das Benachrichtigen eines externen Systems über Webhookwarnungen usw. Weitere Informationen hierzu finden Sie im Tutorial zum [Konfigurieren von Regeln](tutorial-create-telemetry-rules.md). 

### <a name="analytics"></a>Analytics

:::image type="content" source="Media/overview-iot-central-tour/analytics.png" alt-text="Screenshot: Seite „Analyse“":::

Mit der Analyse können Sie benutzerdefinierte Ansichten auf der Grundlage von Gerätedaten erstellen, um mit Ihrer Anwendung Erkenntnisse zu gewinnen. Weitere Informationen finden Sie im Artikel zum Thema [Erstellen einer Analyse für Ihre Azure IoT Central-Anwendung](howto-create-analytics.md).

### <a name="jobs"></a>Aufträge

:::image type="content" source="Media/overview-iot-central-tour/jobs.png" alt-text="Seite „Aufträge“":::

Auf der Seite „Aufträge“ können Sie Massenvorgänge für die Geräteverwaltung auf Ihren Geräten ausführen. Sie können Geräteeigenschaften und Einstellungen aktualisieren und Befehle für Gerätegruppen ausführen. Weitere Informationen finden Sie im Artikel [Ausführen eines Auftrags](howto-run-a-job.md).

### <a name="device-templates"></a>Gerätevorlagen

:::image type="content" source="Media/overview-iot-central-tour/templates.png" alt-text="Screenshot „Gerätevorlagen“":::

Auf der Seite „Gerätevorlagen“ können Ersteller die Gerätevorlagen in der Anwendung erstellen und verwalten. In einer Gerätevorlage werden Gerätemerkmale angegeben, z. B.:

* Telemetriedaten, Status und Ereignismessungen
* Eigenschaften
* Befehle
* Sichten

Der *Lösungsentwickler* kann auch Formulare und Dashboards für Bediener erstellen, die zum Verwalten von Geräten verwendet werden.

Weitere Informationen finden Sie im Tutorial [Define a new device type in your Azure IoT Central application](howto-set-up-template.md) (Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung). 

### <a name="data-export"></a>Datenexport

:::image type="content" source="Media/overview-iot-central-tour/export.png" alt-text="Datenexport":::

Per Datenexport können Sie Datenströme, z. B. für Telemetriedaten, von der Anwendung zu externen Systemen einrichten. Weitere Informationen finden Sie im Artikel [Exportieren von Daten in Azure IoT Central](./howto-export-data.md).

### <a name="administration"></a>Verwaltung

:::image type="content" source="media/overview-iot-central-tour/administration.png" alt-text="Screenshot: IoT-Verwaltung":::

Auf der Seite „Verwaltung“ können Sie Ihre IoT Central-Anwendung konfigurieren und anpassen. Sie können den Anwendungsnamen, die URL und das Design ändern, Benutzer und Rollen verwalten, API-Token erstellen und Ihre Anwendung exportieren. Weitere Informationen finden Sie im Artikel [How to administer your application](howto-administer.md) (Verwalten Ihrer Anwendung).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über Azure IoT Central verschafft und sich mit dem Layout der Benutzeroberfläche vertraut gemacht haben, empfehlen wir, mit der Schnellstartanleitung zum [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md) fortzufahren.
