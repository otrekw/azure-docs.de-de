---
title: Sichern von Pods mit Azure Policy in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie Pods mit Azure Policy in Azure Kubernetes Service (AKS) sichern.
services: container-service
ms.topic: article
ms.date: 07/06/2020
ms.openlocfilehash: 8a5107b9ba3c05c92a06753b2cb30bcfc2896d91
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86090828"
---
# <a name="secure-pods-with-azure-policy-preview"></a>Sichern von Pods mit Azure Policy (Vorschau)

Zum Erhöhen der Sicherheit Ihres AKS-Clusters können Sie steuern, welche Funktionen den Pods gewährt werden und ob etwas nicht der Unternehmensrichtlinie entspricht. Dieser Zugriff wird durch integrierte Richtlinien definiert, die durch das [Azure Policy-Add-On für AKS][kubernetes-policy-reference] bereitgestellt werden. Durch die Bereitstellung einer zusätzlichen Kontrolle über die Sicherheitsaspekte der Spezifikation Ihres Pods, z. B. Stammberechtigungen, wird eine strengere Einhaltung der Sicherheitsvorschriften und ein besserer Einblick in die in Ihrem Cluster bereitgestellten Systeme ermöglicht. Wenn ein Pod die in der Richtlinie festgelegten Bedingungen nicht erfüllt, kann Azure Policy dem Pod das Starten untersagen oder einen Verstoß kennzeichnen. In diesem Artikel erfahren Sie, wie Sie Azure Policy verwenden können, um die Bereitstellung von Pods in AKS einzuschränken.

> [!IMPORTANT]
> AKS-Previewfunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. AKS-Vorschauen werden teilweise vom Kundensupport auf der Grundlage der bestmöglichen Leistung abgedeckt. Daher sind diese Funktionen nicht für die Verwendung in der Produktion vorgesehen. Weitere Informationen hierzu finden Sie in den folgenden Supportartikeln:
>
> * [Unterstützungsrichtlinien für Azure Kubernetes Service][aks-support-policies]
> * [Häufig gestellte Fragen zum Azure-Support][aks-faq]

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

### <a name="install-the-azure-policy-add-on-for-aks"></a>Installieren des Azure Policy-Add-Ons für AKS

