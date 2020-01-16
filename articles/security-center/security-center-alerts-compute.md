---
title: Bedrohungserkennung für cloudnatives Computing in Azure Security Center | Microsoft-Dokumentation
description: In diesem Artikel werden die in Azure Security Center verfügbaren Warnungen für cloudnatives Computing vorgestellt.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 5aa5efcf-9f6f-4aa1-9f72-d651c6a7c9cd
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: f2fda893ad84aaf9d11d26d761f5395c7f5650d2
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666371"
---
# <a name="threat-detection-for-cloud-native-computing-in-azure-security-center"></a>Bedrohungserkennung für cloudnatives Computing in Azure Security Center

Als native Lösung ermöglicht Azure Security Center einzigartige Einblicke in interne Protokolle zur Erkennung von Angriffsmethoden über mehrere Ziele hinweg. In diesem Artikel werden die Warnungen vorgestellt, die für folgende Azure-Dienste zur Verfügung stehen:

* [Azure App Service](#app-services)
* [Azure-Container](#azure-containers) 

> [!NOTE]
> Diese Dienste sind derzeit nicht in Azure Government- und Sovereign Cloud-Regionen verfügbar.

## Azure App Service <a name="app-services"></a>

Security Center nutzt die Kapazitäten der Cloud, um Angriffe auf Anwendungen unter App Service zu identifizieren. Angreifer testen Webanwendungen, um Schwachstellen zu suchen und auszunutzen. Bevor Anforderungen für in Azure ausgeführte Anwendungen an bestimmte Umgebungen weitergeleitet werden, durchlaufen sie mehrere Gateways, wo sie jeweils überprüft und protokolliert werden. Diese Daten werden dann verwendet, um Exploits und Angreifer zu identifizieren sowie um neue Muster zu erkennen, die später zur Anwendung kommen.

Dank der Möglichkeiten, über die Azure als Cloudanbieter verfügt, kann Security Center interne App Service-Protokolle analysieren und Angriffsmethoden für mehrere Ziele identifizieren. Die Methodik umfasst beispielsweise großflächige Scans und verteilte Angriffe. Bei dieser Art von Angriff, der in der Regel von einer kleinen Untergruppe von IP-Adressen ausgeht, werden ähnliche Endpunkte auf mehreren Hosts durchforstet. Die Angriffe dienen dazu, anfällige Seiten oder Plug-Ins ausfindig zu machen, und sind aus der Perspektive eines einzelnen Hosts nicht erkennbar.

Security Center hat auch Zugriff auf die zugrunde liegenden Sandboxes und virtuellen Computer. In Kombination mit forensischen Techniken für den Arbeitsspeicher kann die Infrastruktur das gesamte Spektrum erfassen – von neu in Umlauf gebrachten Angriffen bis hin zu kompromittierten Kundencomputern. Auch wenn Security Center bereitgestellt wird, nachdem eine Web-App ausgenutzt wurde, können fortlaufende Angriffe möglicherweise erkannt werden.

Eine Liste der Azure App Service-Warnungen finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-azureappserv).

## Azure-Container <a name="azure-containers"></a>

Security Center bietet eine Echtzeit-Bedrohungserkennung für Ihre Containerumgebungen und generiert Warnungen für verdächtige Aktivitäten. Mit diesen Informationen können Sie schnell Sicherheitsprobleme lösen und die Sicherheit Ihrer Container verbessern.

Bedrohungen auf unterschiedlichen Ebenen werden erkannt: 

* **Hostebene**: der Security Center-Agent (verfügbar im Tarif „Standard“, weitere Informationen unter [Preise](security-center-pricing.md)) überwacht Linux auf verdächtige Aktivitäten. Der Agent löst Warnungen für verdächtige Aktivitäten aus, die aus dem Knoten oder einem darauf ausgeführten Container stammen. Beispiele für derartige Aktivitäten sind Webshell-Erkennung und Verbindungen mit bekannten verdächtigen IP-Adressen.

    Um einen tieferen Einblick in die Sicherheit Ihrer Containerumgebung zu erhalten, überwacht der Agent containerspezifische Analysen. Er löst Warnungen für Ereignisse aus, z.B. die Erstellung privilegierter Container, den verdächtigen Zugriff auf API-Server und Secure Shell (SSH)-Server, die in einem Docker-Container ausgeführt werden.

    >[!NOTE]
    > Wenn Sie die Agents nicht auf Ihren Hosts installieren möchten, kommen Sie nur in den Genuss eines Teils der Vorteile und Warnungen der Bedrohungserkennung. Sie erhalten weiterhin Warnungen im Zusammenhang mit der Netzwerkanalyse und der Kommunikation mit schädlichen Servern.

    Eine Liste der Warnungen auf Hostebene finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-containerhost).


* Für die **AKS-Clusterebene** ist eine Bedrohungserkennung basierend auf der Analyse der Kubernetes-Überwachungsprotokolle vorhanden. Um diese Überwachung **ohne Agents** zu aktivieren, fügen Sie die Kubernetes-Option über die Seite **Preise und Einstellungen** (siehe [Preise](security-center-pricing.md)) zu Ihrem Abonnement hinzu. Zum Generieren von Warnungen auf dieser Ebene überwacht Security Center Ihre von AKS verwalteten Dienste mithilfe der von AKS abgerufenen Protokolle. Beispiele für Ereignisse auf dieser Ebene sind verfügbar gemachte Kubernetes-Dashboards und die Erstellung von Rollen mit hohen Berechtigungen und von sensiblen Einbindungen.

    >[!NOTE]
    > Security Center generiert Erkennungswarnungen für Azure Kubernetes Service-Aktionen und -Bereitstellungen, die nach der Aktivierung der Kubernetes-Option in den Abonnementeinstellungen erfolgen. 

    Eine Liste der Warnungen auf AKS-Clusterebene finden Sie in der [Referenztabelle der Warnungen](alerts-reference.md#alerts-akscluster).

Außerdem wird die Bedrohungslandschaft von unserem globalen Team von Sicherheitsforschern ständig überwacht. Sie fügen containerspezifische Warnungen und Sicherheitsrisiken hinzu, sobald sie erkannt werden.