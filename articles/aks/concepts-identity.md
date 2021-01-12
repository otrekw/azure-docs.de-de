---
title: Konzepte – Zugriff und Identität in Azure Kubernetes Service (AKS)
description: Erfahren Sie mehr über Zugriff und Identität in Azure Kubernetes Service (AKS), z. B. zu den Bereichen Integration von Azure Active Directory, rollenbasierte Kubernetes-Zugriffssteuerung (Kubernetes RBAC) sowie Rollen und Bindungen.
services: container-service
ms.topic: conceptual
ms.date: 07/07/2020
author: palma21
ms.author: jpalma
ms.openlocfilehash: 3c291d9a9d48b6f75148b673848b8451521bab91
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97615800"
---
# <a name="access-and-identity-options-for-azure-kubernetes-service-aks"></a>Zugriffs- und Identitätsoptionen für Azure Kubernetes Service (AKS)

Es gibt verschiedene Möglichkeiten, Kubernetes-Cluster zu authentifizieren, den Zugriff/die Autorisierung zu steuern und sie zu schützen. Mit der rollenbasierten Zugriffssteuerung für Kubernetes (Kubernetes RBAC) können Sie den Zugriff von Benutzern, Gruppen und Dienstkonten auf die Ressourcen beschränken, die wirklich benötigt werden. Mit Azure Kubernetes Service (AKS) können Sie die Sicherheits- und Berechtigungsstruktur weiter verbessern, indem Sie Azure Active Directory und Azure RBAC verwenden. Mit diesen Ansätzen können Sie den Zugriff auf den Cluster schützen, und es werden nur die mindestens erforderlichen Berechtigungen für Entwickler und Operatoren bereitgestellt.

In diesem Artikel werden die wichtigsten Konzepte für die Authentifizierung und Zuweisung von Berechtigungen in AKS vorgestellt.

## <a name="aks-service-permissions"></a>AKS-Dienstberechtigungen

Beim Erstellen eines Clusters werden von AKS Ressourcen erstellt oder geändert, die für die Erstellung und Ausführung des Clusters benötigt werden, z. B. VMs und NICs. Dies erfolgt im Namen des Benutzers, der den Cluster erstellt. Diese Identität unterscheidet sich von der Identitätsberechtigung des Clusters, die während der Erstellung des Clusters erstellt wird.

### <a name="identity-creating-and-operating-the-cluster-permissions"></a>Identität für die Erstellung und den Betrieb des Clusters: Berechtigungen

Die folgenden Berechtigungen werden von der Identität benötigt, die zum Erstellen und Betreiben des Clusters verwendet wird.

| Berechtigung | `Reason` |
|---|---|
| Microsoft.Compute/diskEncryptionSets/read | Erforderlich zum Lesen der Datenträgerverschlüsselungssatz-ID. |
| Microsoft.Compute/proximityPlacementGroups/write | Erforderlich zum Aktualisieren von Näherungsplatzierungsgruppen. |
| Microsoft.Network/applicationGateways/read <br/> Microsoft.Network/applicationGateways/write <br/> Microsoft.Network/virtualNetworks/subnets/join/action | Erforderlich zum Konfigurieren von Anwendungsgateways und Beitreten zum Subnetz. |
| Microsoft.Network/virtualNetworks/subnets/join/action | Erforderlich zum Konfigurieren der Netzwerksicherheitsgruppe für das Subnetz, wenn ein benutzerdefiniertes VNET verwendet wird.|
| Microsoft.Network/publicIPAddresses/join/action <br/> Microsoft.Network/publicIPPrefixes/join/action | Erforderlich zum Konfigurieren der ausgehenden öffentlichen IP-Adressen auf der Load Balancer Standard-Instanz. |
| Microsoft.OperationalInsights/workspaces/sharedkeys/read <br/> Microsoft.OperationalInsights/workspaces/read <br/> Microsoft.OperationsManagement/solutions/write <br/> Microsoft.OperationsManagement/solutions/read <br/> Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | Erforderlich zum Erstellen und Aktualisieren von Log Analytics-Arbeitsbereichen und der Azure-Überwachung für Container. |

### <a name="aks-cluster-identity-permissions"></a>AKS-Clusteridentität: Berechtigungen

Die folgenden Berechtigungen werden von der AKS-Clusteridentität verwendet, die beim Erstellen des Clusters erstellt und dem AKS-Cluster zugeordnet wird. Die einzelnen Berechtigungen werden aus den folgenden Gründen verwendet:

