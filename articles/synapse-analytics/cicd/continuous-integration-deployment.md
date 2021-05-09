---
title: Continuous Integration und Continuous Delivery für Synapse-Arbeitsbereiche
description: Erfahren Sie, wie Sie Continuous Integration und Continuous Delivery verwenden, um Änderungen an einem Arbeitsbereich aus einer Umgebung (Entwicklung, Test, Produktion) in eine andere zu überführen.
author: liudan66
ms.service: synapse-analytics
ms.subservice: cicd
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: liud
ms.reviewer: pimorano
ms.openlocfilehash: 2d49deef4cc7f646032219ff9e8f541cc9c1afd6
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108131185"
---
# <a name="continuous-integration-and-delivery-for-azure-synapse-workspace"></a>Continuous Integration und Continuous Delivery für Azure Synapse-Arbeitsbereiche

## <a name="overview"></a>Übersicht

Continuous Integration (CI) ist der Prozess, mit dem das Erstellen und Testen von Code bei jedem Committen von Änderungen durch Teammitglieder an die Versionskontrolle automatisiert werden kann. Continuous Deployment (CD) bezeichnet das Erstellen, Testen, Konfigurieren und Bereitstellen aus mehreren Test- oder Stagingumgebungen in einer Produktionsumgebung.

In einem Azure Synapse Analytics-Arbeitsbereich werden per Continuous Integration und Continuous Delivery (CI/CD) sämtliche Entitäten aus einer Umgebung (Entwicklung, Test, Produktion) in eine andere Umgebung verschoben. Das Höherstufen Ihres Arbeitsbereichs in einen anderen Arbeitsbereich umfasst zwei Schritte. Im ersten Schritt verwenden Sie eine [Azure Resource Manager-Vorlage (ARM-Vorlage)](../../azure-resource-manager/templates/overview.md), um Arbeitsbereichsressourcen (Pools und Arbeitsbereich) zu erstellen oder zu aktualisieren. Anschließend migrieren Sie Artefakte (SQL-Skripts, Notebooks, Spark-Auftragsdefinitionen, Pipelines, Datasets, Datenflüsse usw.) mit Azure Synapse Analytics-CI/CD-Tools in Azure DevOps. 

In diesem Artikel wird beschrieben, wie Sie eine Azure DevOps-Releasepipeline nutzen, um die Bereitstellung eines Azure Synapse-Arbeitsbereichs in mehreren Umgebungen zu automatisieren.

## <a name="prerequisites"></a>Voraussetzungen

Für eine Automatisierung der Bereitstellung eines Azure Synapse-Arbeitsbereichs in mehreren Umgebungen müssen die nachfolgend aufgeführten Voraussetzungen und Konfigurationen erfüllt bzw. vorhanden sein.

### <a name="azure-devops"></a>Azure DevOps

- Für die Ausführung der Releasepipeline wurde ein Azure DevOps-Projekt vorbereitet.
- [Gewähren Sie allen Benutzern, die Code einchecken, Basic-Zugriff auf Organisationsebene](/azure/devops/organizations/accounts/add-organization-users?view=azure-devops&tabs=preview-page&preserve-view=true), damit sie das Repository anzeigen können.
- Gewähren Sie Besitzerrechte für das Azure Synapse-Repository.
- Stellen Sie sicher, dass Sie einen selbstgehosteten Azure DevOps-VM-Agent erstellt haben, oder verwenden Sie einen von Azure DevOps gehosteten Agent.
- Berechtigungen zum [Erstellen einer Azure Resource Manager-Dienstverbindung (ARM) für die Ressourcengruppe](/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml&preserve-view=true).
- Ein Azure Active Directory-Administrator (Azure AD) muss die [Agent-Erweiterung für die Azure DevOps Synapse-Arbeitsbereichsbereitstellung in der Azure DevOps-Organisation installieren](/azure/devops/marketplace/install-extension).
- Erstellen Sie ein Dienstkonto für die Pipelineausführung, oder legen Sie für die Ausführung ein vorhandenes Dienstkonto fest. Sie können anstelle eines Dienstkontos ein persönliches Zugriffstoken verwenden, aber Ihre Pipelines funktionieren nicht mehr, wenn das Benutzerkonto gelöscht wird.