Um AKS-Pods durch Azure Policy zu schützen, müssen Sie das Azure Policy-Add-On für AKS auf einem AKS-Cluster installieren. Befolgen Sie diese [Schritte zum Installieren des Azure Policy-Add-Ons](../governance/policy/concepts/policy-for-kubernetes.md#install-azure-policy-add-on-for-aks).

In diesem Dokument wird davon ausgegangen, dass Sie über Folgendes verfügen, das in der oben verknüpften exemplarischen Vorgehensweise bereitgestellt wird.

* Die Ressourcenanbieter `Microsoft.ContainerService` und `Microsoft.PolicyInsights` müssen mit `az provider register` registriert sein.
* Das Featureflag `AKS-AzurePolicyAutoApprove` (Vorschau) muss mit `az feature register` registriert sein.
* Die Azure-Befehlszeilenschnittstelle muss mit der `aks-preview`-Erweiterung, Version 0.4.53 oder höher, installiert sein.
* Ein AKS-Cluster unter der unterstützten Version 1.15 oder höher, der mit dem Azure Policy-Add-On installiert ist.

## <a name="overview-of-securing-pods-with-azure-policy-for-aks-preview"></a>Übersicht über das Sichern von Pods mit Azure Policy für AKS (Vorschau)

>[!NOTE]
> In diesem Dokument wird ausführlich beschrieben, wie Azure Policy zum Sichern von Pods verwendet werden kann. Hierbei handelt es sich um den Nachfolger des [Features für Kubernetes-Podsicherheitsrichtlinien in der Vorschauversion](use-pod-security-policies.md).
> **Die Podsicherheitsrichtlinie (Vorschau) und das Azure Policy-Add-On für AKS können nicht gleichzeitig aktiviert werden.**
> 
> Wenn das Azure Policy-Add-On in einem Cluster mit aktivierter Podsicherheitsrichtlinie installiert wird, [folgen Sie diesen Schritten, um die Podsicherheitsrichtlinie zu deaktivieren](use-pod-security-policies.md#enable-pod-security-policy-on-an-aks-cluster).

In einem AKS-Cluster wird eine Zugangssteuerung verwendet, um an den API-Server gerichtete Anforderungen abzufangen, wenn eine Ressource erstellt und aktualisiert werden soll. Der Zugangscontroller kann dann die Ressourcenanforderung anhand einer Reihe von Regeln *überprüfen*, um zu ermitteln, ob sie erstellt werden soll.

Zuvor wurde das Feature [Podsicherheitsrichtlinie (Vorschau)](use-pod-security-policies.md) über das Kubernetes-Projekt aktiviert, um einzuschränken, welche Pods bereitgestellt werden können. Dieses Feature befindet sich nicht mehr in der aktiven Entwicklung des Kubernetes-Projekts.

Durch die Verwendung des Azure Policy-Add-Ons kann ein AKS-Cluster integrierte Azure-Richtlinien verwenden, die Pods und andere Kubernetes-Ressourcen ähnlich der bisherigen Podsicherheitsrichtlinie sichern. Das Azure Policy-Add-On für AKS installiert eine verwaltete Instanz von [Gatekeeper](https://github.com/open-policy-agent/gatekeeper), einem überprüfenden Zugangscontroller. Azure Policy für Kubernetes baut auf dem Open Policy Agent (Open-Source) auf, der auf der [Rego-Richtliniensprache](../governance/policy/concepts/policy-for-kubernetes.md#policy-language) beruht.

In diesem Dokument wird ausführlich beschrieben, wie Sie mit Azure Policy die Pods in einem AKS-Cluster sichern können. Zudem enthält es eine Anleitung zur Migration von Podsicherheitsrichtlinien (Vorschau).

## <a name="limitations"></a>Einschränkungen

* Während der Vorschau können maximal 200 Pods mit 20 Azure Policy für Kubernetes-Richtlinien in einem einzelnen Cluster ausgeführt werden.
* [Einige Systemnamespaces](#namespace-exclusion), die mit AKS verwaltete Pods enthalten, werden aus der Richtlinienauswertung ausgeschlossen.
* Windows-Pods [bieten keine Unterstützung für Sicherheitskontexte](https://kubernetes.io/docs/concepts/security/pod-security-standards/#what-profiles-should-i-apply-to-my-windows-pods), daher gelten viele Azure-Richtlinien nur für Linux-Pods, z. B. das Unterbinden von Stammberechtigungen, die in Windows-Pods nicht eskaliert werden können.
* Die Podsicherheitsrichtlinie und das Azure Policy-Add-On für AKS können nicht beide aktiviert sein. Wenn Sie das Azure Policy-Add-On in einem Cluster mit aktivierter Podsicherheitsrichtlinie installieren, deaktivieren Sie die Podsicherheitsrichtlinie mit den [folgenden Anweisungen](use-pod-security-policies.md#enable-pod-security-policy-on-an-aks-cluster).

## <a name="azure-policies-to-secure-kubernetes-pods"></a>Azure-Richtlinien zum Sichern von Kubernetes-Pods

Nach der Installation des Azure Policy-Add-Ons werden standardmäßig keine Richtlinien angewendet.

Es gibt vierzehn (14) integrierte individuelle Azure-Richtlinien und zwei (2) integrierte Initiativen, die speziell Pods in einem AKS-Cluster sichern.
Jede Richtlinie kann mit einer Auswirkung angepasst werden. Eine vollständige Liste der [AKS-Richtlinien und ihrer unterstützten Auswirkungen ist hier aufgeführt][policy-samples]. Erfahren Sie mehr über die [Azure Policy-Auswirkungen](../governance/policy/concepts/effects.md).

Azure-Richtlinien können auf Verwaltungsgruppenebene, Abonnement- oder Ressourcengruppenebene angewendet werden. Stellen Sie bei der Zuweisung einer Richtlinie auf Ressourcengruppenebene sicher, dass die Ressourcengruppe des AKS-Zielclusters innerhalb des Gültigkeitsbereichs der Richtlinie ausgewählt wird. Jeder Cluster im zugewiesenen Bereich mit installiertem Azure Policy-Add-On befindet sich im Gültigkeitsbereich der Richtlinie.

Wenn Sie die [Podsicherheitsrichtlinie (Vorschau)](use-pod-security-policies.md) verwenden, erfahren Sie, wie Sie zu [Azure Policy migrieren und erhalten Informationen zu anderen Verhaltensunterschieden](#migrate-from-kubernetes-pod-security-policy-to-azure-policy).

### <a name="built-in-policy-initiatives"></a>Integrierte Richtlinieninitiativen

Eine Initiative in Azure Policy ist eine Auflistung von Richtliniendefinitionen, die auf das Erreichen eines einzigen übergeordneten Ziels ausgerichtet sind. Die Verwendung von Initiativen kann die Verwaltung und Zuweisung von Richtlinien über AKS-Cluster hinweg vereinfachen. Eine Initiative existiert als einzelnes Objekt. Weitere Informationen erhalten Sie unter [Azure Policy-Initiativen](../governance/policy/overview.md#initiative-definition).

Azure Policy für Kubernetes bietet zwei integrierte Initiativen zur Sicherung von Pods, [Baseline](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d) und [Eingeschränkt](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00).

Beide integrierten Initiativen bauen auf Definitionen auf, die in [Podsicherheitsrichtlinie aus Kubernetes](https://github.com/kubernetes/website/blob/master/content/en/examples/policy/baseline-psp.yaml) verwendet werden.

|[Steuerung der Podsicherheitsrichtlinie](https://kubernetes.io/docs/concepts/policy/pod-security-policy/#what-is-a-pod-security-policy)| Link zur Azure Policy-Definition| Baseline-Initiative | Eingeschränkte Initiative |
|---|---|---|---|
|Unterbinden der Ausführung privilegierter Container|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F95edb821-ddaf-4404-9732-666045e056b4)| Ja | Ja
|Unterbinden der gemeinsamen Verwendung von Hostnamespaces|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F47a1ee2f-2a2a-4576-bf2a-e0e36709c2b8)| Ja | Ja
|Einschränken der Verwendung von Hostnetzwerken und Ports auf eine bekannte Liste|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82985f06-dc18-4a48-bc1c-b9f4f0098cfe)| Ja | Ja
|Einschränken der Verwendung des Hostdateisystems|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F098fc59e-46c7-4d99-9b16-64990e543d75)| Ja | Ja
|Hinzufügen von Linux-Funktionen, die über den [Standardsatz](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) hinausgehen|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc26596ff-4d70-4e6a-9a30-c2506bd2f80c) | Ja | Ja
|Einschränken der Verwendung definierter Volumetypen|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F16697877-1118-4fb1-9b65-9898ec2509ec)| - | Ja
|Rechteausweitung auf den Stamm|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1c6e92c9-99f0-4e55-9cf2-0c234dc48f99) | - | Ja |
|Einschränken der Benutzer- und Gruppen-IDs des Containers|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff06ddb64-5fa3-4b77-b166-acb36f7f6042) | - | Ja |
|Einschränken der Zuordnung einer FSGroup, die Besitzer der Podvolumes ist|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff06ddb64-5fa3-4b77-b166-acb36f7f6042) | - | Ja |
|Erfordert die Verwendung des seccomp-Profils|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F975ce327-682c-4f2e-aa46-b9598289b86c) | - | - |
|Einschränken des von Containern verwendeten sysctl-Profils|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F56d0a13f-712f-466b-8416-56fb354fb823) | - | - |
|Definition von Proc Mount-Standardtypen zum Reduzieren der Angriffsfläche|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff85eb0dd-92ee-40e9-8a76-db25a507d6d3) | - | - |
|Einschränken auf bestimmte FlexVolume-Treiber|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4a8fce0-2dd5-4c21-9a36-8f0ec809d663) | - | - |
|Zulassen von nicht schreibgeschützten Einbindungen|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdf49d893-a74c-421d-bc95-c663042e5b80) | - | - |
|Definieren des benutzerdefinierten SELinux-Kontexts eines Containers|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe1e6c427-07d9-46ab-9689-bfa85431e636) | - | - |
|Definieren des von Containern verwendeten AppArmor-Profils|[Öffentliche Cloud](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F511f5417-5d12-434d-ab2e-816901e72a5e) | - | - |