| Berechtigung | `Reason` |
|---|---|
| Microsoft.Network/loadBalancers/delete <br/> Microsoft.Network/loadBalancers/read <br/> Microsoft.Network/loadBalancers/write | Erforderlich zum Konfigurieren des Lastenausgleichs für einen LoadBalancer-Dienst. |
| Microsoft.Network/publicIPAddresses/delete <br/> Microsoft.Network/publicIPAddresses/read <br/> Microsoft.Network/publicIPAddresses/write | Erforderlich zum Ermitteln und Konfigurieren von öffentlichen IP-Adressen für einen LoadBalancer-Dienst. |
| Microsoft.Network/publicIPAddresses/join/action | Erforderlich zum Konfigurieren von öffentlichen IP-Adressen für einen LoadBalancer-Dienst. |
| Microsoft.Network/networkSecurityGroups/read <br/> Microsoft.Network/networkSecurityGroups/write | Erforderlich zum Erstellen oder Löschen von Sicherheitsregeln für einen LoadBalancer-Dienst. |
| Microsoft.Compute/disks/delete <br/> Microsoft.Compute/disks/read <br/> Microsoft.Compute/disks/write <br/> Microsoft.Compute/locations/DiskOperations/read | Erforderlich zum Konfigurieren von AzureDisks. |
| Microsoft.Storage/storageAccounts/delete <br/> Microsoft.Storage/storageAccounts/listKeys/action <br/> Microsoft.Storage/storageAccounts/read <br/> Microsoft.Storage/storageAccounts/write <br/> Microsoft.Storage/operations/read | Erforderlich zum Konfigurieren von Speicherkonten für AzureFile oder AzureDisk. |
| Microsoft.Network/routeTables/read <br/> Microsoft.Network/routeTables/routes/delete <br/> Microsoft.Network/routeTables/routes/read <br/> Microsoft.Network/routeTables/routes/write <br/> Microsoft.Network/routeTables/write | Erforderlich zum Konfigurieren von Routingtabellen und Routen für Knoten. |
| Microsoft.Compute/virtualMachines/read | Erforderlich zum Suchen nach Informationen für virtuelle Computer in einer VM-Verfügbarkeitsgruppe, z. B. Zonen, Fehlerdomäne, Größe und Datenträger. |
| Microsoft.Compute/virtualMachines/write | Erforderlich, um AzureDisks an einen virtuellen Computer in einer VM-Verfügbarkeitsgruppe anzufügen. |
| Microsoft.Compute/virtualMachineScaleSets/read <br/> Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read <br/> Microsoft.Compute/virtualMachineScaleSets/virtualmachines/instanceView/read | Erforderlich zum Suchen nach Informationen für virtuelle Computer in einer VM-Skalierungsgruppe, z. B. Zonen, Fehlerdomäne, Größe und Datenträger. |
| Microsoft.Network/networkInterfaces/write | Erforderlich, um einen virtuellen Computer aus einer VM-Verfügbarkeitsgruppe dem Back-End-Adresspool eines Lastenausgleichsmoduls hinzuzufügen. |
| Microsoft.Compute/virtualMachineScaleSets/write | Erforderlich, um eine VM-Skalierungsgruppe dem Back-End-Adresspool eines Lastenausgleichsmoduls hinzuzufügen und Knoten in einer VM-Skalierungsgruppe aufzuskalieren. |
| Microsoft.Compute/virtualMachineScaleSets/virtualmachines/write | Erforderlich, um AzureDisks anzufügen und einen virtuellen Computer aus einer VM-Skalierungsgruppe dem Lastenausgleichsmodul hinzuzufügen. |
| Microsoft.Network/networkInterfaces/read | Erforderlich, um für virtuelle Computer in einer VM-Verfügbarkeitsgruppe nach internen IP-Adressen und Back-End-Adresspools eines Lastenausgleichsmoduls zu suchen. |
| Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Erforderlich, um für einen virtuellen Computer in einer VM-Skalierungsgruppe nach internen IP-Adressen und Back-End-Adresspools eines Lastenausgleichsmoduls zu suchen. |
| Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ ipconfigurations/publicipaddresses/read | Erforderlich, um in einer VM-Skalierungsgruppe nach öffentlichen IP-Adressen für einen virtuellen Computer zu suchen. |
| Microsoft.Network/virtualNetworks/read <br/> Microsoft.Network/virtualNetworks/subnets/read | Erforderlich für die Überprüfung, ob für das interne Lastenausgleichsmodul in einer anderen Ressourcengruppe ein Subnetz vorhanden ist. |
| Microsoft.Compute/snapshots/delete <br/> Microsoft.Compute/snapshots/read <br/> Microsoft.Compute/snapshots/write | Erforderlich zum Konfigurieren von Momentaufnahmen für AzureDisk. |
| Microsoft.Compute/locations/vmSizes/read <br/> Microsoft.Compute/locations/operations/read | Erforderlich, um nach VM-Größen zur Ermittlung von Grenzwerten für AzureDisk-Volumes zu suchen. |