### <a name="azure-active-directory"></a>Azure Active Directory

- Erstellen Sie in Azure AD einen Dienstprinzipal, der für die Bereitstellung verwendet wird. Die Aufgabe zur Synapse-Arbeitsbereichsbereitstellung unterstützt in Version 1* und früheren Versionen keine Verwendung einer verwalteten Identität.
- Für diese Aktion werden Azure AD-Administratorrechte benötigt.

### <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

> [!NOTE]
> Sie können diese Voraussetzungen automatisieren und bereitstellen, indem Sie dieselbe Pipeline, eine ARM-Vorlage oder die Azure CLI verwenden. Die Vorgehensweise wird in diesem Artikel jedoch nicht beschrieben.

- Der für die Entwicklung verwendete „Quellarbeitsbereich“ muss mit einem Git-Repository in Synapse Studio konfiguriert werden. Weitere Informationen finden Sie unter [Quellcodeverwaltung in Synapse Studio](source-control.md#configuration-method-2-manage-hub).

- Ein leerer Arbeitsbereich, der als Ziel für Bereitstellung dient. So richten Sie den leeren Arbeitsbereich ein:

  1. Erstellen Sie einen neuen Azure Synapse Analytics-Arbeitsbereich.
  1. Gewähren Sie dem VM-Agent und dem Dienstprinzipal Rechte als Mitwirkender für die Ressourcengruppe, in der der neue Arbeitsbereich gehostet wird.
  1. Konfigurieren Sie im neuen Arbeitsbereich nicht die Git-Repositoryverbindung.
  1. Suchen Sie im Azure-Portal den neuen Azure Synapse Analytics-Arbeitsbereich, und gewähren Sie sich selbst und jedem Benutzer, der die Azure DevOps-Pipeline ausführen wird, Rechte als Azure Synapse Analytics-Arbeitsbereichsbesitzer. 
  1. Fügen Sie den Azure DevOps-VM-Agent und den Dienstprinzipal der Rolle „Mitwirkender“ für den Arbeitsbereich hinzu (diese sollte vererbt worden sein, aber überprüfen Sie, ob dies der Fall ist).
  1. Wechseln Sie im Azure Synapse Analytics-Arbeitsbereich zu **Studio** > **Verwalten** > **IAM**. Fügen Sie den Azure DevOps-VM-Agent und den Dienstprinzipal der Gruppe der Arbeitsbereichsadministratoren hinzu.
  1. Öffnen Sie das Speicherkonto, das für den Arbeitsbereich verwendet wird. Fügen Sie in IAM den VM-Agent und den Dienstprinzipal der Rolle „Mitwirkender an Storage-Blobdaten“ hinzu.
  1. Erstellen Sie einen Schlüsseltresor im Supportabonnement, und stellen Sie sicher, dass sowohl der vorhandene Arbeitsbereich als auch der neue Arbeitsbereich mindestens über GET- und LIST-Berechtigungen für den Tresor verfügen.
  1. Damit die automatisierte Bereitstellung funktioniert, stellen Sie sicher, dass alle Verbindungszeichenfolgen, die in Ihren verknüpften Diensten angegeben sind, im Schlüsseltresor vorhanden sind.

### <a name="additional-prerequisites"></a>Zusätzliche Voraussetzungen
 
 - Spark-Pools und selbstgehostete Integration Runtimes werden nicht in einer Pipeline erstellt. Wenn Sie über einen verknüpften Dienst verfügen, der eine selbstgehostete Integration Runtime verwendet, erstellen Sie diese manuell im neuen Arbeitsbereich.
 - Wenn Sie Notebooks entwickeln und diese mit einem Spark-Pool verbunden sind, erstellen Sie den Spark-Pool im Arbeitsbereich neu.
 - Notebooks, die mit einem Spark-Pool verknüpft sind, der nicht in einer Umgebung vorhanden ist, werden nicht bereitgestellt.
 - Die Namen der Spark-Pools müssen in beiden Arbeitsbereichen identisch sein.
 - Benennen Sie alle Datenbanken, SQL-Pools und weitere Ressourcen in beiden Arbeitsbereichen gleich.
 - Wenn Ihre bereitgestellten SQL-Pools zum Zeitpunkt der Bereitstellung angehalten sind, kann die Bereitstellung möglicherweise nicht erfolgreich durchgeführt werden.

Weitere Informationen finden Sie unter [CI CD in Azure Synapse Analytics, Teil 4: Die Releasepipeline](https://techcommunity.microsoft.com/t5/data-architecture-blog/ci-cd-in-azure-synapse-analytics-part-4-the-release-pipeline/ba-p/2034434). 


## <a name="set-up-a-release-pipeline"></a>Einrichten einer Releasepipeline

1.  Öffnen Sie in [Azure DevOps](https://dev.azure.com/) das Projekt, das für das Release erstellt wurde.

1.  Wählen Sie links auf der Seite die Option **Pipelines** und dann **Releases** aus.

    ![Auswählen von „Pipelines“ > „Releases“](media/create-release-1.png)

1.  Wählen Sie **Neue Pipeline** oder – falls Pipelines vorhanden sind – die Option **Neu** und dann **Neue Releasepipeline** aus.

1.  Wählen Sie die Vorlage **Leere Stufe** aus.

    ![Auswählen von „Leere Stufe“](media/create-release-select-empty.png)

1.  Geben Sie im Feld **Name der Stufe** den Namen der Umgebung ein.

1.  Wählen Sie **Artefakt hinzufügen** und dann das Git-Repository aus, das bei der Entwicklung in Synapse Studio konfiguriert wurde. Wählen Sie das Git-Repository aus, das Sie zum Verwalten der ARM-Vorlage für Pools und den Arbeitsbereich verwendet haben. Wenn Sie als Quelle GitHub verwenden, müssen Sie eine Dienstverbindung für Ihr GitHub-Konto erstellen und Repositorys pullen. Weitere Informationen zur [Dienstverbindung](/azure/devops/pipelines/library/service-endpoints) 

    ![Hinzufügen eines Branches für die Veröffentlichung](media/release-creation-github.png)

1.  Wählen Sie den Branch der ARM-Vorlage für das Ressourcenupdate aus. Wählen Sie unter **Standardversion** die Option **Letztes Element aus Standardbranch** aus.

    ![Hinzufügen einer ARM-Vorlage](media/release-creation-arm-branch.png)

1.  Wählen Sie den [Branch für die Veröffentlichung](source-control.md#configure-publishing-settings) des Repositorys für den **Standardbranch** aus. Dieser Branch für die Veröffentlichung lautet standardmäßig `workspace_publish`. Wählen Sie unter **Standardversion** die Option **Letztes Element aus Standardbranch** aus.

    ![Hinzufügen eines Artefakts](media/release-creation-publish-branch.png)

## <a name="set-up-a-stage-task-for-an-arm-template-to-create-and-update-resource"></a>Einrichten einer Stagingaufgabe für eine ARM-Vorlage zum Erstellen und Aktualisieren von Ressourcen 

Wenn Sie über eine ARM-Vorlage zum Bereitstellen einer Ressource – z. B. einen Azure Synapse Arbeitsbereich, Spark- und SQL-Pools oder einen Schlüsseltresor – verfügen, fügen Sie eine ARM-Bereitstellungsaufgabe hinzu, um diese Ressourcen zu erstellen oder zu aktualisieren:

1. Wählen Sie in der Stufenansicht die Option **Stufenaufgaben anzeigen** aus.

    ![Stufenansicht](media/release-creation-stage-view.png)

1. Erstellen Sie eine neue Aufgabe. Suchen Sie nach **AMR-Vorlagenbereitstellung**, und wählen Sie dann **Hinzufügen** aus.

1. Wählen Sie in der Bereitstellungsaufgabe das Abonnement, die Ressourcengruppe und den Speicherort für den Zielarbeitsbereich aus. Geben Sie die Anmeldeinformationen an, falls dies erforderlich ist.

1. Wählen Sie in der Liste **Aktion** den Eintrag **Create or update resource group** (Ressourcengruppe erstellen oder aktualisieren) aus.

1. Wählen Sie neben dem Feld **Vorlage** die Schaltfläche mit den Auslassungszeichen ( **…** ) aus. Suchen Sie nach der Azure Resource Manager-Vorlage für Ihren Zielarbeitsbereich.

1. Wählen Sie **…** neben dem Feld **Vorlagenparameter** aus, um die Parameterdatei auszuwählen.

1. Wählen Sie **…** neben dem Feld **Vorlagenparameter überschreiben** aus, und geben Sie die gewünschten Parameterwerte für den Zielarbeitsbereich ein. 

1. Wählen Sie unter **Bereitstellungsmodus** die Option **Inkrementell** aus.
    
    ![Bereitstellen von Arbeitsbereich und Pools](media/pools-resource-deploy.png)

1. (Optional:) Fügen Sie **Azure PowerShell** hinzu, um die Rollenzuweisung für den Arbeitsbereich festzulegen und zu aktualisieren. Wenn Sie einen Synapse-Arbeitsbereich mit der Releasepipeline erstellen, wird der Dienstprinzipal der Pipeline als Standardadministrator für den Arbeitsbereich hinzugefügt. Sie können PowerShell ausführen, um auch anderen Konten Zugriff auf den Arbeitsbereich zu gewähren. 
    
    ![Erteilen einer Berechtigung](media/release-creation-grant-permission.png)

 > [!WARNING]
> Im vollständigen Bereitstellungsmodus werden Ressourcen, die in der Ressourcengruppe vorhanden, aber in der neuen Resource Manager-Vorlage nicht angegeben sind, **gelöscht**. Weitere Informationen finden Sie unter [Azure Resource Manager-Bereitstellungsmodi](../../azure-resource-manager/templates/deployment-modes.md).

## <a name="set-up-a-stage-task-for-synapse-artifacts-deployment"></a>Einrichten einer Stagingaufgabe für die Synapse-Artefaktbereitstellung 

Verwenden Sie die Erweiterung für die [Synapse-Arbeitsbereichsbereitstellung](https://marketplace.visualstudio.com/items?itemName=AzureSynapseWorkspace.synapsecicd-deploy), um andere Elemente im Synapse-Arbeitsbereich wie Datasets, SQL-Skripts, Notebooks, Spark-Auftragsdefinitionen, Datenflüsse, Pipelines, verknüpfte Dienste, Anmeldeinformationen und IRs (Integration Runtime) bereitzustellen.  

1. Suchen Sie die Erweiterung im **Azure DevOps-Marketplace** (https://marketplace.visualstudio.com/azuredevops), und laden Sie sie herunter. 

     ![Abrufen einer Erweiterung](media/get-extension-from-market.png)

1. Wählen Sie eine Organisation aus, um die Erweiterung zu installieren. 

     ![Installieren der Erweiterung](media/install-extension.png)

1. Stellen Sie sicher, dass dem Dienstprinzipal der Azure DevOps-Pipeline die Berechtigung des Abonnements erteilt wurde und dass er auch als Arbeitsbereichsadministrator für den Zielarbeitsbereich zugewiesen wurde. 

1. Erstellen Sie eine neue Aufgabe. Suchen Sie nach **Synapse-Arbeitsbereichsbereitstellung**, und wählen Sie **Hinzufügen** aus.

     ![Erweiterung hinzufügen](media/add-extension-task.png)

1.  Wählen Sie in der Aufgabe **...** neben dem Feld **Vorlage** aus, um die Vorlagendatei auszuwählen.

1. Wählen Sie **…** neben dem Feld **Vorlagenparameter** aus, um die Parameterdatei auszuwählen.

1. Wählen Sie die Verbindung, die Ressourcengruppe und den Namen des Zielarbeitsbereichs aus. 

1. Wählen Sie **…** neben dem Feld **Vorlagenparameter außer Kraft setzen** aus, und geben Sie die gewünschten Parameterwerte für den Zielarbeitsbereich ein, darunter auch die Verbindungszeichenfolgen und Kontoschlüssel, die in Ihren verknüpften Diensten verwendet werden. [Klicken Sie hier, um weitere Informationen zu erhalten.] (https://techcommunity.microsoft.com/t5/data-architecture-blog/ci-cd-in-azure-synapse-analytics-part-4-the-release-pipeline/ba-p/2034434)

    ![Synapse-Arbeitsbereichsbereitstellung](media/create-release-artifacts-deployment.png)

> [!IMPORTANT]
> In CI/CD-Szenarien muss der Typ der Integration Runtime (IR) in unterschiedlichen Umgebungen identisch sein. Wenn sich in Ihrer Entwicklungsumgebung beispielsweise eine selbstgehostete IR befindet, muss diese IR auch in anderen Umgebungen, z. B. Test- und Produktionsumgebungen, den Typ „Selbstgehostet“ aufweisen. Wenn Sie Integration Runtimes in mehreren Stufen freigeben, müssen Sie die Integration Runtimes in allen Umgebungen, z. B. Entwicklungs-, Test- und Produktionsumgebungen, als „Verknüpft selbstgehostet“ konfigurieren.

## <a name="create-release-for-deployment"></a>Erstellen eines Release für die Bereitstellung 

Nachdem alle Änderungen gespeichert wurden, können Sie **Release erstellen** auswählen, um manuell ein Release zu erstellen. Informationen zum Automatisieren der Erstellung von Releases finden Sie unter [Azure DevOps-Releasetrigger](/azure/devops/pipelines/release/triggers).

   ![Auswählen von „Release erstellen“](media/release-creation-manually.png)

## <a name="use-custom-parameters-of-the-workspace-template"></a>Verwenden von benutzerdefinierten Parametern der Arbeitsbereichsvorlage 

Sie verwenden automatisierte CI/CD und möchten bei der Bereitstellung einige Eigenschaften ändern, die Eigenschaften sind standardmäßig aber nicht parametrisiert. In diesem Fall können Sie die Vorlage mit den Standardparametern außer Kraft setzen.

Zum Außerkraftsetzen der Vorlage mit den Standardparametern müssen Sie im Stammordner Ihres Git-Kollaborationsbranchs eine benutzerdefinierte Parametervorlage mit dem Dateinamen **template-parameters-definition.json** erstellen. Sie müssen exakt diesen Dateinamen verwenden. Beim Veröffentlichen aus dem Kollaborationsbranch liest der Synapse-Arbeitsbereich diese Datei und verwendet deren Konfiguration, um die Parameter zu konfigurieren. Sollte keine Datei gefunden werden, wird die Standardparametervorlage verwendet.

### <a name="custom-parameter-syntax"></a>Benutzerdefinierte Parametersyntax

Im Folgenden finden Sie einige Richtlinien zum Erstellen der benutzerdefinierten Parameterdatei:

* Geben Sie den Eigenschaftenpfad unter dem relevanten Entitätstyp ein.
* Durch das Festlegen eines Eigenschaftennamens auf `*` geben Sie an, dass alle untergeordneten Eigenschaften parametrisiert werden sollen (nicht rekursiv, sondern nur bis zur ersten Ebene). Sie können auch Ausnahmen für diese Konfiguration angeben.
* Wenn Sie den Wert einer Eigenschaft als Zeichenfolge festlegen, geben Sie damit an, dass die Eigenschaft parametrisiert werden soll. Verwenden Sie das Format `<action>:<name>:<stype>`.
   *  `<action>` kann für eines dieser Zeichen durchgeführt werden:
      * `=` bedeutet, dass der aktuelle Wert als Standardwert für den Parameter beibehalten werden soll.
      * `-` bedeutet, dass der Standardwert für den Parameter nicht beibehalten werden soll.
      * `|` ist ein Sonderfall für Geheimnisse aus Azure Key Vault für Verbindungszeichenfolgen oder Schlüssel.
   * `<name>` ist der Name des Parameters. Wenn dieser Wert leer ist, wird der Name der Eigenschaft verwendet. Beginnt der Wert mit dem Zeichen `-`, wird der Name gekürzt. `AzureStorage1_properties_typeProperties_connectionString` wird beispielsweise in `AzureStorage1_connectionString` gekürzt.
   * `<stype>` ist der Typ des Parameters. Wenn `<stype>` leer ist, wird standardmäßig der Typ `string` verwendet. Unterstützte Werte: `string`, `securestring`, `int`, `bool`, `object`, `secureobject` und `array`.
* Wenn Sie ein Array in der Datei angeben, bedeutet dies, dass die entsprechende Eigenschaft in der Vorlage ein Array ist. Synapse durchläuft alle Objekte im Array anhand der angegebenen Definition. Das zweite Objekt (eine Zeichenfolge) wird zum Namen der Eigenschaft, der bei jeder Iteration als Name für den Parameter verwendet wird.
* Eine Definition kann nicht spezifisch für eine Ressourceninstanz sein. Jede Definition gilt für alle Ressourcen dieses Typs.
* Standardmäßig werden alle sicheren Zeichenfolgen parametrisiert, z. B. Key Vault-Geheimnisse, Verbindungszeichenfolgen, Schlüssel und Token.

### <a name="parameter-template-definition-samples"></a>Beispiele für Parametervorlagendefinitionen 

Im Folgenden finden Sie ein Beispiel für eine Parametervorlagendefinition:

```json
{
"Microsoft.Synapse/workspaces/notebooks": {
        "properties":{
            "bigDataPool":{
                "referenceName": "="
            }
        }
    },
    "Microsoft.Synapse/workspaces/sqlscripts": {
     "properties": {
         "content":{
             "currentConnection":{
                    "*":"-"
                 }
            } 
        }
    },
    "Microsoft.Synapse/workspaces/pipelines": {
        "properties": {
            "activities": [{
                 "typeProperties": {
                    "waitTimeInSeconds": "-::int",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.Synapse/workspaces/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.Synapse/workspaces/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:int",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.Synapse/workspaces/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                     "*": "="
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.Synapse/workspaces/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```

Im Folgenden wird das Erstellen der obigen Vorlage mit einer Aufschlüsselung nach Ressourcentypen beschrieben.

#### <a name="notebooks"></a>Notebooks 

* Alle Eigenschaften im Pfad `properties/bigDataPool/referenceName` sind mit ihrem jeweiligen Standardwert parametrisiert. Sie können einen angefügten Spark-Pool für jede Notebook-Datei parametrisieren. 

#### <a name="sql-scripts"></a>SQL-Skripts 

* Eigenschaften (poolName und databaseName) im Pfad `properties/content/currentConnection` sind als Zeichenfolgen ohne die Standardwerte in der Vorlage parametrisiert. 

#### <a name="pipelines"></a>Pipelines

* Jede Eigenschaft im Pfad `activities/typeProperties/waitTimeInSeconds` wird parametrisiert. Jede Aktivität in einer Pipeline, die eine Eigenschaft auf Codeebene mit dem Namen `waitTimeInSeconds` enthält (z. B. die Aktivität `Wait`), wird als Zahl mit einem Standardnamen parametrisiert. Die Aktivität verfügt jedoch nicht über einen Standardwert in der Resource Manager-Vorlage. Hierbei handelt es sich um eine erforderliche Eingabe bei der Resource Manager-Bereitstellung.
* Analog dazu wird eine Eigenschaft namens `headers` (etwa in einer Aktivität vom Typ `Web`) mit dem Typ `object` (Objekt) parametrisiert. Sie verfügt über einen Standardwert (gleicher Wert wie für die Quellfactory).

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Alle Eigenschaften unter dem Pfad `typeProperties` werden mit ihren jeweiligen Standardwerten parametrisiert. Beispielsweise sind unter Eigenschaften vom Typ `IntegrationRuntimes` zwei Eigenschaften vorhanden: `computeProperties` und `ssisProperties`. Beide Eigenschaftentypen werden mit ihren jeweiligen Standardwerten und -typen (Objekt) erstellt.

#### <a name="triggers"></a>Trigger

* Unter `typeProperties` werden zwei Eigenschaften parametrisiert. Die erste ist `maxConcurrency`. Diese Eigenschaft besitzt einen Standardwert und ist vom Typ `string`. Der Standardparametername lautet `<entityName>_properties_typeProperties_maxConcurrency`.
* Die Eigenschaft `recurrence` wird ebenfalls parametrisiert. Darunter werden alle Eigenschaften auf dieser Ebene gemäß Angabe als Zeichenfolgen mit Standardwerten und Parameternamen parametrisiert. Eine Ausnahme ist die `interval`-Eigenschaft, für die beim Parametrisieren der Typ `int` verwendet wird. An den Parameternamen ist das Suffix `<entityName>_properties_typeProperties_recurrence_triggerSuffix` angehängt. Analog dazu ist die Eigenschaft `freq` eine Zeichenfolge und wird als Zeichenfolge parametrisiert. Die Eigenschaft `freq` wird jedoch ohne Standardwert parametrisiert. Der Name wird verkürzt und mit einem Suffix versehen. Beispiel: `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Verknüpfte Dienste sind ein Sonderfall. Da verknüpfte Dienste und Datasets eine breite Palette von Typen umfassen, können Sie eine typspezifische Anpassung vornehmen. In diesem Beispiel wird für alle verknüpften Dienste vom Typ `AzureDataLakeStore` eine bestimmte Vorlage angewendet. Für alle anderen Dienste wird eine andere Vorlage angewendet (per `*`).
* Die `connectionString`-Eigenschaft wird als `securestring`-Wert parametrisiert. Sie hat keinen Standardwert. Sie weist einen verkürzten Parameternamen auf, an den das Suffix `connectionString` angehängt ist.
* Die `secretAccessKey`-Eigenschaft ist eine Eigenschaft vom Typ `AzureKeyVaultSecret` (beispielsweise in einem verknüpften Amazon S3-Dienst). Sie wird automatisch als Azure Key Vault-Geheimnis parametrisiert und aus dem konfigurierten Schlüsseltresor abgerufen. Auch der Schlüsseltresor kann parametrisiert werden.

#### <a name="datasets"></a>Datasets

* Für Datasets steht zwar eine typspezifische Anpassung zur Verfügung, aber Sie können die Konfiguration durchführen, ohne dass eine explizite Konfiguration auf der Ebene \* vorhanden sein muss. Im vorherigen Beispiel werden alle Dataseteigenschaften unter `typeProperties` parametrisiert.


## <a name="best-practices-for-cicd"></a>Bewährte Methoden für CI/CD

Wenn Sie die Git-Integration für Ihren Azure Synapse-Arbeitsbereich verwenden und über eine CI/CD-Pipeline verfügen, die Ihre Änderungen aus der Entwicklungs- in die Test- und dann in die Produktionsumgebung verschiebt, werden diese Best Practices empfohlen:

-   **Git-Integration**. Konfigurieren Sie nur Ihren Azure Synapse-Arbeitsbereich für die Entwicklung mit Git-Integration. Änderungen an den Test- und Produktionsarbeitsbereichen werden über CI/CD bereitgestellt, und eine Git-Integration wird hierfür nicht benötigt.
-   **Bereiten Sie vor der Migration von Artefakten Pools vor.** Wenn Sie über SQL-Skripts oder Notebooks verfügen, die im Entwicklungsarbeitsbereich an Pools angefügt sind, werden in den anderen Umgebungen dieselben Poolnamen erwartet. 
-   **Infrastruktur als Code (Infrastructure-as-Code, IaC).** Verwenden Sie für die Verwaltung von Infrastruktur (Netzwerken, VMs, Lastenausgleichsmodulen und der Verbindungstopologie) in einem beschreibenden Modell die gleiche Versionsverwaltung, die das DevOps-Team für den Quellcode nutzt. 
-   **Sonstige.** Weitere Informationen finden Sie unter [Bewährte Methoden für ADF-Artefakte](../../data-factory/continuous-integration-deployment.md#best-practices-for-cicd).

## <a name="troubleshooting-artifacts-deployment"></a>Problembehandlung bei der Bereitstellung von Artefakten 

### <a name="use-the-azure-synapse-analytics-workspace-deployment-task"></a>Verwenden der Aufgabe für die Azure Synapse Analytics-Arbeitsbereichsbereitstellung

In Azure Synapse Analytics gibt es eine Reihe von Artefakten, bei denen es sich nicht um ARM-Ressourcen handelt. Dies ist ein Unterschied zu Azure Data Factory. Die Aufgabe zur ARM-Vorlagenbereitstellung kann Azure Synapse Analytics-Artefakte nicht ordnungsgemäß bereitstellen.
 
### <a name="unexpected-token-error-in-release"></a>Unerwarteter Tokenfehler in Release

Wenn Ihre Parameterdatei Parameterwerte enthält, die nicht mit Escapezeichen versehen sind, würde beim Analysieren der Datei durch die Releasepipeline der Fehler „Unerwartetes Token“ auftreten. Es wird empfohlen, die Parameter zu überschreiben oder die Parameterwerte mithilfe von Azure Key Vault abzurufen. Als Abhilfe können Sie auch doppelte Escapezeichen verwenden.
