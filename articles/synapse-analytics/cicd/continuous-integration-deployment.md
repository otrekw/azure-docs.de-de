---
title: Continuous Integration und Continuous Delivery für Synapse-Arbeitsbereiche
description: Erfahren Sie, wie Sie Continuous Integration und Continuous Delivery verwenden, um Änderungen an einem Arbeitsbereich aus einer Umgebung (Entwicklung, Test, Produktion) in eine andere zu überführen.
services: synapse-analytics
author: liud
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: liud
ms.reviewer: pimorano
ms.openlocfilehash: de3738573bb9bb6f045a45d290c74ba9e6902a5e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103561956"
---
# <a name="continuous-integration-and-delivery-for-azure-synapse-workspace"></a>Continuous Integration und Continuous Delivery für Azure Synapse-Arbeitsbereiche

## <a name="overview"></a>Übersicht

Continuous Integration (CI) ist der Prozess, mit dem das Erstellen und Testen von Code bei jedem Committen von Änderungen durch Teammitglieder an die Versionskontrolle automatisiert werden kann. Continuous Deployment (CD) bezeichnet das Erstellen, Testen, Konfigurieren und Bereitstellen aus mehreren Test- oder Stagingumgebungen in einer Produktionsumgebung.

Bei Azure Synapse-Arbeitsbereichen dienen Continuous Integration und Continuous Delivery (CI/CD) dazu, sämtliche Entitäten aus einer Umgebung (Entwicklung, Test, Produktion) in eine andere zu verschieben. Um Ihren Arbeitsbereich auf einen anderen Arbeitsbereich höher zu stufen, müssen Sie zwei Schritte ausführen: Verwenden Sie [Azure Resource Manager-Vorlagen](../../azure-resource-manager/templates/overview.md), um Arbeitsbereichsressourcen (Pools und Arbeitsbereich) zu erstellen oder zu aktualisieren, und migrieren Sie Artefakte (SQL-Skripts, Notebooks, Spark-Auftragsdefinitionen, Pipelines, Datasets, Datenflüsse usw.) mit Synapse-CI/CD-Tools zu Azure DevOps. 

In diesem Artikel wird die Verwendung von Azure-Releasepipelines zum Automatisieren der Bereitstellung eines Synapse-Arbeitsbereichs in mehreren Umgebungen beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

-   Der für die Entwicklung verwendete Arbeitsbereich wurde mit einem Git-Repository in Studio konfiguriert. Informationen hierzu finden Sie unter [Quellcodeverwaltung in Synapse Studio](source-control.md).
-   Ein Azure DevOps-Projekt wurde für die Ausführung der Releasepipeline vorbereitet.

## <a name="set-up-a-release-pipelines"></a>Einrichten einer Releasepipeline

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

## <a name="set-up-a-stage-task-for-arm-resource-create-and-update"></a>Einrichten einer Stagingaufgabe für das Erstellen und Aktualisieren von ARM-Ressourcen 

Fügen Sie eine Azure Resource Manager-Bereitstellungsaufgabe hinzu, um Ressourcen wie den Arbeitsbereich und Pools zu erstellen oder zu aktualisieren:

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

## <a name="set-up-a-stage-task-for-artifacts-deployment"></a>Einrichten einer Stagingaufgabe für die Artefaktbereitstellung 

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

1. Wählen Sie **…** neben dem Feld **Vorlagenparameter überschreiben** aus, und geben Sie die gewünschten Parameterwerte für den Zielarbeitsbereich ein. 

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

Wenn Sie die Git-Integration mit Ihre, Synapse-Arbeitsbereich verwenden und über eine CI/CD-Pipeline verfügen, die Ihre Änderungen aus der Entwicklungs- in die Test- und dann in die Produktionsumgebung verschiebt, empfehlen wir Ihnen diese bewährten Methoden:

-   **Git-Integration**. Konfigurieren Sie nur Ihren Synapse-Arbeitsbereich für die Entwicklung mit Git-Integration. Änderungen an den Test- und Produktionsarbeitsbereichen werden über CI/CD bereitgestellt, und eine Git-Integration wird hierfür nicht benötigt.
-   **Bereiten Sie vor der Migration von Artefakten Pools vor.** Wenn Sie über SQL-Skripts oder Notebooks verfügen, die im Entwicklungsarbeitsbereich an Pools angefügt sind, werden in den anderen Umgebungen dieselben Poolnamen erwartet. 
-   **Infrastruktur als Code (Infrastructure-as-Code, IaC).** Verwenden Sie für die Verwaltung von Infrastruktur (Netzwerken, VMs, Lastenausgleichsmodulen und der Verbindungstopologie) in einem beschreibenden Modell die gleiche Versionsverwaltung, die das DevOps-Team für den Quellcode nutzt. 
-   **Sonstige.** Weitere Informationen finden Sie unter [Bewährte Methoden für ADF-Artefakte](../../data-factory/continuous-integration-deployment.md#best-practices-for-cicd).

## <a name="troubleshooting-artifacts-deployment"></a>Problembehandlung bei der Bereitstellung von Artefakten 

### <a name="use-the-synapse-workspace-deployment-task"></a>Verwenden der Aufgabe zur Synapse-Arbeitsbereichsbereitstellung

In Synapse gibt es eine Reihe von Artefakten, die keine ARM-Ressourcen sind. Dies ist ein Unterschied zu Azure Data Factory. Die Aufgabe zur Bereitstellung von ARM-Vorlagen kann Synapse-Artefakte nicht ordnungsgemäß bereitstellen.
 
### <a name="unexpected-token-error-in-release"></a>Unerwarteter Tokenfehler in Release

Wenn Ihre Parameterdatei Parameterwerte enthält, die nicht mit Escapezeichen versehen sind, würde beim Analysieren der Datei durch die Releasepipeline der Fehler „Unerwartetes Token“ auftreten. Es wird empfohlen, die Parameter zu überschreiben oder Azure Key Vault außer Kraft zu setzen, um Parameterwerte abzurufen. Als Abhilfe können Sie auch doppelte Escapezeichen verwenden.
