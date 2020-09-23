---
title: Schützen Ihrer Azure App Service-Web-Apps und -APIs
description: Dieser Artikel bietet eine Einführung in den Schutz von Azure App Service-Web-Apps und -APIs in Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e8518710-fcf9-44a8-ae4b-8200dfcded1a
ms.service: security-center
ms.topic: conceptual
ms.date: 01/27/2019
ms.author: memildin
ms.openlocfilehash: ec3fb8609612f3920e330da7922fdd1eb8883305
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89459775"
---
# <a name="protect-your-azure-app-service-web-apps-and-apis"></a>Schützen Ihrer Azure App Service-Web-Apps und -APIs

Azure App Service ist eine vollständig verwaltete Plattform zum Entwickeln und Hosting Ihrer Web-Apps und -APIs, wobei Sie sich keine Gedanken über die Verwaltung der Infrastruktur machen müssen. Sie bietet Verwaltung, Überwachung sowie betriebliche Erkenntnisse und erfüllt die Anforderungen auf Unternehmensniveau hinsichtlich Leistung, Sicherheit und Konformität. Weitere Informationen finden Sie unter [Azure App Service](https://azure.microsoft.com/services/app-service/).

Zum Aktivieren von Advanced Threat Protection für Ihren Azure App Service-Plan müssen Sie folgende Schritte ausführen:

* Abonnieren des Standard-Tarifs von Azure Security Center
* Aktivieren Sie den App Service-Plan wie unten veranschaulicht. Das Security Center ist nativ in den App Service integriert, wodurch die Notwendigkeit von Bereitstellung und Onboarding entfällt – die Integration ist transparent.
* Sie verfügen über einen App Service-Plan, der dedizierten Computern zugeordnet ist. Unterstützte Pläne: Basic, Standard, Premium, Isolated oder Linux. Security Center unterstützt keine Free-, Shared- oder Consumption-Pläne. Weitere Informationen finden Sie unter [App Service-Pläne](https://azure.microsoft.com/pricing/details/app-service/plans/).

Mit dem aktivierten App Service-Plan bewertet Security Center die von Ihrem App Service-Plan abgedeckten Ressourcen und generiert auf der Grundlage der Erkenntnisse Sicherheitsempfehlungen. Security Center schützt die VM-Instanz, in der Ihr App Service ausgeführt wird, und die Verwaltungsoberfläche. Es überwacht auch Anforderungen und Antworten, die an und von Ihren Apps gesendet werden, die im App Service ausgeführt werden.

Security Center nutzt die Skalierung der Cloud und die Sichtbarkeit von Azure als Cloudanbieter, um allgemeine Angriffe auf Web-Apps zu überwachen. Security Center kann Angriffe auf Ihre Anwendungen erkennen und neu auftretende Angriffe identifizieren – selbst wenn sich Angreifer noch in der Aufklärungsphase befinden und Schwachstellen in mehreren von Azure gehosteten Anwendungen ermitteln. Als Azure-nativer Dienst ist Security Center auch in der einzigartigen Lage, hostbasierte Sicherheitsanalysen anzubieten, welche die zugrunde liegenden Serverknoten für diese PaaS abdecken und es Security Center ermöglichen, Angriffe auf bereits genutzte Webanwendungen zu erkennen. Ausführliche Informationen finden Sie unter [Bedrohungsschutz für Azure App Service](threat-protection.md#app-services).


## <a name="enable-monitoring-and-protection-of-app-service"></a>Aktivieren der Überwachung und des Schutz von App Service

1. Wählen Sie im Azure-Portal „Security Center“ aus.
2. Navigieren Sie zu **Preise und Einstellungen**, und wählen Sie ein Abonnement aus.
3. Schalten Sie Ihren Tarif unter **Tarif** in der Zeile **App Service** auf **Aktiviert** um.

    [![Aktivieren von App-Diensten in Ihrem Abonnement mit Standard-Tarif](media/security-center-app-services/app-services-toggle.png)](media/security-center-app-services/app-services-toggle.png#lightbox)


>[!NOTE]
> Die Anzahl der Instanzen, die für Ihre **Ressourcenmenge** aufgelistet sind, stellt die Gesamtzahl der Compute-Instanzen in allen App Service-Plänen für dieses Abonnement dar, die zu dem Zeitpunkt ausgeführt wurden, als Sie das Tarifblatt geöffnet haben.
>
> Azure App Service bietet eine Vielzahl von Plänen. Ihr App Service-Plan definiert einen Satz von Computeressourcen für eine auszuführende Web-App. Diese entsprechen Serverfarmen im herkömmlichen Webhosting. Es können eine oder mehrere Apps für die Ausführung auf denselben Computeressourcen (oder in demselben App Service-Plan) konfiguriert werden.
>
>Navigieren Sie zum Bestätigen der Anzahl im Azure-Portal zu „App Service-Pläne“. Dort wird die Anzahl der Compute-Instanzen angegeben, die von den einzelnen Plänen verwendet werden. 






Um die Überwachung und Empfehlungen für Ihren App Service zu deaktivieren, wiederholen Sie diesen Vorgang und schalten Sie Ihren **App Service**-Plan auf **Deaktiviert** um.



## <a name="see-also"></a>Weitere Informationen
In diesem Artikel haben Sie erfahren, wie Sie die Überwachungsfunktionen in Azure Security Center verwenden können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Artikeln:

* [Festlegen von Sicherheitsrichtlinien in Azure Security Center](tutorial-security-policy.md): Erfahren Sie, wie Sie Sicherheitseinstellungen in Azure Security Center konfigurieren.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Hier erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [App Services](security-center-virtual-machine-protection.md#app-services):  Hier können Sie eine Liste Ihrer App Service-Umgebungen mit Integritätszusammenfassungen anzeigen.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Hier erfahren Sie, wie Sie den Integritätsstatus Ihrer Partnerlösungen überwachen.
* [Azure Security-Blog](https://docs.microsoft.com/archive/blogs/azuresecurity/): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.
