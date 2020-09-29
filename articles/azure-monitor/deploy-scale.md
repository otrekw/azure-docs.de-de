---
title: Bedarfsorientiertes Bereitstellen von Azure Monitor mithilfe von Azure Policy
description: Bedarfsorientiertes Bereitstellen der Features von Azure Monitor mithilfe von Azure Policy.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/08/2020
ms.openlocfilehash: a69a58da85cf1ee03046626bb076c5cd44196279
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87828709"
---
# <a name="deploy-azure-monitor-at-scale-using-azure-policy"></a>Bedarfsorientiertes Bereitstellen von Azure Monitor mithilfe von Azure Policy
Während einige Features von Azure Monitor einmalig oder nur für eine begrenzte Zahl von Malen konfiguriert werden, muss dies bei anderen für jede Ressource wiederholt werden, die Sie überwachen möchten. In diesem Artikel werden Methoden beschrieben, wie Sie Azure Monitor mit Azure Policy bedarfsorientiert implementieren, um sicherzustellen, dass die Überwachung für alle Ihre Azure-Ressourcen konsistent und genau konfiguriert ist.

Beispielsweise müssen Sie eine Diagnoseeinstellung für alle vorhandenen Azure-Ressourcen und für jede von Ihnen neu erstellte Ressource erstellen. Sie müssen auch jedes Mal, wenn Sie einen virtuellen Computer erstellen, einen Agent installieren und konfigurieren. Sie könnten Methoden wie PowerShell oder die CLI verwenden, um diese Aktionen auszuführen, da diese Methoden für alle Features von Azure Monitor verfügbar sind. Wenn Sie Azure Policy verwenden, kann programmgesteuert automatisch bei jeder Erstellung oder Änderung einer Ressource die entsprechende Konfiguration ausgeführt werden.


## <a name="azure-policy"></a>Azure Policy
In diesem Abschnitt finden Sie eine kurze Einführung in [Azure Policy](../governance/policy/overview.md), mit der Sie mit minimalem Aufwand Organisationsstandards in Ihrem gesamten Azure-Abonnement oder einer Verwaltungsgruppe bewerten und erzwingen können. Weitere Informationen finden Sie in der [Dokumentation zu Azure Policy](../governance/policy/overview.md).

Mit Azure Policy können Sie Konfigurationsanforderungen für alle erstellten Ressourcen angeben und Ressourcen identifizieren, die nicht kompatibel sind, die Erstellung der Ressourcen blockieren oder die erforderliche Konfiguration hinzufügen. Dies funktioniert durch das Abfangen von Aufrufen, eine neue Ressource zu erstellen oder eine vorhandene Ressource zu ändern. Als Reaktion kommen Effekte wie Ablehnung der Anforderung, wenn sie nicht mit den in einer Richtliniendefinition erwarteten Eigenschaften übereinstimmt, Kennzeichnung als nicht kompatibel oder Bereitstellen einer zugehörigen Ressource infrage. Sie können vorhandene Ressourcen mit einer **deployIfNotExists**- oder **modify**-Richtliniendefinition korrigieren.