### <a name="additional-cluster-identity-permissions"></a>Clusteridentität: Weitere Berechtigungen

Die folgenden zusätzlichen Berechtigungen werden von der Clusteridentität benötigt, wenn ein Cluster mit bestimmten Attributen erstellt wird. Da diese Berechtigungen nicht automatisch zugewiesen werden, müssen Sie sie der Clusteridentität nach der Erstellung hinzufügen.

| Berechtigung | `Reason` |
|---|---|
| Microsoft.Network/networkSecurityGroups/write <br/> Microsoft.Network/networkSecurityGroups/read | Erforderlich, wenn eine Netzwerksicherheitsgruppe in einer anderen Ressourcengruppe verwendet wird. Erforderlich zum Konfigurieren von Sicherheitsregeln für einen LoadBalancer-Dienst. |
| Microsoft.Network/virtualNetworks/subnets/read <br/> Microsoft.Network/virtualNetworks/subnets/join/action | Erforderlich, wenn ein Subnetz in einer anderen Ressourcengruppe, z. B. einem benutzerdefinierten VNET, verwendet wird. |
| Microsoft.Network/routeTables/routes/read <br/> Microsoft.Network/routeTables/routes/write | Erforderlich, wenn ein Subnetz mit einer zugeordneten Routingtabelle in einer anderen Ressourcengruppe, z. B. einem VNET mit einer benutzerdefinierten Routingtabelle, verwendet wird. Erforderlich zum Überprüfen, ob für das Subnetz in der anderen Ressourcengruppe bereits ein Subnetz vorhanden ist. |
| Microsoft.Network/virtualNetworks/subnets/read | Erforderlich, wenn ein internes Lastenausgleichsmodul in einer anderen Ressourcengruppe verwendet wird. Erforderlich für die Überprüfung, ob für das interne Lastenausgleichsmodul in der Ressourcengruppe bereits ein Subnetz vorhanden ist. |

## <a name="kubernetes-role-based-access-control-kubernetes-rbac"></a>Rollenbasierte Zugriffssteuerung in Kubernetes (Kubernetes RBAC)

Für eine präzise Filterung der Aktionen, die Benutzer ausführen können, wird in Kubernetes die rollenbasierte Zugriffssteuerung (Kubernetes RBAC) verwendet. Mit diesem Steuerungsmechanismus können Sie Benutzern oder Benutzergruppen die Berechtigung für bestimmte Aktionen (z. B. Ressourcen erstellen bzw. ändern oder Protokolle zur Workload ausgeführter Anwendungen anzeigen) zuweisen. Diese Berechtigungen können auf einen einzelnen Namespace begrenzt oder für den gesamten AKS-Cluster erteilt werden. Mit der rollenbasierten Zugriffssteuerung (RBAC) von Kubernetes können Sie *Rollen* erstellen, um Berechtigungen zu definieren, und anschließend diese Rollen mit *Rollenbindungen* Benutzern zuweisen.

Weitere Informationen finden Sie unter [Verwenden der Kubernetes RBAC-Autorisierung][kubernetes-rbac].

### <a name="roles-and-clusterroles"></a>Rollen und Clusterrollen

Bevor Sie Benutzern mit der rollenbasierten Zugriffssteuerung von Kubernetes Berechtigungen zuweisen, müssen Sie diese Berechtigungen als *Rolle* definieren. Kubernetes-Rollen *gewähren* Berechtigungen. In diesem Konzept ist eine Berechtigung des Typs *Verweigern* nicht vorgesehen.

Rollen werden zum Gewähren von Berechtigungen innerhalb eines Namespace verwendet. Wenn Sie Berechtigungen für den gesamten Cluster oder für Clusterressourcen außerhalb eines bestimmten Namespace erteilen müssen, können Sie stattdessen *Clusterrollen* verwenden.

Auch mit einer Clusterrolle werden Berechtigungen für Ressourcen erteilt. Allerdings kann sie auf Ressourcen im gesamten Cluster angewendet werden und ist nicht auf einen bestimmten Namespace beschränkt.

### <a name="rolebindings-and-clusterrolebindings"></a>Rollenbindungen und Clusterrollenbindungen