<!---
# Removing until custom initiatives are supported the week after preview

#### Custom initiative

If the built-in initiatives to address pod security do not match your requirements, you can choose your own policies to exist in a custom initiative. Read more about [building custom initatives in Azure Policy](../governance/policy/tutorials/create-and-manage#create-and-assign-an-initiative-definition).

--->

### <a name="namespace-exclusion"></a>Namespaceausschluss

> [!WARNING]
> Pods in Administratornamespaces wie „kube-system“ müssen ausgeführt werden, damit ein Cluster fehlerfrei arbeiten kann. Das Entfernen eines erforderlichen Namespaces aus der Liste der standardmäßig ausgeschlossenen Namespaces kann aufgrund eines erforderlichen Systempods Richtlinienverletzungen auslösen.

AKS erfordert die Ausführung von Systempods auf einem Cluster, um entscheidende Dienste wie die DNS-Auflösung bereitzustellen. Richtlinien, die die Funktionalität von Pods einschränken, können sich auf die Systempodstabilität auswirken. Infolgedessen sind die folgenden Namespaces **bei Zulassungsanforderungen während der Erstellung, Aktualisierung und Überwachung der Richtlinien** von der Bewertung der Richtlinien ausgeschlossen. Dies erzwingt den Ausschluss neuer Bereitstellungen für diese Namespaces aus Azure-Richtlinien.

1. kube-system
1. gatekeeper-system
1. azure-arc
1. aks-periscope

Zusätzliche benutzerdefinierte Namespaces können während der Erstellung, Aktualisierung und Überwachung von der Auswertung ausgeschlossen werden. Dies sollte verwendet werden, wenn Sie bestimmte Pods verwenden, die in einem sanktionierten Namespace ausgeführt werden und keine Überwachungsverstöße auslösen möchten.

## <a name="apply-the-baseline-initiative"></a>Anwenden der Baseline-Initiative

> [!TIP]
> Alle Richtlinien verwenden standardmäßig die Auswirkung „audit“ (überwachen). Die Auswirkungen können jederzeit durch Azure Policy aktualisiert werden, um sie zu verweigern.

Zum Anwenden der Baseline-Initiative kann die Zuweisung über das Azure-Portal erfolgen.
<!--
1. Navigate to the Policy service in Azure portal
1. In the left pane of the Azure Policy page, select **Definitions**
1. Search for "Baseline Profile" on the search pane to the right of the page
1. Select `Kubernetes Pod Security Standards Baseline Profile for Linux-based workloads` from the `Kubernetes` category
-->
1. Folgen Sie [diesem Link zum Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d), um die Baseline-Initiative zur Podsicherheit zu überprüfen.
1. Legen Sie den **Bereich** auf die Abonnementebene oder nur die Ressourcengruppe fest, die AKS-Cluster mit aktiviertem Azure Policy-Add-On enthält.
1. Wählen Sie die Seite **Parameter** aus, und aktualisieren Sie die **Auswirkung** von `audit` auf `deny`, um neue Bereitstellungen zu blockieren, die gegen die Baseline-Initiative verstoßen.
1. Fügen Sie zusätzliche Namespaces hinzu, um sie während der Erstellung, Aktualisierung und Überwachung von der Auswertung auszuschließen. [Einige Namespaces werden zwangsweise von der Richtlinienauswertung ausgeschlossen.](#namespace-exclusion)
![Auswirkung „update“](media/use-pod-security-on-azure-policy/update-effect.png)
1. Wählen Sie **Überprüfen + erstellen** aus, um die Richtlinien zu übermitteln.

Bestätigen Sie, dass Richtlinien auf Ihren Cluster angewendet werden, indem Sie `kubectl get constrainttemplates` ausführen.

> [!NOTE]
> Es kann bis zu [20 Minuten dauern](../governance/policy/concepts/policy-for-kubernetes.md#assign-a-built-in-policy-definition), bis Richtlinien in die einzelnen Cluster synchronisiert sind.

Die Ausgabe sollte in etwa wie folgt aussehen:

```console
$ kubectl get constrainttemplate
NAME                                     AGE
k8sazureallowedcapabilities              30m
k8sazureblockhostnamespace               30m
k8sazurecontainernoprivilege             30m
k8sazurehostfilesystem                   30m
k8sazurehostnetworkingports              30m
```

## <a name="validate-rejection-of-a-privileged-pod"></a>Überprüfen der Ablehnung eines privilegierten Pods

Testen Sie als Erstes, was passiert, wenn Sie einen Pod mit dem Sicherheitskontext `privileged: true` planen. Dieser Sicherheitskontext weitet die Podberechtigungen aus. Die Baseline-Initiative lässt keine privilegierten Pods zu, sodass die Anforderung abgelehnt wird, was zur Ablehnung der Bereitstellung führt.

Erstellen Sie eine Datei mit dem Namen `nginx-privileged.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: nginx
      securityContext:
        privileged: true
```

Erstellen Sie den Pod mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl apply -f nginx-privileged.yaml
```

Der Pod kann wie erwartet nicht geplant werden, wie in der folgenden Beispielausgabe zu sehen:

```console
$ kubectl apply -f privileged.yaml

Error from server ([denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}): error when creating "privileged.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}
```

Da der Pod die Planungsphase nicht erreicht, müssen keine Ressourcen gelöscht werden, und Sie können direkt mit dem nächsten Schritt fortfahren.

## <a name="test-creation-of-an-unprivileged-pod"></a>Testen der Erstellung eines nicht privilegierten Pods

Im vorherigen Beispiel hat das Containerimage automatisch versucht, Stammberechtigungen zu verwenden, um NGINX an den Port 80 zu binden. Diese Anforderung wurde von der Baseline-Richtlinieninitiative zurückgewiesen, sodass der Pod nicht gestartet werden kann. Lassen Sie uns jetzt versuchen, denselben NGINX-Pod ohne privilegierten Zugriff auszuführen.

Erstellen Sie eine Datei mit dem Namen `nginx-unprivileged.yaml`, und fügen Sie das folgende YAML-Manifest ein:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: nginx
```

Erstellen Sie den Pod mithilfe des Befehls [kubectl apply][kubectl-apply], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl apply -f nginx-unprivileged.yaml
```

Der Pod wird erfolgreich geplant. Wenn Sie mithilfe des Befehls [kubectl get pods][kubectl-get] den Status des Pods überprüfen, sehen Sie, dass der Pod ausgeführt wird (*Running*):

```console
$ kubectl get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          18s
```

Dieses Beispiel zeigt die Baseline-Initiative, die sich nur auf Bereitstellungen auswirkt, die gegen Richtlinien in der Sammlung verstoßen. Zulässige Bereitstellungen funktionieren weiterhin.

Löschen Sie den nicht privilegierten NGINX-Pod mithilfe des Befehls [kubectl delete][kubectl-delete], und geben Sie den Namen Ihres YAML-Manifests an:

```console
kubectl delete -f nginx-unprivileged.yaml
```

## <a name="disable-policies-and-the-azure-policy-add-on"></a>Deaktivieren von Richtlinien und des Azure Policy-Add-Ons

So entfernen Sie die Baseline-Initiative

1. Navigieren Sie zum Richtlinienbereich im Azure-Portal.
1. Wählen Sie im linken Bereich **Zuweisungen** aus.
1. Klicken Sie auf die Schaltfläche „...“ neben dem Baseline-Profil.
1. Wählen Sie „Zuweisung löschen“ aus.

![Zuweisung löschen](media/use-pod-security-on-azure-policy/delete-assignment.png)

Verwenden Sie den Befehl [az aks disable-addons][az-aks-disable-addons], um das Azure Policy-Add-On zu deaktivieren.

```azure-cli
az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
```

Erfahren Sie, wie Sie das [Azure Policy-Add-On aus dem Azure-Portal](../governance/policy/concepts/policy-for-kubernetes.md#remove-the-add-on-from-aks) entfernen.

## <a name="migrate-from-kubernetes-pod-security-policy-to-azure-policy"></a>Migrieren von der Kubernetes-Podsicherheitsrichtlinie zu Azure Policy

Sie müssen die folgenden Maßnahmen auf einem Cluster ergreifen, um von der Podsicherheitsrichtlinie zu migrieren.

1. [Deaktivieren Sie die Podsicherheitsrichtlinie](use-pod-security-policies.md#clean-up-resources) auf dem Cluster.
1. Aktivieren Sie das [Azure Policy-Add-On][kubernetes-policy-reference].
1. Aktivieren Sie die gewünschten Azure-Richtlinien aus den [verfügbaren integrierten Richtlinien][policy-samples].

Nachfolgend finden Sie eine Zusammenfassung der Verhaltensänderungen zwischen der Podsicherheitsrichtlinie und Azure Policy.

|Szenario| Podsicherheitsrichtlinie | Azure Policy |
|---|---|---|
|Installation|Aktivieren des Podsicherheitsrichtlinien-Features |Aktivieren des Azure Policy-Add-Ons
|Bereitstellen von Richtlinien| Bereitstellen der Podsicherheitsrichtlinien-Ressource| Weisen Sie Azure-Richtlinien dem Abonnement- oder Ressourcengruppenbereich zu. Das Azure Policy-Add-On ist für Kubernetes-Ressourcenanwendungen erforderlich.
| Standardrichtlinien | Wenn die Podsicherheitsrichtlinie in AKS aktiviert ist, werden standardmäßig die Richtlinien „Privilegiert“ (Privileged) und „Uneingeschränkt“ (Unrestricted) angewendet. | Durch die Aktivierung des Azure Policy-Add-Ons werden keine Standardrichtlinien angewendet. Sie müssen Richtlinien in Azure Policy explizit aktivieren.
| Wer kann Richtlinien erstellen und zuweisen? | Der Clusteradministrator erstellt eine Ressource für Podsicherheitsrichtlinien. | Benutzer müssen in der AKS-Clusterressourcengruppe mindestens die Berechtigungen „Besitzer“ oder „Ressourcenrichtlinienmitwirkender“ aufweisen. – Über die API können Benutzer Richtlinien im Bereich der AKS-Clusterressourcen zuweisen. Der Benutzer sollte mindestens die Berechtigungen „Besitzer“ oder „Ressourcenrichtlinienmitwirkender" für die AKS-Clusterressource aufweisen. – Im Azure-Portal können Richtlinien auf Verwaltungsgruppen-/Abonnement-/Ressourcengruppenebene angewendet werden.
| Autorisierungsrichtlinien| Benutzer und Dienstkonten benötigen explizite Berechtigungen zur Verwendung von Podsicherheitsrichtlinien. | Für die Autorisierung von Richtlinien ist keine zusätzliche Zuweisung erforderlich. Nachdem Richtlinien in Azure zugewiesen wurden, können alle Clusterbenutzer diese Richtlinien verwenden.
| Anwendbarkeit von Richtlinien | Der Administratorbenutzer umgeht die Erzwingung von Podsicherheitsrichtlinien. | Alle Benutzer (Administrator und Nicht-Administrator) sehen dieselben Richtlinien. Es gibt keine spezielle Schreibweise auf der Basis von Benutzern. Die Anwendung von Richtlinien kann auf der Namespaceebene ausgeschlossen werden.
| Geltungsbereich der Richtlinie | Podsicherheitsrichtlinien sind keinem Namespace zugeordnet | Die von Azure Policy verwendeten Einschränkungsvorlagen sind keinem Namespace zugeordnet.
| Deny/Audit/Mutation (Aktion) | Podsicherheitsrichtlinien unterstützen nur Aktionen vom Typ „deny“. Die Mutation kann mit Standardwerten bei Erstellungsanforderungen erfolgen. Die Validierung kann während der Aktualisierungsanforderungen durchgeführt werden.| Azure Policy unterstützt sowohl Aktionen vom Typ „audit“ als auch vom Typ „deny“. Die Mutation wird noch nicht unterstützt, ist aber geplant.
| Compliance von Sicherheitsrichtlinien | Es gibt keine Transparenz hinsichtlich der Compliance von Pods, die vor der Aktivierung der Podsicherheitsrichtlinie existierten. Nicht konforme Pods, die nach der Aktivierung von Podsicherheitsrichtlinien erstellt wurden, werden verweigert. | Nicht konforme Pods, die vor der Anwendung von Azure-Richtlinien existierten, würden in Richtlinienverletzungen angezeigt werden. Nicht konforme Pods, die nach der Aktivierung von Azure-Richtlinien erstellt wurden, werden verweigert, wenn Richtlinien mit einer Auswirkung vom Typ „deny“ festgelegt werden.
| Anzeigen von Richtlinien für den Cluster | `kubectl get psp` | `kubectl get constrainttemplate` – Alle Richtlinien werden zurückgegeben.
| Podsicherheitsrichtlinie (Standard): Privilegiert | Beim Aktivieren des Features wird standardmäßig eine Sicherheitsrichtlinienressource für privilegierte Pods erstellt. | Der privilegierte Modus impliziert keine Einschränkung, daher ist er gleichbedeutend mit keiner Zuweisung von Azure-Richtlinien.
| [Podsicherheitsrichtlinie (Standard): Baseline/Standard](https://kubernetes.io/docs/concepts/security/pod-security-standards/#baseline-default) | Der Benutzer installiert eine Baseline-Ressource der Podsicherheitsrichtlinie. | Azure Policy bietet eine [integrierte Baseline-Initiative](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2Fa8640138-9b0a-4a28-b8cb-1666c838647d), die sich an der Baseline-Richtlinie für Pods orientiert.
| [Podsicherheitsrichtlinie (Standard): Eingeschränkt](https://kubernetes.io/docs/concepts/security/pod-security-standards/#restricted) | Der Benutzer installiert eine eingeschränkte Ressource der Podsicherheitsrichtlinie. | Azure Policy bietet eine [integrierte eingeschränkte Initiative](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicySetDefinitions%2F42b8ef37-b724-4e24-bbc8-7a7708edfe00), die der eingeschränkten Podsicherheitsrichtlinie zugeordnet ist.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wurde Ihnen gezeigt, wie Sie eine Azure-Richtlinie anwenden können, die die Bereitstellung von privilegierten Pods einschränkt, um die Nutzung des privilegierten Zugriffs zu verhindern. Es gibt viele anwendbare Richtlinien, z. B. Richtlinien zum Einschränken der Verwendung von Volumes. Weitere Informationen zu den verfügbaren Optionen finden Sie in den [Referenzdokumentationen zu Azure Policy für Kubernetes][kubernetes-policy-reference].

Weitere Informationen zum Einschränken des Netzwerkdatenverkehrs von Pods finden Sie unter [Vorschauversion: Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)][network-policies].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[kubernetes-policy-reference]: ../governance/policy/concepts/policy-for-kubernetes.md
[policy-samples]: policy-samples.md#microsoftcontainerservice
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policies]: use-network-policies.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-disable-addons]: /cli/azure/aks#az-aks-disable-addons
[az-policy-assignment-delete]: /cli/azure/policy/assignment#az-policy-assignment-delete