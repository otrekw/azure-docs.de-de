---
title: Verwenden von Cloud Services (erweiterter Support, Vorschau)
description: Hier erfahren Sie, wie Sie einen Azure-Clouddienst unter Verwendung von Azure Resource Manager mit Visual Studio erstellen und bereitstellen.
author: ghogen
ms.service: cloud-services-extended-support
manager: jillfra
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: tutorial
ms.date: 10/5/2020
ms.author: ghogen
ms.openlocfilehash: 80aa160c53b278137467dba2afa41384c7c4f378
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101722669"
---
# <a name="create-and-deploy-a-azure-cloud-service-extended-support-using-visual-studio"></a>Erstellen und Bereitstellen eines Azure-Clouddiensts (erweiterter Support) mithilfe von Visual Studio

Ab [Visual Studio 2019 (Version 16.9)](https://visualstudio.microsoft.com/vs/preview/) (derzeit in der Vorschauphase) können Sie Azure Resource Manager (ARM) für die Arbeit mit Clouddiensten verwenden, was die Wartung und Verwaltung von Azure-Ressourcen erheblich vereinfacht und modernisiert. Dies wird durch einen neuen Azure-Dienst ermöglicht: Cloud Services (erweiterter Support). Sie können einen vorhandenen Clouddienst in Cloud Services (erweiterter Support) veröffentlichen. Informationen zu diesem Azure-Dienst finden Sie unter [Grundlegendes zu Azure Cloud Services (erweiterter Support)](overview.md).

> [!IMPORTANT]
> Cloud Services (erweiterter Support) befindet sich derzeit in der Public Preview-Phase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register-the-feature-for-your-subscription"></a>Registrieren des Features für Ihr Abonnement
Cloud Services (erweiterter Support) befindet sich derzeit in der Vorschauphase. Registrieren Sie das Feature wie folgt für Ihr Abonnement:

```powershell
Register-AzProviderFeature -FeatureName CloudServices -ProviderNamespace Microsoft.Compute
```
Weitere Informationen finden Sie unter [Voraussetzungen für die Bereitstellung von Azure Cloud Services (erweiterter Support)](deploy-prerequisite.md).

## <a name="create-a-project"></a>Erstellen eines Projekts

Visual Studio bietet eine Projektvorlage, mit der Sie einen Azure-Clouddienst mit erweitertem Support erstellen können. Sie heißt **Azure-Clouddienst (erweiterter Support)** . Ein Clouddienst ist ein einfacher universeller Azure-Dienst. Nach der Erstellung des Projekts können Sie den Clouddienst mit Visual Studio in Azure konfigurieren, debuggen und bereitstellen.

### <a name="to-create-an-azure-cloud-service-extended-support-project-in-visual-studio"></a>Erstellen eines Projekts für einen Azure-Clouddienst (erweiterter Support) in Visual Studio

In diesem Abschnitt erfahren Sie Schritt für Schritt, wie Sie ein Azure-Clouddienstprojekt in Visual Studio mit einer oder mehreren Webrollen erstellen.

1. Wählen Sie im Startfenster **Neues Projekt erstellen** aus.

1. Geben Sie *Cloud* in das Suchfeld ein, und wählen Sie **Azure-Clouddienst (erweiterter Support)** aus.

   ![Neuer Azure-Clouddienst mit erweitertem Support](./media/choose-project-template.png)

1. Benennen Sie das Projekt, und klicken Sie auf **Erstellen**.

   ![Benennen Sie das Projekt.](./media/configure-new-project.png)

1. Wählen Sie im Dialogfeld **Neuer Clouddienst** die Rollen aus, die Sie hinzufügen möchten, und klicken Sie auf die Schaltfläche mit dem Pfeil nach rechts, um sie der Projektmappe hinzuzufügen.

    ![Auswählen neuer Azure-Clouddienstrollen](./media/choose-roles.png)

1. Um eine Rolle umzubenennen, die Sie hinzugefügt haben, zeigen Sie im Dialogfeld **Neuer Clouddienst** mit dem Mauszeiger auf die Rolle, und wählen Sie im Kontextmenü die Option **Umbenennen** aus. Sie können eine Rolle auch in der Projektmappe umbenennen (im **Projektmappen-Explorer**), nachdem sie hinzugefügt wurde.

    ![Umbenennen einer Azure-Clouddienstrolle](./media/new-cloud-service-rename.png)

Das Visual Studio-Azure-Projekt weist Zuordnungen zu den Rollenprojekten in der Projektmappe auf. Das Projekt enthält zudem die *Dienstdefinitionsdatei* und die *Dienstkonfigurationsdatei*:

- **Dienstdefinitionsdatei:** Definiert die Laufzeiteinstellungen für Ihre Anwendung wie etwa erforderliche Rollen, Endpunkte und Größe des virtuellen Computers.
- **Dienstkonfigurationsdatei:** Konfiguriert, wie viele Instanzen einer Rolle ausgeführt werden, und die Werte der für eine Rolle definierten Einstellungen.

Weitere Informationen zu diesen Dateien finden Sie unter [Konfigurieren der Rollen für einen Azure-Clouddienst mit Visual Studio](/visualstudio/azure/vs-azure-tools-configure-roles-for-cloud-service).

## <a name="publish-a-cloud-service"></a>Veröffentlichen eines Clouddiensts

1. Erstellen oder öffnen Sie ein Azure-Clouddienstprojekt in Visual Studio.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü die Option **Veröffentlichen** aus.

   ![Anmeldeseite](./media/publish-step-1.png)

1. **Konto**: Wählen Sie ein Konto aus, oder wählen Sie in der Konto-Dropdownliste **Konto hinzufügen** aus.

1. **Abonnement wählen**: Wählen Sie das Abonnement aus, das Sie für Ihre Bereitstellung verwenden möchten. Dem Abonnement, das für die Bereitstellung von Cloud Services (erweiterter Support) verwendet wird, müssen über die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) Rollen vom Typ „Besitzer“ oder „Mitwirkender“ zugewiesen werden. Sollte Ihr Abonnement über keine dieser Rollen verfügen, erfahren Sie unter [Schritte zum Hinzufügen einer Rollenzuweisung](../role-based-access-control/role-assignments-steps.md), wie Sie sie hinzufügen. Anschließend können Sie mit diesem Artikel fortfahren.