Azure Policy umfasst die Objekte in der folgenden Tabelle: Eine ausführlichere Erläuterung der Objekte finden Sie unter [Azure Policy-Objekte](../governance/policy/overview.md#azure-policy-objects).

| Element | BESCHREIBUNG |
|:---|:---|
| Richtliniendefinition | Beschreibt Bedingungen für die Ressourcencompliance und die Maßnahmen, die ergriffen werden, wenn eine Bedingung erfüllt ist. Dabei kann es sich um alle Ressourcen eines bestimmten Typs oder nur um Ressourcen handeln, die bestimmten Eigenschaften entsprechen. Die Auswirkung kann darin bestehen, dass die Ressource einfach als konform markiert oder eine zugehörige Ressource bereitgestellt wird. Richtliniendefinitionen werden mithilfe von JSON geschrieben, wie in [Struktur von Azure Policy-Definitionen](../governance/policy/concepts/definition-structure.md) beschrieben. Die Auswirkungen werden in [Grundlegendes zu Azure Policy-Auswirkungen](../governance/policy/concepts/effects.md) beschrieben.
| Richtlinieninitiative | Eine Gruppe von Richtliniendefinitionen, die gleichzeitig angewendet werden sollten. Beispielsweise können Sie eine Richtliniendefinition einsetzen, um Ressourcenprotokolle an einen Log Analytics-Arbeitsbereich zu senden, und eine andere, um Ressourcenprotokolle an Event Hubs zu senden. Erstellen Sie eine Initiative, die beide Richtliniendefinitionen umfasst, und wenden Sie die Initiative anstelle der einzelnen Richtliniendefinitionen auf Ressourcen an. Initiativen werden mithilfe von JSON geschrieben, wie in [Struktur von Azure Policy-Initiativendefinitionen](../governance/policy/concepts/initiative-definition-structure.md) beschrieben. |
| Zuweisung | Eine Richtliniendefinition oder -initiative tritt erst in Kraft, wenn sie einem Bereich zugewiesen wird. Weisen Sie z. B. einer Ressourcengruppe eine Richtlinie zu, um sie auf alle Ressourcen anzuwenden, die in dieser Ressource erstellt werden, oder wenden Sie sie auf ein Abonnement an, um sie auf alle Ressourcen in diesem Abonnement anzuwenden.  Weitere Informationen finden Sie unter [Azure Policy-Zuweisungsstruktur](../governance/policy/concepts/assignment-structure.md). |

## <a name="built-in-policy-definitions-for-azure-monitor"></a>Integrierte Richtliniendefinitionen für Azure Monitor
Azure Policy enthält mehrere vordefinierte Definitionen im Zusammenhang mit Azure Monitor. Sie können diese Richtliniendefinitionen Ihrem vorhandenen Abonnement zuweisen oder sie als Grundlage verwenden, um eigene benutzerdefinierte Definitionen zu erstellen. Eine umfassende Liste der integrierten Richtlinien in der Kategorie **Überwachung** finden Sie unter [Integrierte Azure Policy-Definitionen für Azure Monitor](samples/policy-samples.md).

Führen Sie die folgenden Schritte aus, um die in Bezug auf die Überwachung integrierten Richtliniendefinitionen anzuzeigen:

1. Wechseln Sie im Azure-Portal zu **Azure Policy**.
2. Wählen Sie **Definitionen** aus.
3. Wählen Sie für **Typ** die Option *Integriert* und für **Kategorie** die Option *Überwachung* aus.

  ![Integrierte Richtliniendefinitionen](media/deploy-scale/builtin-policies.png)


## <a name="diagnostic-settings"></a>Diagnoseeinstellungen
[Diagnoseeinstellungen](platform/diagnostic-settings.md) sammeln Ressourcenprotokolle und Metriken von Azure-Ressourcen an mehreren Speicherorten, in der Regel in einem Log Analytics-Arbeitsbereich, der Ihnen ermöglicht, die Daten mit [Protokollabfragen](log-query/log-query-overview.md) und [Protokollwarnungen](platform/alerts-log.md) zu analysieren. Verwenden Sie Policy, um automatisch jedes Mal, wenn Sie eine Ressource erstellen, eine Diagnoseeinstellung zu erstellen.

Jeder Azure-Ressourcentyp verfügt über einen eindeutigen Satz von Kategorien, die in der Diagnoseeinstellung aufgelistet werden müssen. Daher ist für jeden Ressourcentyp eine separate Richtliniendefinition erforderlich. Einige Ressourcentypen verfügen über integrierte Richtliniendefinitionen, die Sie ohne Änderungen zuweisen können. Bei anderen Ressourcentypen müssen Sie eine benutzerdefinierte Definition erstellen.

### <a name="built-in-policy-definitions-for-azure-monitor"></a>Integrierte Richtliniendefinitionen für Azure Monitor
Es gibt zwei integrierte Richtliniendefinitionen für jeden Ressourcentyp, eine zum Senden an den Log Analytics-Arbeitsbereich und eine andere zum Senden an einen Event Hub. Wenn Sie nur einen Speicherort benötigen, weisen Sie diese Richtlinie für den Ressourcentyp zu. Wenn Sie beide benötigen, weisen Sie beiden Richtliniendefinitionen für die Ressource zu.

Die folgende Abbildung zeigt z. B. die integrierten Richtliniendefinitionen für die Diagnoseeinstellung für Data Lake Analytics.

  ![Integrierte Richtliniendefinitionen](media/deploy-scale/builtin-diagnostic-settings.png)

### <a name="custom-policy-definitions"></a>Benutzerdefinierte Richtliniendefinitionen
Für Ressourcentypen, die über keine integrierte Richtlinie verfügen, müssen Sie eine benutzerdefinierte Richtliniendefinition erstellen. Sie können hierzu manuell im Azure-Portal eine vorhandene integrierte Richtlinie kopieren und dann für Ihren Ressourcentyp ändern. Es ist jedoch effizienter, die Richtlinie programmgesteuert mithilfe eines Skripts im PowerShell-Katalog zu erstellen.

Das Skript [Create-AzDiagPolicy](https://www.powershellgallery.com/packages/Create-AzDiagPolicy) erstellt Richtliniendateien für einen bestimmten Ressourcentyp, die Sie mithilfe von PowerShell oder der CLI installieren können. Erstellen Sie eine benutzerdefinierte Richtliniendefinition für Diagnoseeinstellungen mit folgendem Verfahren.


1. Vergewissern Sie sich, dass [Azure PowerShell](/powershell/azure/install-az-ps) installiert ist.
2. Installieren Sie das Skript mit folgendem Befehl:
  
    ```azurepowershell
    Install-Script -Name Create-AzDiagPolicy
    ```

3. Führen Sie das Skript mit den Parametern aus, die angeben, wohin die Protokolle gesendet werden sollen. Sie werden aufgefordert, ein Abonnement und einen Ressourcentyp anzugeben. Wenn Sie z. B. eine Richtliniendefinition erstellen möchten, die an einen Log Analytics-Arbeitsbereich und einen Event Hub sendet, verwenden Sie den folgenden Befehl.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

4. Alternativ können Sie ein Abonnement und einen Ressourcentyp im Befehl angeben. Wenn Sie z. B. eine Richtliniendefinition erstellen möchten, die an einen Log Analytics-Arbeitsbereich und einen Event Hub für Azure SQL Server-Datenbanken sendet, verwenden Sie den folgenden Befehl.

   ```azurepowershell
   Create-AzDiagPolicy.ps1 -SubscriptionID xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -ResourceType Microsoft.Sql/servers/databases  -ExportLA -ExportEH -ExportDir ".\PolicyFiles"  
   ```

5. Das Skript erstellt separate Ordner für die einzelnen Richtliniendefinition, die jeweils die drei Dateien azurepolicy.json, azurepolicy.rules.json und azurepolicy.parameters.json enthalten. Wenn Sie die Richtlinie manuell im Azure-Portal erstellen möchten, können Sie den Inhalt von „azurepolicy.json“ kopieren und einfügen, da er die gesamte Richtliniendefinition enthält. Verwenden Sie die anderen beiden Dateien mit PowerShell oder der CLI, um die Richtliniendefinition über eine Befehlszeile zu erstellen.

    In den folgenden Beispielen wird gezeigt, wie die Richtliniendefinition sowohl über PowerShell als auch über die CLI installiert wird. Beide enthalten Metadaten zum Angeben einer Kategorie zur **Überwachung**, um die neue Richtliniendefinition mit den integrierten Richtliniendefinitionen zu gruppieren.

      ```azurepowershell
      New-AzPolicyDefinition -name "Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace" -policy .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json -parameter .\Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json -mode All -Metadata '{"category":"Monitoring"}'
      ```

      ```azurecli
      az policy definition create --name 'deploy-diag-setting-sql-database--workspace' --display-name 'Deploy Diagnostic Settings for SQL Server database to Log Analytics workspace'  --rules 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.rules.json' --params 'Apply-Diag-Settings-LA-Microsoft.Sql-servers-databases\azurepolicy.parameters.json' --subscription 'AzureMonitor_Docs' --mode All
      ```

### <a name="initiative"></a>Initiative
Anstatt für jede Richtliniendefinition eine Zuweisung zu erstellen, ist es eine gängige Strategie, eine Initiative zu erstellen, die die Richtliniendefinitionen umfasst, um Diagnoseeinstellungen für jeden Azure-Dienst zu erstellen. Erstellen Sie abhängig von der Art der Verwaltung Ihrer Umgebung eine Zuweisung zwischen der Initiative und einer Verwaltungsgruppe, einem Abonnement oder einer Ressourcengruppe. Diese Strategie bietet folgende Vorteile:

- Erstellen Sie eine einzelne Zuweisung für die Initiative anstelle mehrerer Zuweisungen für jeden Ressourcentyp. Verwenden Sie dieselbe Initiative für mehrere Überwachungsgruppen, Abonnements oder Ressourcengruppen.
- Ändern Sie die Initiative, wenn Sie einen neuen Ressourcentyp oder ein neues Ziel hinzufügen müssen. Beispielsweise können die ersten Anforderungen darin bestehen, Daten nur an einen Log Analytics-Arbeitsbereich zu senden, aber später möchten Sie Event Hub hinzufügen. Ändern Sie die Initiative, anstatt neue Zuweisungen zu erstellen.

Ausführliche Informationen zum Erstellen einer Initiative finden Sie unter [Erstellen und Zuweisen einer Initiativdefinition](../governance/policy/tutorials/create-and-manage.md#create-and-assign-an-initiative-definition). Beachten Sie die folgenden Empfehlungen:

- Legen Sie die **Kategorie** auf **Überwachung** fest, um sie mit zugehörigen integrierten und benutzerdefinierten Richtliniendefinitionen zu gruppieren.
- Anstatt die Details für den Log Analytics-Arbeitsbereich und den Event Hub für die Richtliniendefinition in der Initiative anzugeben, verwenden Sie einen allgemeinen Initiativparameter. So können Sie auf einfache Weise einen allgemeinen Wert für alle Richtliniendefinitionen angeben und diesen Wert bei Bedarf ändern.

![Initiativdefinition](media/deploy-scale/initiative-definition.png)

### <a name="assignment"></a>Zuweisung 
Weisen Sie die Initiative abhängig vom Umfang ihrer zu überwachenden Ressourcen einer Azure-Verwaltungsgruppe, einem Abonnement oder einer Ressourcengruppe zu. Eine [Verwaltungsgruppe](../governance/management-groups/overview.md) ist besonders nützlich für bereichsbezogene Richtlinien, insbesondere wenn Ihre Organisation über mehrere Abonnements verfügt.

![Initiativzuweisung](media/deploy-scale/initiative-assignment.png)

Mithilfe von Initiativparametern können Sie den Arbeitsbereich oder andere Details einmal für alle Richtliniendefinitionen in der Initiative angeben. 

![Initiativparameter](media/deploy-scale/initiative-parameters.png)

### <a name="remediation"></a>Wiederherstellung
Die Initiative gilt für jeden virtuellen Computer, während er erstellt wird. Mit einem [Wartungstask](../governance/policy/how-to/remediate-resources.md) werden die Richtliniendefinitionen in der Initiative für vorhandene Ressourcen bereitgestellt, sodass Sie Diagnoseeinstellungen für alle Ressourcen erstellen können, die bereits erstellt wurden. Wenn Sie die Zuweisung mithilfe des Azure-Portals erstellen, können Sie gleichzeitig einen Wartungstask erstellen. Weitere Informationen zur Wartung finden Sie unter [Korrigieren nicht konformer Ressourcen mit Azure Policy](../governance/policy/how-to/remediate-resources.md).

![Initiativwartung](media/deploy-scale/initiative-remediation.png)


## <a name="azure-monitor-for-vms"></a>Azure Monitor für VMs
[Azure Monitor für VMs](insights/vminsights-overview.md) ist das primäre Tool in Azure Monitor zur VM-Überwachung. Beim Aktivieren von Azure Monitor für VMs werden sowohl der Log Analytics-Agent als auch der Dependency-Agent installiert. Anstatt diese Aufgaben manuell auszuführen, verwenden Sie Azure Policy, um sicherzustellen, dass jeder virtuelle Computer bei der Erstellung konfiguriert wird.

> [!NOTE]
> Azure Monitor für VMs umfasst die Funktion **Richtlinienabdeckung in Azure Monitor für VMs**, mit der Sie nicht richtlinienkonforme VMs in Ihrer Umgebung ermitteln und die jeweils vorliegenden Probleme behandeln können. Sie können diese Funktion verwenden, anstatt direkt mit Azure Policy für Azure-VMs zu arbeiten. Außerdem eignet sie sich für Hybrid-VMs, die mit Azure Arc verbunden sind. Für Azure-VM-Skalierungsgruppen müssen Sie die Zuweisung mithilfe von Azure Policy erstellen.
 

Azure Monitor für VMs umfasst die folgenden integrierten Initiativen, mit denen beide Agents installiert werden, um eine umfassende Überwachung zu ermöglichen. 

|Name |BESCHREIBUNG |
|:---|:---|
|Aktivieren von Azure Monitor für VMs | Installiert den Log Analytics-Agent und den Dependency-Agent auf Azure-VMs und Hybrid-VMs, die mit Azure Arc verbunden sind. |
|Aktivieren von Azure Monitor für VM-Skalierungsgruppen | Installiert den Log Analytics-Agent und den Dependency-Agent in der Azure-VM-Skalierungsgruppe. |


### <a name="virtual-machines"></a>Virtuelle Computer
Anstatt Zuweisungen für diese Initiativen mithilfe der Azure Policy-Schnittstelle zu erstellen, enthält Azure Monitor für VMs eine Funktion, mit der Sie die Anzahl der VMs in den einzelnen Bereichen überprüfen können, um zu bestimmen, ob die Initiative angewendet wurde. Anschließend können Sie den Arbeitsbereich konfigurieren und alle erforderlichen Zuweisungen mithilfe dieser Schnittstelle erstellen.

Weitere Informationen hierzu finden Sie unter [Aktivieren von Azure Monitor für VMs mit Azure Policy](./insights/vminsights-enable-policy.md).

![Richtlinie für Azure Monitor für VMs](media/deploy-scale/vminsights-policy.png)

### <a name="virtual-machine-scale-sets"></a>VM-Skalierungsgruppen
Wenn Sie Azure Policy zum Aktivieren der Überwachung von VM-Skalierungsgruppen verwenden möchten, weisen Sie die Initiative **Azure Monitor für Virtual Machine Scale Sets aktivieren** einer Azure-Verwaltungsgruppe, einem Abonnement oder einer Ressource zu (abhängig vom Umfang der Ressourcen, die überwacht werden sollen). Eine [Verwaltungsgruppe](../governance/management-groups/overview.md) ist besonders nützlich für bereichsbezogene Richtlinien, insbesondere wenn Ihre Organisation über mehrere Abonnements verfügt.

![Initiativzuweisung](media/deploy-scale/virtual-machine-scale-set-assign-initiative.png)

Wählen Sie den Arbeitsbereich aus, an den die Daten gesendet werden. In diesem Arbeitsbereich muss die *VMInsights*-Lösung installiert sein, wie unter []() beschrieben.

![Arbeitsbereich auswählen](media/deploy-scale/virtual-machine-scale-set-workspace.png)

Erstellen Sie einen Wartungstask, wenn Sie über eine vorhandene VM-Skalierungsgruppe verfügen, der diese Richtlinie zugewiesen werden muss.

![Wartungstask](media/deploy-scale/virtual-machine-scale-set-remediation.png)

### <a name="log-analytics-agent"></a>Log Analytics-Agent
Möglicherweise gibt es Szenarien, in denen zwar der Log Analytics-Agent, jedoch nicht der Dependency-Agent installiert werden soll. Es ist keine integrierte Initiative vorhanden, die nur für diesen einen Agent verwendet werden kann. Sie können jedoch eine eigene Initiative erstellen, die auf den integrierten Richtliniendefinitionen basiert, die von Azure Monitor für VMs bereitgestellt werden.

> [!NOTE]
> Da der Dependency-Agent den Log Analytics-Agent benötigt, um Daten an Azure Monitor zu übermitteln, ist die alleinige Bereitstellung des Dependency-Agents nicht sinnvoll.


|Name |BESCHREIBUNG |
|-----|------------|
|Überwachen der Bereitstellung des Log Analytics-Agents – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VMs als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
|Bereitstellen des Log Analytics-Agents für Linux-VMs |Hiermit stellen Sie den Log Analytics-Agent für Linux-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
|Bereitstellen des Log Analytics-Agents für Windows-VMs |Hiermit stellen Sie den Log Analytics-Agent für Windows-VMs bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
| [Vorschau]: Log Analytics agent should be installed on your Linux Azure Arc machines (Log Analytics-Agent muss auf Ihren Linux-basierten Azure Arc-Computern installiert sein).\ |Bei dieser Richtlinie werden Azure Arc-Hybridcomputer, bei denen es sich um Linux-VMs handelt, als nicht konform gemeldet, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
| [Vorschau]: Log Analytics agent should be installed on your Windows Azure Arc machines (Log Analytics-Agent muss auf Ihren Windows-basierten Azure Arc-Computern installiert sein)\ |Bei dieser Richtlinie werden Azure Arc-Hybridcomputer, bei denen es sich um Windows-VMs handelt, als nicht konform gemeldet, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
| [Vorschau]: Deploy Log Analytics agent to Linux Azure Arc machines (Log Analytics-Agent für Linux-basierte Azure Arc-Computer bereitstellen)\ |Hiermit stellen Sie den Log Analytics-Agent für Linux-basierte Azure Arc-Hybridcomputer bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
| [Vorschau]: Deploy Log Analytics agent to Windows Azure Arc machines (Log Analytics-Agent für Windows-basierte Azure Arc-Computer bereitstellen)\ |Hiermit stellen Sie den Log Analytics-Agent für Windows-basierte Azure Arc-Hybridcomputer bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
|Überwachen der Bereitstellung des Dependency-Agents in VM-Skalierungsgruppen – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VM-Skalierungsgruppen als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
|Überwachen der Bereitstellung des Log Analytics-Agents in VM-Skalierungsgruppen – VM-Image (Betriebssystem) nicht aufgelistet |Meldet VM-Skalierungsgruppen als nicht konform, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
|Bereitstellen des Log Analytics-Agents für Linux-VM-Skalierungsgruppen |Hiermit stellen Sie den Log Analytics-Agent für Linux-VM-Skalierungsgruppen bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |
|Bereitstellen des Log Analytics-Agents für Windows-VM-Skalierungsgruppen |Hiermit stellen Sie den Log Analytics-Agent für Windows-VM-Skalierungsgruppen bereit, wenn das VM-Image (Betriebssystem) nicht in der Liste definiert und der Agent nicht installiert ist. |


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure Policy](../governance/policy/overview.md).
- Erfahren Sie mehr über [Diagnoseeinstellungen](platform/diagnostic-settings.md).