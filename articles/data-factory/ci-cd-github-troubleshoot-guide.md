---
title: Beheben von CI/CD-, Azure DevOps- und GitHub-Problemen in ADF
description: Verwenden Sie verschiedene Methoden, um CI/CD-Probleme in ADF zu beheben.
author: ssabat
ms.author: susabat
ms.reviewer: susabat
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 12/03/2020
ms.openlocfilehash: 5c33ef9559d9ce67eea62ee7f78425d18010c1cb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101727956"
---
# <a name="troubleshoot-ci-cd-azure-devops-and-github-issues-in-adf"></a>Beheben von CI/CD-, Azure DevOps- und GitHub-Problemen in ADF 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden gängige Methoden zur Problembehandlung für Continuous Integration/Continuous Deployment (CI/CD), Azure DevOps und GitHub in Azure Data Factory erläutert.

Wenn Sie Fragen oder Probleme bei der Quellcodeverwaltung oder DevOps-Verfahren haben, können die folgenden Artikel nützlich sein:

- Unter [Quellcodeverwaltung in Azure Data Factory](source-control.md) erfahren Sie, wie die Quellcodeverwaltung in ADF funktioniert. 
- Unter [Continuous Integration und Continuous Delivery in Azure Data Factory](continuous-integration-deployment.md) erfahren Sie, wie CI/CD für DevOps in ADF funktioniert.
 
## <a name="common-errors-and-messages"></a>Häufige Fehler und Meldungen

### <a name="connect-to-git-repository-failed-due-to-different-tenant"></a>Fehler bei der Verbindungsherstellung mit Git-Repository aufgrund eines anderen Mandanten

#### <a name="issue"></a>Problem
    
Zuweilen können Authentifizierungsprobleme auftreten, beispielsweise der HTTP-Statusfehler 401. Das Ganze kann ziemlich kompliziert werden – insbesondere dann, wenn Sie über mehrere Mandanten mit Gastkonto verfügen.

#### <a name="cause"></a>Ursache

Folgendes lässt sich beobachten: Das Token wurde vom ursprünglichen Mandanten abgerufen, aber ADF befindet sich im Gastmandanten und versucht, das Token zum Zugreifen auf DevOps im Gastmandanten zu verwenden. Dies entspricht nicht dem erwarteten Verhalten.

#### <a name="recommendation"></a>Empfehlung

Sie sollten stattdessen das Token verwenden, das vom Gastmandanten ausgegeben wurde. Sie müssen beispielsweise dieselbe Azure Active Directory-Instanz für den Gastmandanten wie für Ihre DevOps-Instanz zuweisen, sodass Azure Active Directory das Tokenverhalten ordnungsgemäß festlegen und den richtigen Mandanten verwenden kann.

### <a name="template-parameters-in-the-parameters-file-are-not-valid"></a>Vorlagenparameter in der Parameterdatei sind nicht gültig

#### <a name="issue"></a>Problem

Wenn Sie einen Trigger im Entwicklungsbranch löschen, der bereits mit **derselben** Konfiguration (z. B. Häufigkeit und Intervall) im Test- oder Produktionsbranch verfügbar ist, kann die Releasepipeline erfolgreich bereitgestellt werden, und der entsprechende Trigger wird in den jeweiligen Umgebungen gelöscht. Wenn jedoch **unterschiedliche** Konfigurationen (z. B. für Häufigkeit und Intervall) für Trigger in Test- und Produktionsumgebungen vorliegen und Sie diesen Trigger in der Entwicklungsumgebung löschen, tritt bei der Bereitstellung ein Fehler auf.

#### <a name="cause"></a>Ursache

Bei der CI/CD-Pipeline tritt der folgende Fehler auf:

`
2020-07-20T11:19:02.1276769Z ##[error]Deployment template validation failed: 'The template parameters 'Trigger_Salesforce_properties_typeProperties_recurrence_frequency, Trigger_Salesforce_properties_typeProperties_recurrence_interval, Trigger_Salesforce_properties_typeProperties_recurrence_startTime, Trigger_Salesforce_properties_typeProperties_recurrence_timeZone' in the parameters file are not valid; they are not present in the original template and can therefore not be provided at deployment time. The only supported parameters for this template are 'factoryName, PlanonDWH_connectionString, PlanonKeyVault_properties_typeProperties_baseUrl
`

#### <a name="recommendation"></a>Empfehlung