1. Wählen Sie **Weiter** aus, um zur Seite **Einstellungen** zu gelangen.

   ![Allgemeine Einstellungen](./media/publish-settings.png)

1. **Clouddienst**: Wählen Sie in der Dropdownliste entweder einen bereits vorhandenen Clouddienst aus, oder wählen Sie **Neu erstellen** aus, und erstellen Sie einen Clouddienst. Das Rechenzentrum für den Clouddienst wird jeweils in Klammern angezeigt. Es empfiehlt sich, für den Clouddienst den gleichen Rechenzentrumsstandort zu verwenden wie für das Speicherkonto.

   Wenn Sie einen neuen Clouddienst erstellen, wird das Dialogfeld **Clouddienst erstellen (erweiterter Support)** angezeigt. Geben Sie den gewünschten Standort und die gewünschte Ressourcengruppe für den Clouddienst an.

   ![Erstellen eines Clouddiensts mit erweitertem Support](./media/extended-support-dialog.png)

1. **Buildkonfiguration**: Wählen Sie entweder **Debuggen** oder **Release** aus.

1. **Dienstkonfiguration**: Wählen Sie entweder **Cloud** oder **Lokal** aus.

1. **Speicherkonto**: Wählen Sie das für diese Bereitstellung zu verwendende Speicherkonto aus, oder verwenden Sie die Option **Neu erstellen**, um ein Speicherkonto zu erstellen. Die Region für das Speicherkonto wird jeweils in Klammern angezeigt. Es empfiehlt sich, für das Speicherkonto den gleichen Rechenzentrumsstandort zu verwenden wie für den Clouddienst (allgemeine Einstellungen).

   Das Azure-Speicherkonto speichert das Paket für die Bereitstellung der Anwendung. Nach der Bereitstellung der Anwendung wird das Paket aus dem Speicherkonto entfernt.

