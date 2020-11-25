---
title: Unternehmenssicherheit und -governance
titleSuffix: Azure Machine Learning
description: 'Verwenden Sie Azure Machine Learning sicher: Authentifizierung, Autorisierung, Netzwerksicherheit, Datenverschlüsselung und Überwachung.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/09/2020
ms.openlocfilehash: fb1f1d098970927ba04c840e77ec0a0b8d76ca02
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94561317"
---
# <a name="enterprise-security-and-governance-for-azure-machine-learning"></a>Sicherheit und Governance in Unternehmen für Azure Machine Learning

In diesem Artikel lernen Sie Sicherheitsfeatures kennen, die für Azure Machine Learning zur Verfügung stehen.

Wenn Sie einen Clouddienst verwenden, empfiehlt es sich, den Zugriff auf die Benutzer zu beschränken, die ihn benötigen. Als Erstes sollten Sie dabei das vom Dienst verwendete Authentifizierungs- und Autorisierungsmodell kennenlernen. Ggf. ist es auch erforderlich, den Zugriff auf das Netzwerk einzuschränken oder Ressourcen in Ihrem lokalen Netzwerk sicher mit der Cloud zu verknüpfen. Datenverschlüsselung ist auch von zentraler Bedeutung, sowohl im Ruhezustand als auch während des Verschiebens der Daten zwischen Diensten. Möglicherweise möchten Sie auch Richtlinien zur Erzwingung bestimmter Konfigurationen erstellen oder protokollieren, wenn nicht konforme Konfigurationen erstellt werden. Schließlich müssen Sie in der Lage sein, den Dienst zu überwachen und ein Überwachungsprotokoll aller Aktivitäten zu erstellen.

> [!NOTE]
> Die Informationen in diesem Artikel arbeiten mit dem Azure Machine Learning Python-SDK Version 1.0.83.1 oder höher.

## <a name="authentication--authorization"></a>Authentifizierung und Autorisierung

Bei der Authentifizierung bei Azure Machine Learning-Ressourcen werden meistens Azure Active Directory (Azure AD) für die Authentifizierung und die rollenbasierte Zugriffssteuerung (Azure RBAC) für die Autorisierung verwendet. Dabei gibt es die folgenden Ausnahmen:

* __SSH__: Sie können den SSH-Zugriff auf einige Computeressourcen wie die Azure Machine Learning-Computeinstanz aktivieren. Für den SSH-Zugriff wird die schlüsselbasierte Authentifizierung verwendet. Weitere Informationen zum Erstellen von SSH-Schlüsseln finden Sie unter [Erstellen und Verwalten von SSH-Schlüsseln](../virtual-machines/linux/create-ssh-keys-detailed.md). Informationen zum Aktivieren des SSH-Zugriffs finden Sie unter [Erstellen und Verwalten einer Azure Machine Learning-Computeinstanz](how-to-create-manage-compute-instance.md).
* __Als Webdienste bereitgestellte Modelle__: Webdienstbereitstellungen können __schlüssel__- oder __token__ basierte Zugriffssteuerung verwenden. Schlüssel sind statische Zeichenfolgen. Token werden mithilfe eines Azure AD-Kontos abgerufen. Weitere Informationen finden Sie unter [Konfigurieren der Authentifizierung für Modelle, die als Webdienst bereitgestellt werden](how-to-authenticate-web-service.md).

Bestimmte Dienste, von denen Azure Machine Learning abhängig ist, z. B. Azure-Datenspeicherdienste, verfügen über eigene Authentifizierungs- und Autorisierungsmethoden. Weitere Informationen zur Speicherdienstauthentifizierung finden Sie unter [Herstellen einer Verbindung mit Speicherdiensten](how-to-access-data.md).

### <a name="azure-ad-authentication"></a>Azure AD-Authentifizierung

Die mehrstufige Authentifizierung wird unterstützt, wenn Azure Active Directory (Azure AD) dafür konfiguriert ist. So verläuft der Authentifizierungsprozess:

1. Der Client meldet sich bei Azure AD an und ruft das Azure Resource Manager-Token ab.  Benutzer und Dienstprinzipale werden vollständig unterstützt.
1. Der Client übergibt das Token an Azure Resource Manager und an Azure Machine Learning.
1. Machine Learning Service stellt ein Machine Learning Service-Token für das Benutzercomputeziel bereit (z. B. Machine Learning Compute). Dieses Token wird nach Abschluss der Ausführung vom Benutzercomputeziel für einen erneuten Aufruf von Machine Learning Service verwendet. Der Bereich ist auf den Arbeitsbereich beschränkt.

[![Authentifizierung in Azure Machine Learning](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication.png#lightbox)

Weitere Informationen finden Sie unter [Authentifizierung für einen Azure Machine Learning-Arbeitsbereich](how-to-setup-authentication.md).

### <a name="azure-rbac"></a>Azure RBAC

Sie können mehrere Arbeitsbereiche erstellen, und jeder Arbeitsbereich kann von mehreren Benutzern gemeinsam genutzt werden. Sie können steuern, auf welche Features oder Vorgänge des Arbeitsbereichs Benutzer zugreifen können, indem Sie deren Azure AD-Konto den Azure RBAC-Rollen zuweisen. Folgende Rollen sind integriert:

* Besitzer
* Mitwirkender
* Leser

Besitzer und Mitwirkende können alle Computeziele und Datenspeicher verwenden, die dem Arbeitsbereich angefügt sind.  

Die folgende Tabelle enthält einige der wichtigsten Azure Machine Learning-Vorgänge und die Rollen, von denen sie ausgeführt werden können:

| Azure Machine Learning-Vorgang | Besitzer | Mitwirkender | Leser |
| ---- |:----:|:----:|:----:|
| Arbeitsbereich erstellen | ✓ | ✓ | |
| Freigeben des Arbeitsbereichs | ✓ | |  |
| Erstellen eines Computeziels | ✓ | ✓ | |
| Anfügen eines Computeziels | ✓ | ✓ | |
| Anfügen von Datenspeichern | ✓ | ✓ | |
| Ausführen des Experiments | ✓ | ✓ | |
| Ausführungen/Metriken anzeigen | ✓ | ✓ | ✓ |
| Registrieren des Modells | ✓ | ✓ | |
| Image erstellen | ✓ | ✓ | |
| Bereitstellen des Webdiensts | ✓ | ✓ | |
| Modelle/Images anzeigen | ✓ | ✓ | ✓ |
| Webdienst aufrufen | ✓ | ✓ | ✓ |

Wenn die integrierten Rollen Ihren Anforderungen nicht entsprechen, können Sie benutzerdefinierte Rollen erstellen. Benutzerdefinierte Rollen steuern alle Vorgänge in einem Arbeitsbereich, z. B. das Erstellen einer Compute-Instanz, das Übermitteln einer Ausführung, das Registrieren eines Datenspeichers oder das Bereitstellen eines Modells. Benutzerdefinierte Rollen können über Lese-, Schreib- oder Löschberechtigungen für die verschiedenen Ressourcen eines Arbeitsbereichs verfügen, z. B. Cluster, Datenspeicher, Modelle und Endpunkte. Sie können die Rolle auf einer bestimmten Arbeitsbereichsebene, einer bestimmten Ressourcengruppenebene oder einer bestimmten Abonnementebene verfügbar machen. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf einen Azure Machine Learning-Arbeitsbereich](how-to-assign-roles.md).

> [!IMPORTANT]
> Azure Machine Learning ist von anderen Azure-Diensten wie Azure Blob Storage und Azure Kubernetes Services abhängig. Jeder Azure-Dienst verfügt über eigene Azure RBAC-Konfigurationen. Für eine Zugriffssteuerung im gewünschten Umfang müssen Sie möglicherweise die Azure RBAC-Konfigurationen sowohl für Azure Machine Learning als auch für die Dienste anwenden, die mit Azure Machine Learning verwendet werden.

> [!WARNING]
> Azure Machine Learning wird mit der Azure Active Directory-Business-to-Business-Zusammenarbeit unterstützt, jedoch derzeit noch nicht mit der Azure Active Directory-Business-to-Consumer-Zusammenarbeit.

### <a name="managed-identities"></a>Verwaltete Identitäten

Jedem Arbeitsbereich ist außerdem eine vom System zugewiesene [verwaltete Identität](../active-directory/managed-identities-azure-resources/overview.md) zugeordnet, die den gleichen Namen wie der Arbeitsbereich trägt. Die verwaltete Identität wird für den sicheren Zugriff auf Ressourcen verwendet, die vom Arbeitsbereich verwendet werden. Sie verfügt über die folgenden Berechtigungen für angefügte Ressourcen:

| Resource | Berechtigungen |
| ----- | ----- |
| Arbeitsbereich | Mitwirkender |
| Speicherkonto | Mitwirkender an Storage-Blobdaten |
| Schlüsseltresor | Zugriff auf alle Schlüssel, Geheimnisse und Zertifikate |
| Azure Container Registry | Mitwirkender |
| Ressourcengruppe, die den Arbeitsbereich enthält | Mitwirkender |
| Ressourcengruppe, die den Schlüsseltresor enthält (wenn sie nicht mit der Ressourcengruppe identisch ist, die den Arbeitsbereich enthält) | Mitwirkender |

Administratoren sollten den Zugriff der verwalteten Identität auf die in der vorhergehenden Tabelle genannten Ressourcen nicht widerrufen. Sie können den Zugriff mithilfe des Vorgangs zum erneuten Synchronisieren von Schlüsseln wiederherstellen.

Azure Machine Learning erstellt eine weitere Anwendung (deren Name mit `aml-` oder `Microsoft-AzureML-Support-App-` beginnt) mit Zugriff auf Mitwirkendenebene in Ihrem Abonnement für jede Region des Arbeitsbereichs. Wenn Sie z. B. im selben Abonnement über einen Arbeitsbereich in „USA, Osten“ und einen anderen in „Europa, Norden“ verfügen, sehen Sie zwei dieser Anwendungen. Azure Machine Learning kann Sie mithilfe dieser Anwendungen beim Verwalten der Computeressourcen unterstützen.

Optional können Sie Ihre eigenen verwalteten Identitäten für die Verwendung mit Azure Virtual Machines und Azure Machine Learning-Computecluster konfigurieren. Bei einer VM kann die verwaltete Identität verwendet werden, um über das SDK anstatt das Azure AD-Konto eines einzelnen Benutzers auf Ihren Arbeitsbereich zuzugreifen. Bei einem Computecluster wird die verwaltete Identität verwendet, um auf Ressourcen wie gesicherte Datenspeicher zuzugreifen, auf die der Benutzer, der den Trainingsauftrag durchführt, möglicherweise keinen Zugriff hat. Weitere Informationen finden Sie unter [Authentifizierung für einen Azure Machine Learning-Arbeitsbereich](how-to-setup-authentication.md).

## <a name="network-security-and-isolation"></a>Netzwerksicherheit und -isolation

Um den physischen Zugriff auf Azure Machine Learning-Ressourcen zu beschränken, können Sie Azure Virtual Network (VNET) verwenden. Mit VNETs können Sie Netzwerkumgebungen erstellen, die teilweise oder vollständig vom öffentlichen Internet isoliert sind. Dadurch werden die Angriffsfläche für die Lösung und die Möglichkeit einer Datenexfiltration reduziert.

Weitere Informationen finden Sie in den folgenden Dokumenten:

* [Übersicht zu Isolation und Datenschutz bei virtuellen Netzwerken](how-to-network-security-overview.md)
* [Schützen von Arbeitsbereichsressourcen](how-to-secure-workspace-vnet.md)
* [Schützen der Trainingsumgebung](how-to-secure-training-vnet.md)
* [Schützen der Rückschlussumgebung](how-to-secure-inferencing-vnet.md)
* [Verwenden von Azure Machine Learning Studio in einem virtuellen Netzwerk](how-to-enable-studio-virtual-network.md)

<a id="encryption-at-rest"></a><a id="azure-blob-storage"></a>

## <a name="data-encryption"></a>Datenverschlüsselung

Azure Machine Learning verwendet verschiedene Computeressourcen und Datenspeicher. Weitere Informationen zu deren Unterstützung der Verschlüsselung von Daten im Ruhezustand und während der Übertragung finden Sie unter [Datenverschlüsselung mit Azure Machine Learning](concept-data-encryption.md).

### <a name="microsoft-generated-data"></a>Von Microsoft generierte Daten

Bei der Verwendung von Diensten wie automatisiertes ML generiert Microsoft möglicherweise vorübergehende, vorverarbeitete Daten für das Training mehrerer Modelle. Diese Daten werden in einem Datenspeicher in Ihrem Arbeitsbereich gespeichert, der es Ihnen ermöglicht, die Zugriffssteuerung und Verschlüsselung entsprechend durchzusetzen.

Möglicherweise möchten Sie auch [Diagnoseinformationen verschlüsseln, die von Ihrem bereitgestellten Endpunkt](how-to-enable-app-insights.md) in Ihrer Azure Application Insights-Instanz protokolliert werden.

## <a name="monitoring"></a>Überwachung

Bei Azure Machine Learning sind je nach Rolle und überwachter Komponente verschiedene Überwachungsszenarien möglich.

| Role | Zu verwendende Überwachung | Beschreibung |
| ---- | ----- | ----- |
| Admin, DevOps, MLOps | [Azure Monitor-Metriken](#azure-monitor), [Aktivitätsprotokoll](#activity-log), [Überprüfung auf Sicherheitsrisiken](#vulnerability-scanning) | Servicelevelinformationen |
| Data Scientist, MLOps | [Überwachen von Ausführungen](#monitor-runs) | Informationen, die bei Trainingsausführungen protokolliert werden |
| MLOps | [Sammeln von Modelldaten](how-to-enable-data-collection.md), [Überwachen mit Application Insights](how-to-enable-app-insights.md) | Informationen, die von Modellen protokolliert werden, die als Webdienste oder IoT Edge-Module bereitgestellt werden|

### <a name="monitor-runs"></a>Überwachen von Ausführungen

Sie können die Ausführung von Experimenten in Azure Machine Learning, einschließlich der Protokollierungsinformationen, aus den Trainingsskripts überwachen. Diese Informationen können über das SDK, die Azure CLI und Studio angezeigt werden. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Starten, Überwachen und Abbrechen von Trainingsausführungen in Python](how-to-manage-runs.md)
* [Aktivieren von Protokollen](how-to-track-experiments.md)
* [Protokoll anzeigen...](how-to-monitor-view-training-logs.md)
* [Visualisieren von Experimentausführungen und -metriken mit TensorBoard und Azure Machine Learning](how-to-monitor-tensorboard.md)

### <a name="azure-monitor"></a>Azure Monitor

Sie können Azure Monitor-Metriken verwenden, um Metriken für Ihren Azure Machine Learning-Arbeitsbereich anzuzeigen und zu überwachen. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Arbeitsbereich und anschließend **Metriken** aus:

[![Screenshot, der Beispielmetriken für einen Arbeitsbereich zeigt](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Die Metriken umfassen Informationen zu Ausführungen, Bereitstellungen und Registrierungen.

Weitere Informationen finden Sie unter [Azure Monitor-Metriken](../azure-monitor/platform/data-platform-metrics.md).

### <a name="activity-log"></a>Aktivitätsprotokoll

Im Aktivitätsprotokoll eines Arbeitsbereichs können Sie verschiedene Vorgänge anzeigen, die im Arbeitsbereich ausgeführt werden. Das Protokoll enthält grundlegende Informationen wie den Vorgangsnamen, den Ereignisinitiator und den Zeitstempel.

Der folgende Screenshot zeigt das Aktivitätsprotokoll eines Arbeitsbereichs:

[![Screenshot, der das Aktivitätsprotokoll eines Arbeitsbereichs zeigt](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Details der Bewertungsanforderung werden in Application Insights gespeichert. Application Insights wird in Ihrem Abonnement erstellt, wenn Sie einen Arbeitsbereich erstellen. Zu den protokollierten Informationen zählen Felder wie:

* HTTPMethod
* UserAgent
* ComputeType
* RequestUrl
* StatusCode
* RequestId
* Duration

> [!IMPORTANT]
> Einige Aktionen im Azure Machine Learning-Arbeitsbereich protokollieren keine Informationen im Aktivitätsprotokoll. Der Start einer Trainingsausführung und die Registrierung eines Modells werden beispielsweise nicht protokolliert.
>
> Einige dieser Aktionen werden im Bereich **Aktivitäten** Ihres Arbeitsbereichs angezeigt, diese Benachrichtigungen geben jedoch nicht an, wer die Aktivität initiiert hat.

### <a name="vulnerability-scanning"></a>Überprüfung auf Sicherheitsrisiken

Azure Security Center bietet einheitliche Funktionen für die Sicherheitsverwaltung und den erweiterten Schutz vor Bedrohungen für Hybrid Cloud-Workloads. Für Azure Machine Learning sollten Sie die Überprüfung der Azure Container Registry-Ressourcen sowie der Azure Kubernetes Service-Ressourcen aktivieren. Informationen finden Sie unter [Einführung in Azure Defender für Containerregistrierungen](../security-center/defender-for-container-registries-introduction.md) und [Einführung in Azure Defender für Kubernetes](../security-center/defender-for-kubernetes-introduction.md).

## <a name="audit-and-manage-compliance"></a>Überwachen und Verwalten der Compliance

[Azure Policy](../governance/policy/index.yml) ist ein Governancetool, mit dem Sie sicherstellen können, dass Azure-Ressourcen mit Ihren Richtlinien konform sind. Mit Azure Machine Learning können Sie die folgenden Richtlinien zuweisen:

* **Kundenseitig verwalteter Schlüssel**: Überwachen oder erzwingen Sie, ob Arbeitsbereiche einen kundenseitig verwalteten Schlüssel verwenden müssen.
* **Private Link**: Überwachen Sie, ob Arbeitsbereiche mithilfe eines privaten Endpunkts mit einem virtuellen Netzwerk kommunizieren.

Weitere Informationen zu Azure Policy finden Sie in der [Dokumentation zu Azure Policy](../governance/policy/overview.md).

Weitere Informationen zu den Richtlinien, die speziell für Azure Machine Learning gelten, finden Sie unter [Überwachen und Verwalten der Compliance mit Azure Policy](how-to-integrate-azure-policy.md).

## <a name="resource-locks"></a>Ressourcensperren

[!INCLUDE [resource locks](../../includes/machine-learning-resource-lock.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Sichere Azure Machine Learning-Webdienste mit TLS](how-to-secure-web-service.md)
* [Nutzen eines als Webdienst bereitgestellten Machine Learning-Modells](how-to-consume-web-service.md)
* [Verwenden von Azure Machine Learning hinter Azure Firewall](how-to-access-azureml-behind-firewall.md)
* [Verwenden von Azure Machine Learning mit einem virtuellen Azure-Netzwerk](how-to-network-security-overview.md)
* [Datenverschlüsselung von ruhenden Daten und während der Übertragung](concept-data-encryption.md)
* [Erstellen einer Echtzeitempfehlungs-API in Azure](/azure/architecture/reference-architectures/ai/real-time-recommendation)
