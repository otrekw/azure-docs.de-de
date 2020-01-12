---
title: 'Bedrohungserkennung für die Azure-Dienstebene: Azure Security Center'
description: In diesem Thema werden die in Azure Security Center verfügbaren Warnungen für die Azure-Dienstebene vorgestellt.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 33c45447-3181-4b75-aa8e-c517e76cd50d
ms.service: security-center
ms.topic: conceptual
ms.date: 08/25/2019
ms.author: memildin
ms.openlocfilehash: d4033989830323856ac14ed06eea7df74806f128
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75665708"
---
# <a name="threat-detection-for-the-azure-service-layer-in-azure-security-center"></a>Bedrohungserkennung für die Azure-Dienstebene in Azure Security Center

In diesem Thema werden die Azure Security Center-Warnungen vorgestellt, die bei der Überwachung der folgenden Azure-Dienstebenen zur Verfügung stehen:

* [Azure-Netzwerkebene](#network-layer)
* [Azure-Verwaltungsebene (Azure Resource Manager) (Vorschauversion)](#management-layer)
* [Azure Key Vault](#azure-keyvault)

## Azure-Netzwerkebene<a name="network-layer"></a>

Die Netzwerkebenenanalysen von Security Center basieren auf exemplarischen [IPFIX-Daten](https://en.wikipedia.org/wiki/IP_Flow_Information_Export) (durch Azure-Kernrouter erfasste Paketheader). Auf der Grundlage dieses Datenfeeds werden schädliche Datenverkehrsaktivitäten von Security Center durch Machine Learning-Modelle identifiziert und gekennzeichnet. Security Center nutzt zudem die Threat Intelligence-Datenbank von Microsoft zur Anreicherung von IP-Adressen.

Einige Netzwerkkonfigurationen schränken Security Center möglicherweise in der Erstellung von Warnungen bei verdächtiger Netzwerkaktivität ein. Damit Security Center Netzwerkwarnungen generiert, stellen Sie Folgendes sicher:

- Ihr virtueller Computer verfügt über eine öffentliche IP-Adresse (oder befindet sich auf einem Load Balancer mit einer öffentlichen IP-Adresse).

- Der ausgehende Netzwerkdatenverkehr des virtuellen Computers wird nicht durch eine externe IDS-Lösung blockiert.

- Ihrem virtuellen Computer wurde für die gesamte Stunde, in der die verdächtige Kommunikation stattgefunden hat, dieselbe IP-Adresse zugewiesen. Dies gilt auch für als Teil eines verwalteten Diensts (z. b. AKS, Databricks) erstellte virtuelle Computer.

Eine Liste der Warnungen der Azure-Netzwerkebene finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azurenetlayer).

Unter [Heuristic DNS detections in Azure Security Center](https://azure.microsoft.com/blog/heuristic-dns-detections-in-azure-security-center/) (Heuristische DNS-Erkennungen in Security Center) finden Sie Informationen dazu, wie Security Center netzwerkbezogene Signale für den Bedrohungsschutz verwenden kann.


## Azure-Verwaltungsebene (Azure Resource Manager) (Vorschauversion)<a name ="management-layer"></a>

Die auf Azure Resource Manager basierende Sicherheitsstufe von Security Center befindet sich derzeit in der Vorschauphase.

Security Center nutzt Azure Resource Manager-Ereignisse (also gewissermaßen die Steuerungsebene von Azure), um eine zusätzliche Schutzebene zu bieten. Durch die Analyse der Azure Resource Manager-Datensätze erkennt Security Center ungewöhnliche bzw. potenziell schädliche Vorgänge in der Azure-Abonnementumgebung.

Eine Liste der Warnungen von Azure Resource Manager (Vorschau) finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azureresourceman).



>[!NOTE]
> Einige der oben aufgeführten Analysen basieren auf Microsoft Cloud App Security. Um von diesen Analysen zu profitieren, müssen Sie eine Cloud App Security-Lizenz aktivieren. Wenn Sie über eine Cloud App Security-Lizenz verfügen, sind diese Warnungen standardmäßig aktiviert. So können Sie sie deaktivieren:
>
> 1. Wählen Sie auf dem Blatt **Security Center** die Option **Sicherheitsrichtlinie** aus. Wählen Sie für das Abonnement, das Sie ändern möchten, **Einstellungen bearbeiten** aus.
> 2. Wählen Sie **Bedrohungserkennung** aus.
> 3. Deaktivieren Sie unter **Integrationen aktivieren** das Kontrollkästchen **Microsoft Cloud App Security Zugriff auf meine Daten erteilen**, und wählen Sie anschließend **Speichern** aus.

>[!NOTE]
>Security Center speichert sicherheitsbezogene Kundendaten im gleichen geografischen Raum, in dem sich auch die Ressource befindet. Falls Security Center von Microsoft noch nicht im geografischen Raum der Ressource bereitgestellt wurde, werden die Daten in den USA gespeichert. Wenn Cloud App Security aktiviert ist, werden diese Informationen gemäß den Regeln für geografische Standorte von Cloud App Security gespeichert. Weitere Informationen finden Sie unter [Data storage for non-regional services](https://azuredatacentermap.azurewebsites.net/) (Datenspeicherung für nicht regionale Dienste).

## Azure Key Vault (Vorschau)<a name="azure-keyvault"></a>

Der Azure Key Vault-Clouddienst schützt Verschlüsselungsschlüssel und Geheimnisse (wie Zertifikate, Verbindungszeichenfolgen und Kennwörter). 

Azure Security Center enthält den nativen Azure-Dienst Advanced Threat Protection für Azure Key Vault und bietet eine zusätzliche Ebene der Sicherheitsintelligenz. Security Center erkennt ungewöhnliche und potenziell schädliche Versuche, auf Key Vault-Konten zuzugreifen oder diese missbräuchlich zu nutzen. Aufgrund dieser Schutzebene können Sie Bedrohungen begegnen, ohne dass Sie ein Sicherheitsexperte sein oder Systeme von Drittanbietern für die Überwachung der Sicherheit verwalten müssen.  

Wenn anomale Aktivitäten auftreten, zeigt Security Center Warnungen an und sendet diese optional per E-Mail an die Abonnementadministratoren. Diese Warnungen enthalten Details zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Eindämmung von Bedrohungen. 

> [!NOTE]
> Dieser Dienst ist derzeit nicht in Azure Government- und Sovereign Cloud-Regionen verfügbar.

Eine Liste der Azure Key Vault-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azurekv).