1. **Key Vault**: Geben Sie die Key Vault-Instanz an, die die Geheimnisse für diesen Clouddienst enthält. Diese Option wird aktiviert, wenn Remotedesktop aktiviert ist oder der Konfiguration Zertifikate hinzugefügt werden.

1. **Remotedesktop für alle Rollen aktivieren**: Aktivieren Sie diese Option, wenn Sie eine Remoteverbindung mit dem Dienst herstellen möchten. Sie werden zur Angabe von Anmeldeinformationen aufgefordert.

   ![Remotedesktopeinstellungen](./media/remote-desktop-configuration.png)

1. Wählen Sie **Weiter** aus, um zur Seite **Diagnoseeinstellungen** zu gelangen.

   ![Diagnoseeinstellungen](./media/diagnostics-settings.png)

   Die Diagnose ermöglicht die Behandlung von Problemen im Zusammenhang mit einem Azure-Clouddienst (oder virtuellen Azure-Computer). Informationen zur Diagnose finden Sie unter [Konfigurieren der Diagnose für Azure Cloud Services und Virtual Machines](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) . Weitere Informationen zu Application Insights finden Sie unter [Was ist Application Insights?](../azure-monitor/app/app-insights-overview.md).

1. Wählen Sie **Weiter** aus, um zur Seite **Zusammenfassung** zu gelangen.

   ![Zusammenfassung](./media/publish-summary.png)

1. **Zielprofil**: Sie können sich entscheiden, ein Veröffentlichungsprofil auf der Grundlage der von Ihnen ausgewählten Einstellungen zu erstellen. Sie können beispielsweise ein Profil für eine Testumgebung und ein weiteres für die Produktion verwenden. Um dieses Profil zu speichern, wählen Sie das Symbol **Speichern**. Der Assistent erstellt das Profil und speichert es im Visual Studio-Projekt. Öffnen Sie zum Ändern des Profilnamens die Liste **Zielprofil**, und wählen Sie anschließend **Verwalten…** aus.

   > [!Note]
   > Das Veröffentlichungsprofil wird im Projektmappen-Explorer in Visual Studio angezeigt. Die Profileinstellungen werden in eine Datei mit der Erweiterung ".azurePubxml" geschrieben. Einstellungen werden als Attribute der XML-Tags gespeichert.

1. Nachdem Sie alle Einstellungen für die Bereitstellung Ihres Projekts konfiguriert haben, wählen Sie unten im Dialogfeld **Veröffentlichen** aus. Sie können den Prozessstatus in Visual Studio im Ausgabefenster **Azure-Aktivitätsprotokoll** überwachen.

Glückwunsch! Sie haben Ihr Clouddienstprojekt mit erweitertem Support in Azure veröffentlicht. Wenn Sie eine weitere Veröffentlichung mit den gleichen Einstellungen vornehmen möchten, können Sie das Veröffentlichungsprofil wiederverwenden oder die angegebenen Schritte wiederholen, um ein neues zu erstellen.

## <a name="clean-up-azure-resources"></a>Bereinigen von Azure-Ressourcen

Wechseln Sie zum Bereinigen der Azure-Ressourcen, die Sie in diesem Tutorial erstellt haben, zum [Azure-Portal](https://portal.azure.com), und wählen Sie **Ressourcengruppen** aus. Suchen Sie nach der Ressourcengruppe, die Sie zum Erstellen des Diensts verwendet haben, öffnen Sie sie, und wählen Sie **Ressourcengruppe löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Richten Sie mithilfe der Schaltfläche **Konfigurieren** auf dem Bildschirm **Veröffentlichen** Continuous Integration (CI) ein. Weitere Informationen finden Sie in der [Dokumentation zu Azure Pipelines](/azure/devops/pipelines).