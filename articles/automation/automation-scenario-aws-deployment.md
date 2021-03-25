---
title: Bereitstellen eines virtuellen Amazon Web Services-Computers mit einem Azure Automation-Runbook
description: In diesem Artikel erfahren Sie, wie Sie die Erstellung eines virtuellen Amazon Web Services-Computers automatisieren.
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
ms.openlocfilehash: 03f1f1659d120a2aa1b827063cb2bc84138f3655
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98896069"
---
# <a name="deploy-an-amazon-web-services-vm-with-a-runbook"></a>Bereitstellen eines virtuellen Amazon Web Services-Computers mit einem Runbook

In diesem Artikel erfahren Sie, wie Sie Azure Automation zur Bereitstellung eines virtuellen Computers in Ihrem AWS-Abonnement (Amazon Web Services) nutzen und für diesen virtuellen Computer einen bestimmten Namen festlegen können – ein Vorgang, der in AWS als „Tagging“ des virtuellen Computers bezeichnet wird.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen ein Azure Automation-Konto und ein AWS-Abonnement (Amazon Web Services). Weitere Informationen zum Einrichten eines Azure Automation-Kontos sowie zum Konfigurieren dieses Kontos mit den Anmeldeinformationen Ihres AWS-Abonnements finden Sie unter [Authentifizieren mit Amazon Web Services konfigurieren](automation-config-aws-account.md). Da in den nachfolgenden Abschnitten auf dieses Konto verwiesen wird, sollte es mit den Anmeldeinformationen Ihres AWS-Abonnements erstellt oder aktualisiert werden, bevor Sie fortfahren.

## <a name="deploy-amazon-web-services-powershell-module"></a>Bereitstellen des Amazon Web Services-PowerShell-Moduls

Ihr Runbook für die VM-Bereitstellung nutzt das Modul AWSPowerShell zum Erledigen seiner Aufgaben. Führen Sie die folgenden Schritte aus, um das Modul Ihrem Automation-Konto hinzuzufügen, das mit den Anmeldeinformationen Ihres AWS-Abonnements konfiguriert ist.  