Dieser Fehler tritt, da häufig Trigger gelöscht werden, die mit Parametern konfiguriert wurden. Daher sind die Parameter in der ARM-Vorlage nicht verfügbar (da der Trigger nicht mehr existiert). Da sich die Parameter nicht mehr in der ARM-Vorlage befinden, müssen die überschriebenen Parameter in der DevOps-Pipeline aktualisiert werden. Andernfalls müssen Parameter bei jeder Änderung in der ARM-Vorlage die überschriebenen Parameter in der DevOps-Pipeline aktualisieren (im Bereitstellungstask).

### <a name="updating-property-type-is-not-supported"></a>Aktualisieren des Eigenschaftstyps wird nicht unterstützt

#### <a name="issue"></a>Problem

Bei der CI/CD-Releasepipeline tritt der folgende Fehler auf:

`
2020-07-06T09:50:50.8716614Z There were errors in your deployment. Error code: DeploymentFailed.
2020-07-06T09:50:50.8760242Z ##[error]At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/DeployOperations for usage details.
2020-07-06T09:50:50.8771655Z ##[error]Details:
2020-07-06T09:50:50.8772837Z ##[error]DataFactoryPropertyUpdateNotSupported: Updating property type is not supported.
2020-07-06T09:50:50.8774148Z ##[error]DataFactoryPropertyUpdateNotSupported: Updating property type is not supported.
2020-07-06T09:50:50.8775530Z ##[error]Check out the troubleshooting guide to see if your issue is addressed: https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment#troubleshooting
2020-07-06T09:50:50.8776801Z ##[error]Task failed while creating or updating the template deployment.
`

#### <a name="cause"></a>Ursache

Dies liegt an einer Integration Runtime, die in der Zielfactory den gleichen Namen, aber einen anderen Typ aufweist. Die Integration Runtime muss bei der Bereitstellung denselben Typ aufweisen.

#### <a name="recommendation"></a>Empfehlung

- Sehen Sie sich die folgenden Best Practices für CI/CD an:

    https://docs.microsoft.com/azure/data-factory/continuous-integration-deployment#best-practices-for-cicd 
- Integration Runtimes ändern sich nicht häufig und sind in allen CI/CD-Phasen gleich. Daher erwartet Data Factory, dass Sie in allen Phasen von CI/CD eine Integration Runtime mit demselben Namen und demselben Typ verwenden. Wenn sich die Namen, Typen und Eigenschaften unterscheiden, sollten Sie sicherstellen, dass Sie die Quell- und Zielkonfigurationen der Integration Runtime abgleichen und erst dann die Releasepipeline bereitstellen.
- Wenn Sie Integration Runtimes über alle Stufen hinweg freigeben möchten, können Sie eine ternäre Factory verwenden, die nur die freigegebenen Integration Runtimes enthält. Diese freigegebene Factory können Sie in allen Umgebungen als verknüpften Integration Runtime-Typ verwenden.

### <a name="document-creation-or-update-failed-because-of-invalid-reference"></a>Fehler bei Dokumenterstellung oder -aktualisierung aufgrund eines ungültigen Verweises

#### <a name="issue"></a>Problem

Beim Versuch, Änderungen an einer Data Factory zu veröffentlichen, erhalten Sie die folgende Fehlermeldung:

`
"error": {
        "code": "BadRequest",
        "message": "The document creation or update failed because of invalid reference '<entity>'.",
        "target": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/<rgname>/providers/Microsoft.DataFactory/factories/<datafactory>/pipelines/<pipeline>",
        "details": null
    }
`

#### <a name="symptom"></a>Symptom

Sie haben die Git-Konfiguration getrennt und mit ausgewähltem Flag „Import resources“ erneut eingerichtet. Damit wird die Data Factory auf „synchron“ festgelegt. Das bedeutet, dass keine Änderungen zur Veröffentlichung vorhanden sind.

#### <a name="resolution"></a>Lösung

Trennen Sie die Git-Konfiguration, und richten Sie sie erneut ein. Stellen Sie sicher, dass Sie das Kontrollkästchen zum Importieren vorhandener Ressourcen NICHT aktivieren.

### <a name="data-factory-move-failing-from-one-resource-group-to-another"></a>Fehler beim Verschieben von Data Factory von einer Ressourcengruppe in eine andere

#### <a name="issue"></a>Problem

Sie können Data Factory nicht von einer Ressourcengruppe in eine andere verschieben. Folgender Fehler tritt auf:

`
{
    "code": "ResourceMoveProviderValidationFailed",
    "message": "Resource move validation failed. Please see details. Diagnostic information: timestamp 'xxxxxxxxxxxxZ', subscription id 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', tracking id 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', request correlation id 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'.",
    "details": [
        {
            "code": "BadRequest",
            "target": "Microsoft.DataFactory/factories",
            "message": "One of the resources contain integration runtimes that are either SSIS-IRs in starting/started/stopping state, or Self-Hosted IRs which are shared with other resources. Resource move is not supported for those resources."
        }
    ]
}
`

