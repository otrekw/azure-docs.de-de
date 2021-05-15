---
title: Problembehandlung von Azure Static Web Apps
description: Erste Schritte zum Aufspüren von Problemen beim Verwenden von Azure Static Web Apps
services: static-web-apps
author: geektrainer
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 04/07/2021
ms.author: chrhar
ms.openlocfilehash: 082c3af694932f4e864b99aa8c2cdc7d1f5fca30
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108210159"
---
# <a name="troubleshooting-deployment-and-runtime-errors"></a>Problembehandlung bei Bereitstellungs- und Laufzeitfehlern

Dieser Artikel enthält ausführliche Anleitungen zur Problembehandlung bei Bereitstellungs- und anderen Problemen für Ihre statische Web-App.

## <a name="retrieve-deployment-error-messages"></a>Abrufen von Bereitstellungsfehlermeldungen

Der Bereitstellungsworkflow für Azure Static Web Apps verwendet den Node.js [Oryx-Buildprozess](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/nodejs.md#build), der automatisch die folgenden Befehle ausführt:

```bash
npm install
npm run build # if specified in package.json
npm run build:azure # if specified in package.json
```

Alle von diesem Prozess ausgelösten Fehler werden in der GitHub-Workflowausführung protokolliert.

1. Navigieren Sie zum GitHub-Repository für Ihre statische Web-App.
1. Wählen Sie **Actions** aus.

    > [!NOTE]
    > Alle fehlerhaften Workflowausführungen werden mit einem roten *X* anstelle eines grünen Häkchens angezeigt.

1. Wählen Sie den Link für die Workflowausführung aus, die Sie überprüfen möchten.
1. Wählen Sie **Erstellungs- und Bereitstellungsauftrag** aus, um die Details der Bereitstellung zu öffnen.
1. Wählen Sie **Erstellen und bereitstellen** aus, um das Protokoll anzuzeigen.

    ![Screenshot des Bereitstellungsprotokolls für eine statische Web-App](./media/troubleshooting/build-deploy-log.png)

1. Überprüfen Sie die Protokolle und Fehlermeldungen.

    > [!NOTE]
    > Einige Warnmeldungen werden möglicherweise rot angezeigt, z. B. Hinweise zu *.oryx_prod_node_modules* und *Arbeitsbereich*. Diese sind Teil des üblichen Bereitstellungsprozesses.

Wenn Pakete nicht installiert werden können oder andere Probleme ausgelöst werden, werden hier die ursprünglichen Fehlermeldungen angezeigt, als ob Sie die `npm`-Befehle lokal ausgeführt hätten.

### <a name="confirm-folder-configuration"></a>Bestätigen der Ordnerkonfiguration

Azure Static Web Apps muss die Ordner kennen, die zum Hosten Ihrer Anwendung verwendet werden sollen. Diese Konfiguration wird durch den Buildprozess am Ende des Workflows bestätigt. Alle Fehler werden während der Überprüfungsschritte protokolliert.

Wenn im Fehlerprotokoll eine der folgenden Fehlermeldungen angezeigt wird, ist dies ein Hinweis darauf, dass die Ordnerkonfiguration für Ihren Workflow falsch ist.

| Fehlermeldung | BESCHREIBUNG |
| --- | --- |
|Speicherort des App-Verzeichnisses:'/*ordner*' ist ungültig. Dieses Verzeichnis konnte nicht erkannt werden. | Vergewissern Sie sich, dass Ihr Workflow Ihre Repositorystruktur widerspiegelt. |
| Der App-Build konnte keinen Artefaktordner erstellen: '*ordner*'. | Stellen Sie sicher, dass die Eigenschaft `folder` in Ihrer Workflowdatei ordnungsgemäß konfiguriert ist. |
| Entweder wurde kein API-Verzeichnis angegeben, oder das angegebene Verzeichnis wurde nicht gefunden. | Azure Functions wird nicht erstellt, da der Workflow keinen Wert für den Ordner `api` definiert. |

Im Workflow sind drei Ordnerspeicherorte angegeben. Stellen Sie sicher, dass diese Einstellungen sowohl Ihrem Projekt als auch allen Tools entsprechen, die Ihren Quellcode vor der Bereitstellung transformieren.

| Konfigurationseinstellung | BESCHREIBUNG |
| --- | --- |
| `app_location` | Der Stammspeicherort des bereitzustellenden Quellcodes Diese Einstellung ist in der Regel */* oder der Speicherort der JavaScript- und HTML-Dateien für Ihr Projekt. |
| `output_location` | Der Name des Ordners, der von einem Buildprozess aus einem Bundler wie webpack erstellt wurde Dieser Ordner muss sowohl vom Buildprozess als auch von einem Unterverzeichnis unter `app_location` erstellt werden. |
| `api_location` |Der Stammspeicherort Ihrer Azure Functions-Anwendung, die von Azure Static Web Apps gehostet wird Dieser verweist auf den Stammordner aller Azure Functions für Ihr Projekt, in der Regel *api*. |

> [!NOTE]
> Bei Fehlermeldungen, die durch eine falsche `api_location`-Konfiguration generiert werden, kann der Buildprozess immer noch erfolgreich sein, da Azure Static Web Apps keinen serverlosen Code erfordert.

## <a name="review-server-errors"></a>Überprüfen von Serverfehlern

Verwenden Sie [Application Insights](../azure-monitor/app/app-insights-overview.md), um Laufzeitfehlermeldungen zu suchen. Wenn Sie noch keine Instanz erstellt haben, finden Sie weitere Informationen unter [Überwachen von Azure Static Web Apps](monitor.md). Application Insights protokolliert die vollständige Fehlermeldung und die Stapelüberwachung, die von jedem Fehler generiert wird.

> [!NOTE]
> Sie können nur Fehlermeldungen anzeigen, die nach der Installation von Application Insights generiert wurden.

1. Öffnen Sie im Azure-Portal die **Ressourcengruppe**, die Ihrer statischen Web-App zugeordnet ist.
1. Wählen Sie die Application Insights-Instanz aus, die den gleichen Namen wie Ihre statische Web-App hat (sofern sie mit den obigen Schritten erstellt wurde).
1. Wählen Sie unter *Untersuchen* die Option **Fehler** aus.
1. Scrollen Sie nach unten zu *Drilldown* auf der rechten Seite.
1. In der unteren rechten Ecke unter *Drilldown* zeigt eine Schaltfläche die Anzahl kürzlich fehlgeschlagener Vorgänge an.

    ![Screenshot: Fehleransicht](./media/troubleshooting/app-insights-errors.png)

1. Wählen Sie die Schaltfläche **x Vorgänge** aus, um ein Panel zu öffnen, in dem die zuletzt fehlgeschlagenen Vorgänge angezeigt werden.

    ![Screenshot: Vorgangsansicht](./media/troubleshooting/app-insights-operations.png)

1. Sie können einzelne Fehler untersuchen, indem Sie einen Fehler aus der Liste auswählen.

    ![Screenshot: Bildschirm „Fehlerdetails“](./media/troubleshooting/app-insights-details.png)

## <a name="environment-variables"></a>Umgebungsvariablen

Viele Webanwendungen verwenden Umgebungsvariablen zum Speichern sensibler oder umgebungsspezifischer Einstellungen. Azure Static Web Apps unterstützt Umgebungsvariablen durch Anwendungseinstellungen.

Anwendungseinstellungen sind Schlüssel-Wert-Paare, die Umgebungsvariablen für Ihre Anwendung festlegen. Variablen sind für Ihre Anwendung mit der gleichen Syntax verfügbar, die auch typisch für den Zugriff auf Umgebungsvariablen ist.

Überprüfen Sie bei der Bereitstellung, ob alle Umgebungsvariablen als Anwendungseinstellungen festgelegt sind.

1. Öffnen Sie Ihre statische Web-App im Azure-Portal.
1. Wählen Sie **Konfiguration** aus. Auf dem Bildschirm *Konfiguration* wird die Liste aller Anwendungseinstellungen angezeigt.

    ![Screenshot: Bildschirm „Konfiguration“ in einer statischen Web-App](media/troubleshooting/app-settings.png)

Führen Sie die folgenden Schritte aus, um eine neue Variable hinzufügen.

1. Wählen Sie **Hinzufügen**.
1. Legen Sie den **Namen** fest.
1. Legen Sie den **Wert** fest.
1. Klicken Sie auf **OK**.
1. Wählen Sie **Speichern** aus.
