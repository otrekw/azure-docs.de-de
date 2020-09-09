---
author: v-tcassi
ms.service: iot-edge
ms.topic: include
ms.date: 08/26/2020
ms.author: v-tcassi
ms.openlocfilehash: 706b2306fbe9f2a744d2874a8b55f78fa2fc8e4d
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89300880"
---
## <a name="create-a-release-pipeline-for-continuous-deployment"></a>Erstellen einer Releasepipeline für kontinuierliche Bereitstellung (Continuous Deployment)

In diesem Abschnitt erstellen Sie eine Releasepipeline, die so konfiguriert ist, dass sie beim Ablegen von Artefakten durch Ihre Buildpipeline automatisch ausgeführt wird und Bereitstellungsprotokolle in Azure Pipelines anzeigt.

So erstellen Sie eine neue Pipeline und fügen eine neue Stufe hinzu:

1. Wählen Sie auf der Registerkarte **Releases** unter **Pipelines** die Schaltfläche **+ Neue Pipeline** aus. Wenn Sie bereits über Releasepipelines verfügen, wählen Sie die Schaltfläche **+ Neu** und dann **+ Neue Releasepipeline** aus.  

    ![Hinzufügen einer Releasepipeline über die Schaltfläche „+ Neue Pipeline“](./media/iot-edge-create-release-pipeline-for-continuous-deployment/add-release-pipeline.png)

2. Wenn Sie aufgefordert werden, eine Vorlage auszuwählen, wählen Sie für den Einstieg **Leerer Auftrag** aus.

    ![Starten mit einem leeren Auftrag für Ihre Releasepipeline](./media/iot-edge-create-release-pipeline-for-continuous-deployment/start-with-empty-release-job.png)

3. Ihre neue Releasepipeline wird mit einer Stufe namens **Stage 1** initialisiert. Benennen Sie „Stage 1“ in **dev** um, und behandeln Sie sie als Continuous Deployment-Pipeline für Ihre Entwicklungsumgebung. Continuous Deployment-Pipelines haben normalerweise mehrere Stages: **dev**, **staging** und **prod**. Sie können unterschiedliche Namen verwenden und je nach Ihrer DevOps-Methode weitere erstellen. Schließen Sie das Stufendetailsfenster nach der Umbenennung.

   Sie können Ihre Releasepipeline auch umbenennen, indem Sie oben den Text „Neue Releasepipeline“ auswählen.

4. Verknüpfen Sie das Release mit den Buildartefakten, die von der Buildpipeline veröffentlicht werden. Klicken Sie im Artefaktbereich auf **Hinzufügen**.

   ![Im Artefaktbereich der Schnittstelle auf „Hinzufügen“ klicken](./media/iot-edge-create-release-pipeline-for-continuous-deployment/add-artifacts.png)

5. Wählen Sie auf der Seite **Artefakt hinzufügen** die Option **Build** als **Quelltyp** aus. Wählen Sie das Projekt und die erstellte Buildpipeline aus. Wenn Sie möchten, können Sie den **Quellalias** in einen aussagekräftigeren Namen ändern. Klicken Sie anschließend auf **Hinzufügen**.

   ![Auf der Seite „Artefakt hinzufügen“ die Option „Hinzufügen“ auswählen, um das Artefakt zu erstellen](./media/iot-edge-create-release-pipeline-for-continuous-deployment/add-artifact.png)

6. Öffnen Sie die Artefakttrigger, und wählen Sie die Umschaltfläche aus, um die Continuous Deployment-Trigger zu aktivieren. Nun wird immer dann ein neues Release erstellt, wenn ein neuer Build verfügbar ist.

   ![Die Artefakttrigger öffnen, und die Umschaltfläche zum Aktivieren des Continuous Deployment-Triggers auswählen](./media/iot-edge-create-release-pipeline-for-continuous-deployment/add-trigger.png)

7. Die Stage **dev** ist mit einem Auftrag und null (0) Aufgaben vorkonfiguriert. Wählen Sie im Pipelinemenü **Aufgaben** und dann die Stage **dev** aus. Wählen Sie den **Agentauftrag** aus, und ändern Sie dessen **Anzeigenamen** in **QA**. Sie können Details zum Agentauftrag konfigurieren, aber die Bereitstellungsaufgabe ist plattformunabhängig, sodass Sie im ausgewählten **Agentpool** jede beliebige **Agentspezifikation** verwenden können.

   ![Anzeigen der Aufgaben für Ihre Stage „dev“ auf der Registerkarte „Aufgaben“](./media/iot-edge-create-release-pipeline-for-continuous-deployment/view-stage-tasks.png)

8. Wählen Sie im Auftrag „QA“ das Pluszeichen ( **+** ) aus, um zwei Aufgaben hinzuzufügen. Suchen Sie nach **Azure IoT Edge**, und fügen Sie die Aufgabe zweimal hinzu.