1. Öffnen Sie Ihren Webbrowser, navigieren Sie zum [PowerShell-Katalog](https://www.powershellgallery.com/packages/AWSPowerShell/), und klicken Sie auf **In Azure Automation bereitstellen**.<br><br> ![Importieren des AWS-PS-Moduls](./media/automation-scenario-aws-deployment/powershell-gallery-download-awsmodule.png)
2. Sie werden auf die Azure-Anmeldeseite geleitet und nach der Authentifizierung zum Azure-Portal weitergeleitet, in dem die folgende Seite angezeigt wird:<br><br> ![Seite zum Importieren des Moduls](./media/automation-scenario-aws-deployment/deploy-aws-powershell-module-parameters.png)
3. Wählen Sie das zu verwendende Automation-Konto aus, und klicken Sie auf **OK**, um die Bereitstellung zu starten.

   > [!NOTE]
   > Wenn Azure Automation ein PowerShell-Modul importiert, werden die Cmdlets extrahiert. Die Aktivitäten werden erst angezeigt, nachdem der Import des Moduls und das Extrahieren der Cmdlets von Automation vollständig abgeschlossen wurde. Dieser Vorgang kann einige Minuten dauern.  
   > <br>

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Klicken Sie auf die Kachel **Assets**, und wählen Sie im Bereich „Assets“ die Kachel **Module** aus.
3. Auf der Seite **Module** wird das Modul AWSPowerShell in der Liste angezeigt.

## <a name="create-aws-deploy-vm-runbook"></a>Erstellen eines Runbooks zur Bereitstellung einer VM in AWS

Nach der Bereitstellung des AWS-PowerShell-Moduls können Sie nun ein Runbook erstellen, mit dem sich die Bereitstellung eines virtuellen Computers in AWS unter Verwendung eines PowerShell-Skripts automatisieren lässt. Die nachstehenden Schritte zeigen, wie Sie ein natives PowerShell-Skript in Azure Automation nutzen.  

> [!NOTE]
> Weitere Optionen und Informationen zu diesem Skript finden Sie im [PowerShell-Katalog](https://www.powershellgallery.com/packages/New-AwsVM/).
> 

1. Laden Sie das PowerShell-Skript New-AwsVM aus dem PowerShell-Katalog herunter. Öffnen Sie dazu eine PowerShell-Sitzung, und geben Sie folgenden Befehl ein:<br>
   ```powershell
   Save-Script -Name New-AwsVM -Path <path>
   ```
   <br>
2. Öffnen Sie im Azure-Portal Ihr Automation-Konto, und wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus.  
3. Wählen Sie auf der Seite „Runbooks“ die Option **Runbook hinzufügen**.
4. Wählen Sie im Bereich „Runbook hinzufügen“ die Option **Schnellerfassung**, um ein neues Runbook zu erstellen.
5. Geben Sie im Bereich „Runbook-Eigenschaften“ einen Namen für das Runbook ein.
6. Wählen Sie in der Dropdownliste **Runbooktyp** die Option **PowerShell** aus, und klicken Sie dann auf **Erstellen**.<br><br> ![Bereich zum Erstellen eines Runbooks](./media/automation-scenario-aws-deployment/runbook-quickcreate-properties.png)
7. Wenn die Seite „PowerShell-Runbook bearbeiten“ angezeigt wird, kopieren Sie das PowerShell-Skript, und fügen Sie es in der Canvas zur Runbookerstellung ein.<br><br> ![Runbook-PowerShell-Skript](./media/automation-scenario-aws-deployment/runbook-powershell-script.png)<br>
   
    Beachten Sie Folgendes, wenn Sie mit dem PowerShell-Beispielskript arbeiten:

    * Das Runbook enthält eine Reihe von Standardparameterwerten. Evaluieren Sie alle Standardwerte, und aktualisieren Sie diese bei Bedarf.
    * Wenn Sie Ihre AWS-Anmeldeinformationen als Anmeldeinformationsasset gespeichert haben, dessen Name nicht `AWScred` lautet, müssen Sie das Skript in Zeile 57 entsprechend aktualisieren.  
    * Bei der Arbeit mit den AWS-CLI-Befehlen in PowerShell müssen Sie – insbesondere bei diesem Beispielrunbook – die AWS-Region angeben. Andernfalls tritt bei den Cmdlets ein Fehler auf. Weitere Einzelheiten finden Sie im Dokument zu den AWS-Tools für PowerShell im AWS-Thema [Specify AWS Region (Angeben der AWS-Region)](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-installing-specifying-region.html) .  

8. Um eine Liste mit Imagenamen aus Ihrem AWS-Abonnement abzurufen, starten Sie PowerShell ISE, und importieren Sie das AWS-PowerShell-Modul. Führen Sie die Authentifizierung bei AWS durch, indem Sie `Get-AutomationPSCredential` in Ihrer ISE-Umgebung durch `AWScred = Get-Credential` ersetzen. Diese Anweisung fordert Sie zur Eingabe Ihrer Anmeldeinformationen auf. Sie können Ihre Zugriffsschlüssel-ID als Benutzername und Ihren geheimen Zugriffsschlüssel als Kennwort eingeben. 

      ```powershell
        #Sample to get the AWS VM available images
        #Please provide the path where you have downloaded the AWS PowerShell module
        Import-Module AWSPowerShell
        $AwsRegion = "us-west-2"
        $AwsCred = Get-Credential
        $AwsAccessKeyId = $AwsCred.UserName
        $AwsSecretKey = $AwsCred.GetNetworkCredential().Password
   
        # Set up the environment to access AWS
        Set-AwsCredentials -AccessKey $AwsAccessKeyId -SecretKey $AwsSecretKey -StoreAs AWSProfile
        Set-DefaultAWSRegion -Region $AwsRegion
   
        Get-EC2ImageByName -ProfileName AWSProfile
      ```
        
    Die folgende Ausgabe wird zurückgegeben:<br><br>
   ![AWS-Images abrufen](./media/automation-scenario-aws-deployment/powershell-ise-output.png)<br>  
9. Kopieren Sie einen der Imagenamen, und fügen Sie ihn in eine Automation-Variable ein, auf die im Runbook als `$InstanceType` verwiesen wird. Da in diesem Beispiel ein kostenloses AWS-Abonnement eingesetzt wird, verwenden Sie für Ihr Beispielrunbook **t2.micro**.  
10. Speichern Sie das Runbook, und klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen. Klicken Sie dann bei Aufforderung auf **Ja**.

### <a name="test-the-aws-vm-runbook"></a>Testen des AWS-VM-Runbooks

1. Vergewissern Sie sich, dass durch das Runbook ein Objekt namens `AWScred` für die Authentifizierung bei AWS erstellt wird, oder aktualisieren Sie das Skript, um auf den Namen Ihres Anmeldeinformationsobjekts zu verweisen.    
2. Vergewissern Sie sich, dass alle Parameterwerte in Ihrem neuen Runbook ordnungsgemäß aktualisiert wurden.
Vergewissern Sie sich, dass das AWS-PowerShell-Modul in Azure Automation importiert wurde.  
3. Legen Sie in Azure Automation unter dem Runbookvorgang **Protokollierung und Nachverfolgung** die Option **Ausführliche Datensätze protokollieren** und optional die Option **Statusdatensätze protokollieren** auf **Ein** fest.<br><br> ![Runbookprotokollierung und -nachverfolgung](./media/automation-scenario-aws-deployment/runbook-settings-logging-and-tracing.png).  
4. Klicken Sie auf **Starten**, um das Runbook zu starten, und dann im eingeblendeten Bereich „Runbook starten“ auf **OK**.
5. Geben Sie im Bereich „Runbook starten“ einen VM-Namen an. Übernehmen Sie die Standardwerte für die anderen Parameter, die Sie im Skript vorkonfiguriert haben. Klicken Sie auf **OK**, um den Runbookauftrag zu starten.<br><br> ![New-AwsVM-Runbook starten](./media/automation-scenario-aws-deployment/runbook-start-job-parameters.png)
6. Ein Auftragsbereich für den soeben erstellten Runbookauftrag wird angezeigt. Schließen Sie diesen Bereich.
7. Sie können den Status des Auftrags und die Ausgabedatenströme anzeigen, indem Sie auf der Seite des Runbookauftrags auf die Kachel **Alle Protokolle** klicken.<br><br> ![Datenstromausgabe](./media/automation-scenario-aws-deployment/runbook-job-streams-output.png)
8. Um die Bereitstellung der VM zu bestätigen, melden Sie sich bei der AWS-Verwaltungskonsole an, sofern Sie noch nicht angemeldet sind.<br><br> ![Per AWS-Konsole bereitgestellte VM](./media/automation-scenario-aws-deployment/aws-instances-status.png)

## <a name="next-steps"></a>Nächste Schritte
 
* Informationen zu unterstützten Runbooks finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md).
* Informationen zur Arbeit mit Runbooks finden Sie unter [Verwalten eines Runbooks in Azure Automation](manage-runbooks.md).
* Details zu PowerShell finden Sie in der [PowerShell-Dokumentation](/powershell/scripting/overview).
* Informationen zur Skriptunterstützung finden Sie in der [Ankündigung der nativen PowerShell-Skriptunterstützung in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](/powershell/module/az.automation).