Sobald Rollen definiert sind, um Berechtigungen für Ressourcen zu gewähren, weisen Sie diese Berechtigungen der rollenbasierten Zugriffssteuerung von Kubernetes mit einer *Rollenbindung* zu. Wenn Ihr AKS-Cluster [in Azure Active Directory (Azure AD) integriert](#azure-active-directory-integration) ist, erhalten die Azure AD-Benutzer über Bindungen Berechtigungen zum Durchführen von Aktionen innerhalb des Clusters. Dies ist unter [Steuern des Zugriffs auf Clusterressourcen per rollenbasierter Zugriffssteuerung von Kubernetes und mit Azure Active Directory-Identitäten in Azure Kubernetes Service](azure-ad-rbac.md) beschrieben.

Rollenbindungen werden zum Zuweisen von Rollen für einen bestimmten Namespace verwendet. Mit diesem Ansatz können Sie einen einzelnen AKS-Cluster logisch aufteilen und den Benutzern nur den Zugriff auf die Anwendungsressourcen im jeweils zugewiesenen Namespace erlauben. Wenn Sie Rollen über den gesamten Cluster oder an Clusterressourcen außerhalb eines bestimmten Namespace binden müssen, können Sie stattdessen *Clusterrollenbindungen* verwenden.

Eine Clusterrollenbindung funktioniert auf dieselbe Weise wie die Rollenbindung für Benutzer, allerdings kann sie auf Ressourcen im gesamten Cluster angewendet werden und ist nicht auf einen bestimmten Namespace beschränkt. Mit diesem Ansatz können Sie Ihren Administratoren oder Supporttechnikern Zugriff auf alle Ressourcen im AKS-Cluster gewähren.


> [!NOTE]
> Alle von Microsoft/AKS ausgeführten Clusteraktionen werden mit Benutzereinwilligung unter der integrierten Kubernetes-Rolle `aks-service` und der integrierten Rollenbindung `aks-service-rolebinding` durchgeführt. Diese Rolle ermöglicht es AKS, Clusterprobleme zu beheben und zu diagnostizieren, damit können jedoch keine Berechtigungen geändert oder Rollen bzw. Rollenbindungen erstellt oder andere Aktionen mit hohen Berechtigungen ausgeführt werden. Der Rollenzugriff ist nur unter aktiven Supporttickets mit Just-in-time-Zugriff (JIT) aktiviert. [Hier finden Sie weitere Informationen zu AKS-Supportrichtlinien](support-policies.md).


### <a name="kubernetes-service-accounts"></a>Kubernetes-Dienstkonten

Einer der Hauptbenutzertypen in Kubernetes ist ein *Dienstkonto*. Ein Dienstkonto ist in der Kubernetes-API vorhanden und wird von dieser verwaltet. Die Anmeldeinformationen für Dienstkonten werden als Kubernetes-Geheimnisse gespeichert. Dadurch können sie von autorisierten Pods für die Kommunikation mit dem API-Server verwendet werden. Die meisten API-Anforderungen stellen einen Authentifizierungstoken für ein Dienstkonto oder ein normales Benutzerkonto bereit.

Normale Benutzerkonten ermöglichen den eher herkömmlichen Zugriff für Personen wie Administratoren oder Entwickler und nicht nur für Dienste und Prozesse. Kubernetes selbst bietet keine Identitätsverwaltungslösung, in der reguläre Benutzerkonten und Kennwörter gespeichert werden. Stattdessen können externe Identitätsverwaltungslösungen in Kubernetes integriert werden. Bei AKS-Clustern fungiert Azure Active Directory als integrierte Identitätsverwaltungslösung.

Weitere Informationen zu den Identitätsoptionen in Kubernetes finden Sie unter [Authentifizierung in Kubernetes][kubernetes-authentication].

## <a name="azure-active-directory-integration"></a>Azure Active Directory-Integration

Die Sicherheit der AKS-Cluster kann durch die Integration von Azure Active Directory (AD) erhöht werden. Azure Active Directory (Azure AD) ist ein mehrinstanzenfähiger, cloudbasierter Verzeichnis- und Identitätsverwaltungsdienst, der Kernverzeichnisdienste, Anwendungszugriffsverwaltung und Identitätsschutz in sich vereint und auf jahrzehntelange Erfahrung mit der Identitätsverwaltung in Unternehmen aufbaut. Mit Azure AD können Sie lokale Identitäten in AKS-Cluster integrieren und so eine zentrale Quelle für die Kontoverwaltung und Sicherheit bereitstellen.

![Azure Active Directory-Integration mit AKS-Clustern](media/concepts-identity/aad-integration.png)

Mit in Azure AD integrierten AKS-Clustern können Sie Benutzern oder Gruppen Zugriff auf Kubernetes-Ressourcen in einem Namespace oder im ganzen Cluster gewähren. Um einen `kubectl`-Konfigurationskontext abzurufen, kann ein Benutzer den Befehl [az aks get-credentials][az-aks-get-credentials] ausführen. Wenn ein Benutzer dann mit dem AKS-Cluster mit `kubectl` interagiert, wird er aufgefordert, sich mit seinen Azure AD-Anmeldeinformationen anzumelden. Dieser Ansatz stellt eine zentrale Quelle für die Verwaltung von Benutzerkonten und Anmeldekennwörtern bereit. Der Benutzer kann nur auf die Ressourcen zugreifen, die der Clusteradministrator definiert hat.

Die Azure AD-Authentifizierung wird für AKS-Cluster mit OpenID Connect bereitgestellt. OpenID Connect ist eine Identitätsebene, die auf dem OAuth 2.0-Protokoll aufbaut. Weitere Informationen zu OpenID Connect finden Sie in der [OpenID Connect-Dokumentation][openid-connect]. Innerhalb des Kubernetes-Clusters werden Authentifizierungstoken mithilfe der [Webhooktokenauthentifizierung][webhook-token-docs] überprüft. Die Webhooktokenauthentifizierung wird als Teil des AKS-Clusters konfiguriert und verwaltet.

### <a name="webhook-and-api-server"></a>Webhook und API-Server

![Authentifizierungsablauf für Webhook und API-Server](media/concepts-identity/auth-flow.png)

Wie in der obigen Abbildung gezeigt, ruft der API-Server den AKS-Webhookserver auf und führt die folgenden Schritte aus:

1. Die Azure AD-Clientanwendung wird von kubectl zum Anmelden von Benutzern mit dem [OAuth 2.0-Flow für die Geräteautorisierungsgenehmigung](../active-directory/develop/v2-oauth2-device-code.md) verwendet.
2. Azure AD stellt dabei ein access_token, ein id_token und ein refresh_token bereit.
3. Der Benutzer sendet mit einem access_token aus kubeconfig eine Anforderung an kubectl.
4. Kubectl sendet das access_token an den API-Server.
5. Der API-Server wird mit dem Auth-WebHook-Server konfiguriert, um die Validierung auszuführen.
6. Der Webhookserver für die Authentifizierung bestätigt, dass die Signatur des JSON-Webtokens gültig ist, indem der öffentliche Azure AD-Signaturschlüssel überprüft wird.
7. Die Serveranwendung verwendet vom Benutzer bereitgestellte Anmeldeinformationen, um die Gruppenmitgliedschaften des angemeldeten Benutzers aus der MS Graph-API abzufragen.
8. An den API-Server wird eine Antwort mit Benutzerinformationen gesendet, z. B. mit dem UPN-Anspruch (User Principal Name) des Zugriffstokens und der Gruppenmitgliedschaft des Benutzers basierend auf der Objekt-ID.
9. Die API führt eine Autorisierungsentscheidung basierend auf der Kubernetes-Rolle bzw. -Rollenbindung aus.
10. Nach der Autorisierung gibt der API-Server eine Antwort an kubectl zurück.
11. Kubectl stellt Feedback für den Benutzer bereit.
 
**[Hier](managed-aad.md) finden Sie Informationen zur Integration von AKS und AAD.**

## <a name="azure-role-based-access-control-azure-rbac"></a>Rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)

Azure RBAC ist ein Autorisierungssystem, das auf [Azure Resource Manager](../azure-resource-manager/management/overview.md) basiert und eine präzise Verwaltung des Zugriffs auf Azure-Ressourcen ermöglicht.

 Die rollenbasierte Zugriffssteuerung von Azure ist für die Ressourcen in Ihrem Azure-Abonnement, und die rollenbasierte Zugriffssteuerung von Kubernetes ist für Ressourcen innerhalb Ihres Azure-Clusters bestimmt. 

Mit der rollenbasierten Zugriffssteuerung von Azure erstellen Sie eine *Rollendefinition*, welche die anzuwendenden Berechtigungen erläutert. Dieser Rollendefinition wird dann über eine *Rollenzuweisung* ein Benutzer oder eine Gruppe für einen bestimmten *Bereich* zugewiesen. Dieser Bereich kann eine einzelne Ressource oder eine Ressourcengruppe sein oder das ganze Abonnement beinhalten.

Weitere Informationen finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung von Azure (Azure Role-Based Access Control, Azure RBAC)?][azure-rbac]

