---
title: Informationen zu Azure Policy für Azure Kubernetes Service
description: Erfahren Sie, wie Azure Policy Rego und Open Policy Agent verwendet, um Cluster im Azure Kubernetes Service zu verwalten.
ms.date: 03/27/2020
ms.topic: conceptual
ms.openlocfilehash: d77c5cf94a8239f4617e563961cbe1cc40e48fe0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372661"
---
# <a name="understand-azure-policy-for-azure-kubernetes-service"></a>Grundlegendes zu Azure Policy für Azure Kubernetes Service

Mit der Integration von Azure Policy mit [Azure Kubernetes Service](../../../aks/intro-kubernetes.md) (AKS) können Schutz- und Sicherheitsmaßnahmen in großem Umfang zentral und einheitlich auf Ihre Cluster angewendet werden.
Durch die erweiterte Verwendung von [Gatekeeper](https://github.com/open-policy-agent/gatekeeper) v3, einem _Webhook für die Zugangssteuerung_ für [Open Policy Agent](https://www.openpolicyagent.org/) (OPA), bietet Azure Policy die Möglichkeit, den Konformitätsstatus Ihrer Azure-Ressourcen und AKS-Cluster von einer zentralen Stelle aus zu verwalten und zu melden.

> [!IMPORTANT]
> Azure Policy für AKS befindet sich in der Vorschauversion und unterstützt nur integrierte Richtliniendefinitionen. Integrierte Richtlinien befinden sich in der **Kubernetes**-Kategorie. Der **EnforceRegoPolicy**-Effekt und verwandte **Kubernetes Service**-Kategorierichtlinien werden _eingestellt_. Verwenden Sie stattdessen den aktualisierten [EnforceOPAConstraint](./effects.md#enforceopaconstraint)-Effekt.

> [!WARNING]
> Dieses Feature ist noch nicht in allen Regionen verfügbar. Einen Status zum Rollout finden Sie unter [AKS-Probleme: Breaking Change für Richtlinien-Add-On](https://github.com/Azure/AKS/issues/1529).

## <a name="overview"></a>Übersicht

Führen Sie die folgenden Schritte aus, um Azure Policy für AKS mit Ihrem AKS-Cluster zu aktivieren und zu verwenden:

- [Aktivieren der Previewfunktionen](#opt-in-for-preview)
- [Installieren des Azure Policy-Add-On](#installation-steps)
- [Zuweisen einer Richtliniendefinition für AKS](#built-in-policies)
- [Warten auf die Validierung](#validation-and-reporting-frequency)

## <a name="opt-in-for-preview"></a>Aktivieren der Vorschauversion

Bevor Sie das Azure Policy-Add-On installieren oder eines der Dienstfeatures aktivieren, muss Ihr Abonnement den **Microsoft.ContainerService**- und den **Microsoft.PolicyInsights**-Ressourcenanbieter aktivieren und dann für die Teilnahme an der Vorschau genehmigt werden. Um an der Vorschauversion teilzunehmen, führen Sie diese Schritte entweder im Azure-Portal oder mit der Azure CLI durch:

- Azure-Portal:

  1. Registrieren Sie die Ressourcenanbieter **Microsoft.ContainerService** und **Microsoft.PolicyInsights**. Weitere Informationen finden Sie unter [Ressourcenanbieter und -typen](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

  1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Richtlinie** suchen und die entsprechende Option auswählen.

     ![Suchen nach „Policy“ unter „Alle Dienste“](../media/rego-for-aks/search-policy.png)

  1. Wählen Sie links auf der Seite „Azure Policy“ die Option zum **Beitreten zur Vorschauversion** aus.

     ![Beitreten zur Richtlinie für die AKS-Vorschauversion](../media/rego-for-aks/join-aks-preview.png)

  1. Wählen Sie die Zeile des Abonnements aus, das Sie der Vorschauversion hinzufügen möchten.

  1. Wählen Sie die Schaltfläche **Opt-in** (Abonnieren) oben in der Abonnementliste aus.

- Azure CLI:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Kubernetes Service provider
  az provider register --namespace Microsoft.ContainerService

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Feature register: enables installing the add-on
  az feature register --namespace Microsoft.ContainerService --name AKS-AzurePolicyAutoApprove
  
  # Use the following to confirm the feature has registered
  az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-AzurePolicyAutoApprove')].{Name:name,State:properties.state}"
  
  # Once the above shows 'Registered' run the following to propagate the update
  az provider register -n Microsoft.ContainerService
  
  ```

## <a name="azure-policy-add-on"></a>Azure Policy-Add-On

Das _Azure Policy-Add-On_ für Kubernetes verbindet den Azure Policy-Dienst mit der Gatekeeper-Zugangssteuerung. Das Add-On, das in den Namespace _kube-system_ installiert wird, führt die folgenden Funktionen aus:

- Überprüfen auf Zuweisungen an den Cluster mit dem Azure Policy-Dienst
- Bereitstellen von Richtlinien im Cluster als [Einschränkungsvorlage](https://github.com/open-policy-agent/gatekeeper#constraint-templates) und benutzerdefinierte [Einschränkungsressource](https://github.com/open-policy-agent/gatekeeper#constraints)
- Senden von Details zur Überwachung und Konformität zurück an den Azure Policy-Dienst

### <a name="installing-the-add-on"></a>Installieren des Add-Ons

#### <a name="prerequisites"></a>Voraussetzungen

Bevor Sie das Add-On in Ihrem AKS-Cluster installieren, muss die Vorschauerweiterung installiert sein. Dieser Schritt erfolgt mit der Azure CLI:

1. Wenn Gatekeeper v2-Richtlinien installiert wurden, entfernen Sie das Add-On mit der Schaltfläche **Deaktivieren** in Ihrem AKS-Cluster auf der Seite **Policies (Preview)** (Richtlinien (Vorschau)).

1. Azure CLI-Version 2.0.62 oder höher muss installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

1. Der AKS-Cluster muss die Version _1.14_ oder höher aufweisen. Verwenden Sie das folgende Skript, um Ihre AKS-Clusterversion zu überprüfen:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. Installieren Sie die Version _0.4.0_ der Azure CLI-Vorschauerweiterung für AKS, `aks-preview`:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Install/update the preview extension
   az extension add --name aks-preview

   # Validate the version of the preview extension
   az extension show --name aks-preview --query [version]
   ```

   > [!NOTE]
   > Wenn Sie zuvor die Erweiterung _aks-preview_ installiert haben, installieren Sie alle Updates mit dem Befehl `az extension update --name aks-preview`.

#### <a name="installation-steps"></a>Installationsschritte

Nachdem die Voraussetzungen erfüllt sind, installieren Sie das Azure Policy-Add-On in dem zu verwaltenden AKS-Cluster.

- Azure-Portal

  1. Starten Sie den AKS-Dienst im Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Kubernetes-Dienste** suchen und die entsprechende Option auswählen.

  1. Wählen Sie einen Ihrer AKS-Cluster aus.

  1. Wählen Sie links **Richtlinien (Vorschauversion)** auf der Seite des Kubernetes-Diensts aus.

     ![Richtlinien des AKS-Clusters](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Wählen Sie auf der Hauptseite die Schaltfläche **Add-On aktivieren** aus.

     ![Aktivieren des Azure Policy für AKS-Add-Ons](../media/rego-for-aks/enable-policy-add-on.png)

     > [!NOTE]
     > Wenn die Schaltfläche **Add-On aktivieren** abgeblendet ist, wurde das Abonnement noch nicht zur Vorschauversion hinzugefügt. Die erforderlichen Schritte finden Sie unter [Aktivieren der Vorschauversion](#opt-in-for-preview). Wenn die Schaltfläche **Deaktivieren** verfügbar ist, ist Gatekeeper v2 weiterhin installiert und muss entfernt werden.

- Azure-Befehlszeilenschnittstelle

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="validation-and-reporting-frequency"></a>Häufigkeit der Prüfung und Berichterstellung

Das Add-On prüft alle 15 Minuten mit dem Azure Policy-Dienst, ob sich die Richtlinienzuweisungen geändert haben.
Während dieses Aktualisierungszyklus nimmt das Add-On eine Prüfung auf Änderungen vor. Diese Änderungen lösen das Erstellen, Aktualisieren oder Löschen der Einschränkungsvorlagen und Einschränkungen aus.

> [!NOTE]
> Während ein Clusteradministrator möglicherweise über die Berechtigung zum Erstellen und Aktualisieren von Einschränkungsvorlagen und -ressourcen verfügt, werden diese Szenarios nicht unterstützt, da manuelle Updates überschrieben werden.

Das Add-On fordert alle 15 Minuten einen vollständigen Scan des Clusters an. Nachdem Details des vollständigen Scans und alle Echtzeitauswertungen von versuchten Änderungen am Cluster durch Gatekeeper erfasst wurden, meldet das Add-On die Ergebnisse an den Azure Policy-Dienst zurück, um sie in [Konformitätsdetails](../how-to/get-compliance-data.md#portal) wie bei allen Azure Policy-Zuweisungen aufzunehmen. Während des Prüfzyklus werden nur Ergebnisse für aktive Richtlinienzuweisungen zurückgegeben. Prüfungsergebnisse können auch als [Violations](https://github.com/open-policy-agent/gatekeeper#audit) (Verstöße) angezeigt werden, die im Statusfeld der fehlerhaften Einschränkung aufgeführt sind.

## <a name="policy-language"></a>Richtliniensprache

Die Azure Policy-Sprachstruktur zum Verwalten von Kubernetes folgt derjenigen der bestehenden Richtlinien. Der _EnforceOPAConstraint_-Effekt wird verwendet, um Ihre Kubernetes-Cluster zu verwalten, und er übernimmt details-Eigenschaften, die für die Arbeit mit [OPA Constraint Framework](https://github.com/open-policy-agent/frameworks/tree/master/constraint) und Gatekeeper v3 spezifisch sind. Details und Beispiele finden Sie unter der Auswirkung [EnforceOPAConstraint](./effects.md#enforceopaconstraint).
  
Als Teil der Eigenschaften _details.constraintTemplate_ und _details.constraint_ in der Richtliniendefinition übergibt Azure Policy die URIs dieser [CustomResourceDefinitions](https://github.com/open-policy-agent/gatekeeper#constraint-templates) (CRD) an das Add-On. Rego ist die Sprache, die von OPA und Gatekeeper unterstützt wird, um eine Anforderung an den Kubernetes-Cluster zu validieren. Durch die Unterstützung eines bestehenden Standards für das Kubernetes-Management ermöglicht Azure Policy die Wiederverwendung bestehender Regeln und deren Kombination mit Azure Policy für eine einheitliche Berichterstellungsumgebung zur Cloud-Compliance. Weitere Informationen finden Sie unter [Was ist Rego?](https://www.openpolicyagent.org/docs/latest/policy-language/#what-is-rego)

## <a name="built-in-policies"></a>Integrierte Richtlinien

Führen Sie die folgenden Schritte aus, um die integrierten Richtlinien für die Verwaltung Ihres Clusters über das Azure-Portal zu finden:

1. Starten Sie den Azure Policy-Dienst im Azure-Portal. Klicken Sie im linken Bereich auf „Alle Dienste“, suchen Sie nach **Richtlinie**, und klicken Sie darauf.

1. Wählen Sie im linken Bereich der Seite „Azure Policy“ die Option **Definitionen** aus.

1. Verwenden Sie im Dropdown-Listenfeld „Kategorie“ die Option „Alle auswählen“, um den Filter zu löschen, und klicken Sie dann auf **Kubernetes**.

1. Wählen Sie die Richtliniendefinition und dann die Schaltfläche **Zuweisen** aus.

1. Legen Sie den **Bereich** auf die Verwaltungsgruppe, das Abonnement oder die Ressourcengruppe des Kubernetes-Clusters fest, in dem die Richtlinienzuweisung angewendet wird.

   > [!NOTE]
   > Bei der Zuweisung der Azure-Richtlinie für die AKS-Definition muss der **Bereich** die AKS-Clusterressource umfassen.

1. Weisen Sie der Richtlinienzuweisung einen **Namen** und eine **Beschreibung** zu, die Sie zur einfachen Identifizierung verwenden können.

1. Legen Sie die [Policy enforcement](./assignment-structure.md#enforcement-mode) (Richtlinienerzwingung) auf einen der unten aufgeführten Werte fest.

   - **Aktiviert**: Erzwingen Sie die Richtlinie auf dem Cluster. Kubernetes-Zulassungsanforderungen mit Verstößen werden verweigert.
   
   - **Deaktiviert**: Erzwingen Sie die Richtlinie nicht auf dem Cluster. Kubernetes-Zulassungsanforderungen mit Verstößen werden nicht verweigert. Die Ergebnisse der Konformitätsbewertung sind weiterhin verfügbar. Beim Rollout neuer Richtlinien für das Ausführen von Clustern eignet sich die Option _Deaktiviert_ für das Testen von Richtlinien, da Eintrittsanforderungen mit Verstößen nicht verweigert werden.

1. Wählen Sie **Weiter** aus.

1. Festlegen von **Parameterwerten**
   
   - Geben Sie die Liste der Namespaces im Parameter **Namespaceausschlüsse** an, um Kubernetes-Namespaces aus der Richtlinienauswertung auszuschließen. Es wird empfohlen, Folgendes auszuschließen: _kube-system_.

1. Klicken Sie auf **Überprüfen + erstellen**.

Verwenden Sie alternativ die Schnellstartanleitung [Zuweisen einer Richtlinie – Portal](../assign-policy-portal.md), um eine AKS-Richtlinie zu finden und zuzuweisen. Suchen Sie nach einer Kubernetes-Richtliniendefinition anstelle des Musters „audit vms“.

> [!IMPORTANT]
> Integrierte Richtlinien in der Kategorie **Kubernetes** können nur mit AKS verwendet werden. Eine Liste der integrierten Richtlinien finden Sie unter [Kubernetes-Beispiele](../samples/built-in-policies.md#kubernetes).

## <a name="logging"></a>Protokollierung

### <a name="azure-policy-add-on-logs"></a>Protokolle des Azure Policy-Add-Ons

Als Kubernetes-Controller/-Container erstellen sowohl das Azure Policy-Add-On als auch Gatekeeper Protokolle im AKS-Cluster. Die Protokolle werden auf der Seite **Insights** des AKS-Clusters angezeigt. Weitere Informationen finden Sie unter [Verstehen der Leistung von AKS-Clustern mit Azure Monitor für Container](../../../azure-monitor/insights/container-insights-analyze.md).

## <a name="remove-the-add-on"></a>Entfernen des Add-Ons

Verwenden Sie zum Entfernen des Azure Policy-Add-Ons aus Ihrem AKS-Cluster entweder das Azure-Portal oder die Azure CLI:

- Azure-Portal

  1. Starten Sie den AKS-Dienst im Azure-Portal, indem Sie auf **Alle Dienste** klicken und dann nach **Kubernetes-Dienste** suchen und die entsprechende Option auswählen.

  1. Wählen Sie Ihren AKS-Cluster aus, in dem Sie das Azure Policy-Add-On deaktivieren möchten.

  1. Wählen Sie links **Richtlinien (Vorschauversion)** auf der Seite des Kubernetes-Diensts aus.

     ![Richtlinien des AKS-Clusters](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Wählen Sie auf der Hauptseite die Schaltfläche **Add-On deaktivieren** aus.

     ![Deaktivieren des Azure Policy für AKS-Add-Ons](../media/rego-for-aks/disable-policy-add-on.png)

- Azure-Befehlszeilenschnittstelle

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

## <a name="diagnostic-data-collected-by-azure-policy-add-on"></a>Vom Azure Policy-Add-On gesammelte Diagnosedaten

Das Azure Policy-Add-On für Kubernetes sammelt begrenzte Clusterdiagnosedaten. Diese Diagnosedaten sind wichtige technische Daten in Bezug auf Software und Leistung. Sie werden für folgende Zwecke verwendet:

- Azure Policy Add-On auf dem neuesten Stand halten
- Azure Policy Add-On sicher, zuverlässig und leistungsfähig halten
- Azure Policy Add-On verbessern – durch die aggregierte Analyse der Verwendung des Add-Ons

Die vom Add-On gesammelten Informationen sind keine persönlichen Daten. Die folgenden Details werden derzeit gesammelt:

- Version des Azure Policy-Add-On-Agents
- Clustertyp
- Clusterregion
- Clusterressourcengruppe
- Clusterressourcen-ID
- Clusterabonnement-ID
- Clusterbetriebssystem (Beispiel: Linux)
- Ort für den Cluster (Beispiel: Seattle)
- Bundesland oder Kanton für den Cluster (Beispiel: Washington)
- Land oder Region für den Cluster (Beispiel: USA)
- Ausnahmen/Fehler, die während der Installation des Agents bei der Richtlinienauswertung durch das Azure Policy Add-On festgestellt werden
- Anzahl von Gatekeeper-Richtlinien, die nicht vom Azure Policy Add-On installiert werden

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Befassen Sie sich mit der [Struktur von Azure Policy-Definitionen](definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](../how-to/programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](../how-to/get-compliance-data.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](../how-to/remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).