9. Wählen Sie die erste **Azure IoT Edge**-Aufgabe aus, und konfigurieren Sie sie mit den folgenden Werten:

    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | Anzeigename | Der Anzeigename wird bei einer Änderung des Felds „Aktion“ automatisch aktualisiert. |
    | Aktion | Wählen Sie `Generate deployment manifest`aus. |
    | „.template.json“-Datei | Geben Sie den Pfad „`$(System.DefaultWorkingDirectory)/Drop/drop/deployment.template.json`“ an. Der Pfad wird aus der Buildpipeline veröffentlicht. |
    | Standardplattform | Wählen Sie das entsprechende Betriebssystem für Ihre Module, basierend auf Ihrem IoT Edge-Zielgerät, aus. |
    | Ausgabepfad| Geben Sie den Pfad `$(System.DefaultWorkingDirectory)/Drop/drop/configs/deployment.json` an. Dieser Pfad ist die finale Datei mit dem IoT Edge-Bereitstellungsmanifest. |

    Diese Konfigurationen helfen dabei, die Modulimage-URLs in der Datei `deployment.template.json` zu ersetzen. Die Aufgabe **Bereitstellungsmanifest generieren** hilft auch dabei, die Variablen durch die exakten Werte zu ersetzen, die Sie in der Datei `deployment.template.json` definiert haben. In VS bzw. VS Code geben Sie den tatsächlichen Wert in einer `.env`-Datei an. In Azure Pipelines legen Sie den Wert auf der Registerkarte **Releasepipelinevariablen** fest. Wechseln Sie zur Registerkarte **Variablen**, und konfigurieren Sie den Namen und Wert wie folgt:

    * **ACR_ADDRESS**: Der Wert **Anmeldeserver** für Ihre Azure Container Registry. Den Anmeldeserver können Sie im Azure-Portal auf der Übersichtsseite Ihrer Containerregistrierung ermitteln.
    * **ACR_PASSWORD**: Das Kennwort für Ihre Azure Container Registry.
    * **ACR_USER**: Der Benutzername für Ihre Azure Container Registry.

    Wenn in Ihrem Projekt noch weitere Variablen vorhanden sind, können Sie die Namen und Werte auf diese Registerkarte angeben. **Bereitstellungsmanifest generieren** kann nur erkennen, dass die Variablen in der Variante `${VARIABLE}` vorliegen. Sorgen Sie dafür, dass Sie diese Variante in Ihren `*.template.json`-Dateien verwenden.

    ![Konfigurieren der Variablen für Ihre Releasepipeline auf der Registerkarte „Variablen“](./media/iot-edge-create-release-pipeline-for-continuous-deployment/configure-variables.png)

10. Wählen Sie die zweite **Azure IoT Edge**-Aufgabe aus, und konfigurieren Sie sie mit den folgenden Werten:

    | Parameter | BESCHREIBUNG |
    | --- | --- |
    | Anzeigename | Der Anzeigename wird bei einer Änderung des Felds „Aktion“ automatisch aktualisiert. |
    | Aktion | Wählen Sie `Deploy to IoT Edge devices`aus. |
    | Bereitstellungsdatei | Geben Sie den Pfad `$(System.DefaultWorkingDirectory)/Drop/drop/configs/deployment.json` an. Dieser Pfad ist die Datei mit dem IoT Edge-Bereitstellungsmanifest. |
    | Azure-Abonnement | Wählen Sie das Abonnement aus, das Ihre IoT Hub-Instanz enthält.|
    | IoT Hub-Name | Wählen Sie Ihren IoT Hub aus.|
    | Einzelgerät/mehrere Geräte auswählen | Wählen Sie aus, ob die Releasepipeline auf einem oder mehreren Geräten bereitgestellt werden soll. Geben Sie bei Bereitstellung auf einem einzelnen Gerät die **IoT Edge-Geräte-ID** ein. Geben Sie bei einer Bereitstellung auf mehreren Geräten die **Zielbedingung** des Geräts an. Die Zielbedingung ist ein Filter für den Abgleich mit einem Satz von IoT Edge-Geräten in IoT Hub. Wenn Sie Gerätetags als Bedingung verwenden möchten, müssen Sie Ihre entsprechenden Tags mit einem IoT Hub-Gerätezwilling aktualisieren. Aktualisieren Sie die **IoT Edge-Bereitstellungs-ID** und **IoT Edge-Bereitstellungspriorität** in den erweiterten Einstellungen. Weitere Informationen zum Erstellen einer Bereitstellung für mehrere Geräte finden Sie unter [Grundlegendes zu automatischen IoT Edge-Bereitstellungen für einzelne Geräte oder nach Bedarf](../articles/iot-edge/module-deployment-monitoring.md). |
    | Geräte-ID oder Zielbedingung | Geben Sie je nach der vorherigen Auswahl eine Geräte-ID oder [Zielbedingung](../articles/iot-edge/module-deployment-monitoring.md#target-condition) an, die auf mehreren Geräten bereitgestellt werden soll. |
    | Erweitert | Geben Sie für die IoT Edge-Bereitstellungs-ID `$(System.TeamProject)-$(Release.EnvironmentName)` an. Diese Variable ordnet den Projekt- und Releasenamen Ihrer IoT Edge-Bereitstellungs-ID zu. |

    ![Hinzufügen von Azure IoT Edge-Aufgaben für Ihre Stage „dev“](./media/iot-edge-create-release-pipeline-for-continuous-deployment/add-quality-assurance-task.png)

11. Wählen Sie **Speichern** zum Speichern der Änderungen auf der neuen Releasepipeline aus. Kehren Sie zur Pipelineansicht zurück, indem Sie im Menü die Registerkarte **Pipeline** auswählen.