Für den vollständigen Betrieb eines AKS-Clusters sind zwei Zugriffsebenen erforderlich: 
1. [Zugriff auf die AKS-Ressource in Ihrem Azure-Abonnement.](#azure-rbac-to-authorize-access-to-the-aks-resource) Hiermit können Sie z. B. die Skalierung oder das Upgrade Ihres Clusters mithilfe der AKS-APIs steuern und kubeconfig pullen.
2. Zugriff auf die Kubernetes-API. Dieser Zugriff wird entweder durch [Kubernetes RBAC](#kubernetes-role-based-access-control-kubernetes-rbac) (herkömmlich) oder durch die [Integration von Azure RBAC in AKS für die Kubernetes-Autorisierung](#azure-rbac-for-kubernetes-authorization-preview) gesteuert.

### <a name="azure-rbac-to-authorize-access-to-the-aks-resource"></a>Azure RBAC zum Autorisieren des Zugriffs auf die AKS-Ressource

Mit Azure RBAC können Sie Ihren Benutzern (oder Identitäten) einen präzisen Zugriff auf AKS-Ressourcen in einem Abonnement oder abonnementsübergreifend bereitstellen. Beispielsweise können Sie mit der [Rolle „Mitwirkender“ für den Azure Kubernetes Service](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-contributor-role) Aktionen wie das Skalieren und Aktualisieren Ihres Clusters durchführen. Ein anderer Benutzer mit der [Cluster-Admin-Rolle für den Azure Kubernetes Service](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role) verfügt nur über die Berechtigung zum Pullen der kubeconfig-Administratordatei.

Alternativ dazu können Sie dem Benutzer die allgemeine Rolle [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor) zuweisen. Diese umfasst die oben genannten Berechtigungen sowie alle Aktionen, die für die AKS-Ressource möglich sind, mit Ausnahme der Verwaltung der Berechtigungen selbst.

Weitere Informationen zum Schützen des Zugriffs auf die kubeconfig-Datei, die den Zugriff auf die Kubernetes-API ermöglicht, mit Azure RBAC finden Sie [hier](control-kubeconfig-access.md).

### <a name="azure-rbac-for-kubernetes-authorization-preview"></a>Azure RBAC für die Kubernetes-Autorisierung (Vorschau)

Mit der Azure-RBAC-Integration verwendet AKS einen Kubernetes-Webhookserver für die Autorisierung, damit Sie Berechtigungen und Zuweisungen von in Azure AD integrierten K8s-Clusterressourcen mit Azure-Rollendefinitionen und Rollenzuweisungen verwalten können.

![Azure RBAC für die Kubernetes-Autorisierungsablauf](media/concepts-identity/azure-rbac-k8s-authz-flow.png)

Wie im Diagramm oben gezeigt, folgen bei der Verwendung der Azure-RBAC-Integration alle Anforderungen an die Kubernetes-API demselben Authentifizierungsablauf, der im [Abschnitt zur Azure Active Directory-Integration](#azure-active-directory-integration) erläutert wurde. 

Die Anforderung wird danach aber nicht nur durch Kubernetes RBAC autorisiert, sondern wird von Azure autorisiert, solange die Identität, die die Anforderung gesendet hat, in AAD vorhanden ist. Wenn die Identität in AAD nicht vorhanden ist (z. B. ein Kubernetes-Dienstkonto), wird Azure RBAC nicht gestartet, und es wird nur die normale Kubernetes RBAC verwendet.

In diesem Szenario können Sie den Benutzern wie bei Kubernetes-Rollen eine der vier integrierten Rollen zuweisen oder benutzerdefinierte Rollen erstellen, wobei jedoch die Azure RBAC-Mechanismen und -APIs verwendet werden. 

Mit dieser Funktion können Sie z. B. nicht nur Benutzern Berechtigungen für die AKS-Ressource über Abonnements hinweg erteilen, sondern auch die Rolle und Berechtigungen einrichten und zuweisen, die sie in den einzelnen Clustern haben, die den Zugriff auf die Kubernetes-API steuern. Beispielsweise können Sie die Rolle `Azure Kubernetes Service RBAC Viewer` auf Abonnementebene gewähren, und der Empfänger kann alle Kubernetes-Objekte aus allen Clustern auflisten und abrufen, diese jedoch nicht ändern.


#### <a name="built-in-roles"></a>Integrierte Rollen

AKS stellt die folgenden vier integrierten Rollen bereit. Sie sind mit den [integrierten Rollen in Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#user-facing-roles) vergleichbar, weisen aber einige Unterschiede wie der Unterstützung von CRDs auf. Die vollständige Liste mit den Aktionen, die für die einzelnen integrierten Rollen zulässig sind, finden Sie [hier](../role-based-access-control/built-in-roles.md).

| Role                                | BESCHREIBUNG  |
|-------------------------------------|--------------|
| RBAC-Viewer von Azure Kubernetes Service  | Ermöglicht schreibgeschützten Zugriff, um die meisten Objekte in einem Namespace anzuzeigen. Es ist nicht möglich, Rollen oder Rollenbindungen anzuzeigen. Diese Rolle lässt das Anzeigen von `Secrets` nicht zu, da das Lesen des Inhalts von Geheimnissen den Zugriff auf `ServiceAccount`-Anmeldeinformationen im Namespace ermöglicht, was den API-Zugriff als beliebiger `ServiceAccount` im Namespace ermöglichen würde (eine Form von Berechtigungsausweitung).  |
| RBAC-Writer von Azure Kubernetes Service | Ermöglicht Lese-/Schreibzugriff auf die meisten Objekte in einem Namespace. Diese Rolle lässt das Anzeigen oder Ändern von Rollen oder Rollenbindungen nicht zu. Diese Rolle ermöglicht jedoch den Zugriff auf `Secrets` und das Ausführen von Pods als beliebiges Dienstkonto im Namespace, sodass sie verwendet werden kann, um die API-Zugriffsebenen eines beliebigen ServiceAccount im Namespace zu erhalten. |
| RBAC-Administrator von Azure Kubernetes Service  | Ermöglicht Administratorzugriff, der in einem Namespace erteilt werden soll. Ermöglicht Lese-/Schreibzugriff auf die meisten Ressourcen in einem Namespace (oder Clusterbereich), einschließlich der Möglichkeit zum Erstellen von Rollen und Rollenbindungen innerhalb des Namespace. Diese Rolle lässt keinen Schreibzugriff auf das Ressourcenkontingent oder den Namespace selbst zu. |
| RBAC-Clusteradministrator von Azure Kubernetes Service  | Ermöglicht Superuserzugriff, um beliebige Aktionen für beliebige Ressourcen auszuführen. Diese Rolle ermöglicht die vollständige Kontrolle über alle Ressourcen im Cluster und in allen Namespaces. |

**[Hier](manage-azure-rbac.md) finden Sie weitere Informationen zum Aktivieren von Azure RBAC für die Kubernetes-Autorisierung.**

## <a name="summary"></a>Zusammenfassung

Diese Tabelle fasst zusammen, wie Benutzer sich bei Kubernetes authentifizieren können, wenn Azure AD-Integration aktiviert ist.  In allen Fällen lautet die Befehlssequenz des Benutzers wie folgt:
1. Ausführen von `az login` zur Authentifizierung bei Azure.
1. Ausführen von `az aks get-credentials`, um Anmeldeinformationen für den Cluster in `.kube/config` herunterzuladen.
1. Ausführen von `kubectl`-Befehlen (der erste davon kann die browserbasierte Authentifizierung zum Authentifizieren beim Cluster auslösen, wie in der folgenden Tabelle beschrieben).

Die Rollenzuweisung, auf die in der zweiten Spalte verwiesen wird, ist die Azure RBAC-Rollenzuweisung, die auf der Registerkarte **Zugriffssteuerung** im Azure-Portal angezeigt wird. Die Clusteradministrator-Azure AD-Gruppe wird auf der Registerkarte **Konfiguration** im Portal angezeigt (oder mit dem Parameternamen `--aad-admin-group-object-ids` in Azure CLI).

| BESCHREIBUNG        | Rollenzuweisung erforderlich| Clusteradministrator-Azure AD-Gruppe(n) | Verwendung |
| -------------------|------------|----------------------------|-------------|
| Legacy-Administratoranmeldung mit Clientzertifikat| **Azure Kubernetes-Administratorrolle**. Diese Rolle ermöglicht die Verwendung von `az aks get-credentials` mit dem `--admin`-Flag, das ein [Legacy-Clusteradministratorzertifikat (nicht Azure AD)](control-kubeconfig-access.md) in `.kube/config` des Benutzers herunterlädt. Dies ist der einzige Zweck der „Azure Kubernetes-Administratorrolle“.|–|Wenn Sie permanent blockiert werden, indem Sie keinen Zugriff auf eine gültige Azure AD-Gruppe mit Zugriff auf Ihren Cluster haben.| 
| Azure AD mit manuellen (Cluster-)Rollenbindungen| **Azure Kubernetes-Benutzerrolle**. Mit der Rolle „Benutzer“ kann `az aks get-credentials` ohne `--admin`-Flag verwendet werden. (Dies ist der einzige Zweck der „Azure Kubernetes-Benutzerrolle“.) Das Ergebnis ist in einem Azure AD-fähigen Cluster das Herunterladen [eines leeren Eintrags](control-kubeconfig-access.md) in `.kube/config`, der bei erstmaliger Verwendung durch `kubectl` die browserbasierte Authentifizierung auslöst.| Der Benutzer befindet sich nicht in einer dieser Gruppen. Da der Benutzer in keiner Gruppe von Clusteradministratoren vorhanden ist, werden seine Rechte vollständig durch beliebige Rollenbindungen oder Clusterrollenbindungen gesteuert, die von Clusteradministratoren eingerichtet wurden. Die (Cluster-)Rollenbindungen [nominieren Azure AD-Benutzer oder Azure AD-Gruppen](azure-ad-rbac.md) als ihre `subjects`. Wenn solche Bindungen nicht eingerichtet wurden, kann der Benutzer keine `kubectl`-Befehle ausführen.|Wenn Sie eine differenzierte Zugriffssteuerung wünschen und Azure RBAC nicht für die Kubernetes-Autorisierung verwenden. Beachten Sie, dass der Benutzer, der die Bindungen einrichtet, sich mit einer der anderen Methoden anmelden muss, die in dieser Tabelle aufgeführt sind.|
| Azure AD von Mitglied der Gruppe „Administratoren“| Wie oben|Der Benutzer ist Mitglied einer der hier aufgelisteten Gruppen. AKS generiert automatisch eine Clusterrollenbindung, die alle aufgelisteten Gruppen an die Kubernetes-Rolle `cluster-admin` bindet. Daher können Benutzer in diesen Gruppen alle `kubectl`-Befehle als `cluster-admin` ausführen.|Wenn Sie Benutzern komfortabel vollständige Administratorrechte erteilen möchten und _nicht_ Azure RBAC für die Kubernetes-Autorisierung verwenden.|
| Azure AD mit Azure RBAC für Kubernetes-Autorisierung|Zwei Rollen: Erstens **Azure Kubernetes-Benutzerrolle** (wie oben). Zweitens eine der oben aufgeführten „Azure Kubernetes Service **RBAC**“-Rollen oder Ihre eigene benutzerdefinierte Alternative.|Das Feld „Administratorrollen“ auf der Registerkarte „Konfiguration“ ist irrelevant, wenn Azure RBAC für die Kubernetes-Autorisierung aktiviert ist.|Sie verwenden Azure RBAC für die Kubernetes-Autorisierung. Dieser Ansatz ermöglicht Ihnen eine differenzierte Steuerung, ohne Rollenbindungen oder Clusterrollenbindungen einrichten zu müssen.|

## <a name="next-steps"></a>Nächste Schritte

- Die ersten Schritte mit Azure AD und der rollenbasierten Zugriffssteuerung von Kubernetes sind unter [Integrieren von Azure Active Directory in AKS][aks-aad] beschrieben.
- Zugehörige bewährte Methoden finden Sie unter [Best Practices für die Authentifizierung und Autorisierung in Azure Kubernetes Service (AKS)][operator-best-practices-identity].
- Informationen zu den ersten Schritten mit Azure RBAC für die Kubernetes-Autorisierung finden Sie im [Artikel zum Autorisieren des Zugriffs im AKS-Cluster (Azure Kubernetes Service) mit Azure RBAC](manage-azure-rbac.md).
- Informationen zum Schützen der kubeconfig-Datei finden Sie unter [Definieren des Zugriffs auf die Kubernetes-Konfigurationsdatei in Azure Kubernetes Service (AKS) mithilfe der rollenbasierten Zugriffssteuerung von Azure](control-kubeconfig-access.md).

Weitere Informationen zu den wesentlichen Konzepten von Kubernetes und AKS finden Sie in den folgenden Artikeln:

- [Kubernetes/AKS: Cluster und Workloads][aks-concepts-clusters-workloads]
- [Kubernetes/AKS: Sicherheit][aks-concepts-security]
- [Kubernetes/AKS: virtuelle Netzwerke][aks-concepts-network]
- [Kubernetes/AKS: Speicher][aks-concepts-storage]
- [Kubernetes/AKS: Skalierung][aks-concepts-scale]

<!-- LINKS - External -->
[kubernetes-authentication]: https://kubernetes.io/docs/reference/access-authn-authz/authentication
[webhook-token-docs]: https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - Internal -->
[openid-connect]: ../active-directory/develop/v2-protocols-oidc.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[azure-rbac]: ../role-based-access-control/overview.md
[aks-aad]: managed-aad.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[operator-best-practices-identity]: operator-best-practices-identity.md
[upgrade-per-cluster]: ../azure-monitor/insights/container-insights-update-metrics.md#upgrade-per-cluster-using-azure-cli