#### <a name="resolution"></a>Lösung

Sie müssen die SSIS-IR und die freigegebenen IRs löschen, um den Verschiebungsvorgang zuzulassen. Wenn Sie die Integration Runtimes nicht löschen möchten, empfiehlt es sich, die Anweisungen im Dokument zum Kopieren und Klonen zu befolgen und anschließend die alte Data Factory zu löschen.

###  <a name="unable-to-export-and-import-arm-template"></a>Exportieren und Importieren von ARM-Vorlagen nicht möglich

#### <a name="issue"></a>Problem

Eine ARM-Vorlage kann nicht exportiert und importiert werden. Im Portal wurde kein Fehler angezeigt, aber in der Stapelüberwachung des Browsers bemerken Sie folgenden Fehler:

`Failed to load resource: the server responded with a status code of 401 (Unauthorized)`

#### <a name="cause"></a>Ursache

Sie haben als Benutzer eine Kundenrolle erstellt und verfügten nicht über die erforderliche Berechtigung. Wenn die Factory in die Benutzeroberfläche geladen wird, wird eine Reihe von Kontrollwerten zur Risikobewertung für die Factory überprüft. In diesem Fall besitzt die Zugriffsrolle des Benutzers keine Berechtigung für den Zugriff auf die *queryFeaturesValue*-API. Für den Zugriff auf diese API ist das Feature für globale Parameter deaktiviert. Der ARM-Exportcodepfad basiert zum Teil auf dem Feature für globale Parameter.

#### <a name="resolution"></a>Lösung

Um dieses Problem zu lösen, müssen Sie Ihrer Rolle die folgende Berechtigung hinzufügen: *Microsoft.DataFactory/factories/queryFeaturesValue/action*. Diese Berechtigung sollte standardmäßig in der Rolle „Data Factory-Mitwirkender“ enthalten sein.

###  <a name="automatic-publishing-for-cicd-without-clicking-publish-button"></a>Automatisches Veröffentlichen für CI/CD ohne Klick auf die Schaltfläche „Veröffentlichen“  

#### <a name="issue"></a>Problem

Beim manuellen Veröffentlichen per Klick auf die Schaltfläche im ADF-Portal wird der automatische CI/CD-Vorgang nicht aktiviert.

#### <a name="cause"></a>Ursache

Bis vor Kurzem war die einzige Möglichkeit zum Veröffentlichen einer ADF-Pipeline für Bereitstellungen das Klicken auf die Schaltfläche im ADF-Portal. Diesen Prozess können Sie nun automatisieren. 

#### <a name="resolution"></a>Lösung

Der CI/CD-Prozess wurde erweitert. Mit dem Feature **Automatisierte Veröffentlichung** werden alle ARM-Vorlagenfunktionen (Azure Resource Manager) aus der ADF-Benutzeroberfläche verwendet, überprüft und exportiert. Hierdurch kann die Logik über das öffentlich verfügbare npm-Paket [@microsoft/azure-data-factory-utilities](https://www.npmjs.com/package/@microsoft/azure-data-factory-utilities) genutzt werden. So können Sie diese Aktionen programmgesteuert initiieren und müssen nicht zur ADF-Benutzeroberfläche wechseln und auf Schaltflächen klicken. So werden Ihre CI/CD-Pipelines zu **echten** Continuous Integration-Prozessen. Weitere Informationen finden Sie unter [Automatisiertes Veröffentlichen für Continuous Integration und Delivery](./continuous-integration-deployment-improvements.md). 

###  <a name="cannot-publish-because-of-4mb-arm-template-limit"></a>Veröffentlichung aufgrund von 4-MB-Grenzwert für ARM-Vorlagen nicht möglich  

#### <a name="issue"></a>Problem

Sie können die Bereitstellung nicht durchführen, weil Sie für Azure Resource Manager den Grenzwert von 4 MB für die Vorlagengesamtgröße erreicht haben. Sie benötigen eine Lösung für die Bereitstellung nach dem Erreichen dieses Grenzwerts. 

#### <a name="cause"></a>Ursache

Für Azure Resource Manager ist die Vorlagengröße auf 4 MB beschränkt. Begrenzen Sie die Größe der Vorlage auf 4 MB und die jeder Parameterdatei auf 64 KB. Die 4-MB-Beschränkung gilt für den endgültigen Status der Vorlage, nachdem sie durch iterative Ressourcendefinitionen und Werte für variables und Parameter erweitert wurde. Sie haben den Grenzwert aber überschritten. 

#### <a name="resolution"></a>Lösung

Bei kleinen bis mittelgroßen Lösungen lässt sich eine Einzelvorlage einfacher verstehen und verwalten. Sie können alle Ressourcen und Werte in einer einzelnen Datei anzeigen. In erweiterten Szenarien können Sie mithilfe verknüpfter Vorlagen die Lösung in Zielkomponenten unterteilen. Halten Sie sich an die bewährte Methode unter [Verwenden von verknüpften und geschachtelten Vorlagen bei der Bereitstellung von Azure-Ressourcen](../azure-resource-manager/templates/linked-templates.md?tabs=azure-powershell).

### <a name="cannot-connect-to-git-enterprise"></a>Ich kann keine Verbindung mit Git Enterprise herstellen 

##### <a name="issue"></a>Problem

Aufgrund von Berechtigungsproblemen können Sie keine Verbindung mit Git Enterprise herstellen. Ein Fehler wie **422 – Einheit kann nicht bearbeitet werden.** wird angezeigt.

#### <a name="cause"></a>Ursache

Sie haben OAuth für ADF nicht konfiguriert. Ihre URL ist falsch konfiguriert.

##### <a name="resolution"></a>Lösung

Sie gewähren zuerst den OAuth-Zugriff auf ADF. Anschließend müssen Sie durch Angabe der richtigen URL eine Verbindung mit Git Enterprise herstellen. Die Konfiguration muss auf die Kundenorganisation(en) festgelegt werden, weil der ADF-Dienst zuerst https://hostname/api/v3/search/repositories?q=user%3<customer credential> ausprobiert und dann fehlschlägt. Anschließend probiert er https://hostname/api/v3/orgs/<vaorg>/<repo> aus, und das gelingt. 
 
### <a name="recover-from-a-deleted-data-factory"></a>Wiederstellen aus einer gelöschten Data Factory

#### <a name="issue"></a>Problem
Der Kunde hat die Data Factory oder die Ressourcengruppe mit der Data Factory gelöscht. Er möchte wissen, wie er eine gelöschte Data Factory wiederherstellen kann.

#### <a name="cause"></a>Ursache

Die Data Factory kann nur dann wiederhergestellt werden, wenn der Kunde die Quellcodeverwaltung konfiguriert hat (DevOps oder Git). Dadurch wird die neueste veröffentlichte Ressource angezeigt, und die unveröffentlichte Pipeline, das Dataset und der verknüpfte Dienst **werden nicht** wiederhergestellt.

Wenn es keine Quellcodeverwaltung gibt, kann eine gelöschte Data Factory aus dem Back-End nicht wiederhergestellt werden. Der Grund: Sobald der Dienst den Löschbefehl empfangen hat, wird die Instanz gelöscht, und es wurde keine Sicherung gespeichert.

#### <a name="resoloution"></a>Lösung
Führen Sie die folgenden Schritte aus, um die gelöschte Data Factory mit der Quellcodeverwaltung wiederherzustellen:

 * Erstellen Sie eine neue Azure Data Factory-Instanz.

 * Konfigurieren Sie Git mit denselben Einstellungen neu. Vergewissern Sie sich aber, dass „Import existing Data Factory resources to the selected repository“ (Vorhandene Data Factory-Ressourcen in das ausgewählte Repository importieren) aktiviert ist, und wählen Sie „Neuer Branch“ aus.

 * Erstellen Sie einen Pull Request zum Mergen der Änderungen in den Kollaborationsbranch und zum Veröffentlichen.

 * Wenn ein Kunde eine selbstgehostete Integration Runtime im gelöschten ADF hatte, muss er eine neue Instanz in der neuen ADF erstellen, sie dann auf dem lokalen Computer bzw. virtuellen Computer deinstallieren und erneut installieren, wobei der neue Schlüssel abgerufen wird. Nachdem der Setupvorgang für die IR abgeschlossen ist, muss der Kunde den verknüpften Dienst so ändern, dass er auf die neue IR verweist, und dann die Verbindung testen. Andernfalls tritt der Fehler **Ungültiger Verweis** auf.



## <a name="next-steps"></a>Nächste Schritte

Weitere Hilfe zur Problembehandlung finden Sie in den folgenden Ressourcen:

*  [Data Factory-Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory-Funktionsanfragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-Videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Stack Overflow-Forum für Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-Informationen über Data Factory](https://twitter.com/hashtag/DataFactory)
