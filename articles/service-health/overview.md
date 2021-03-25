---
title: Was ist Azure Service Health?
description: Personalisierte Informationen dazu, wie Ihre Azure-Apps von aktuellen und zukünftigen Problemen und Wartungen der Azure-Dienste betroffen sind.
ms.topic: overview
ms.date: 05/10/2019
ms.openlocfilehash: cb1eb8eb75a18a73e8f024f02217df37f45981fe
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87499771"
---
# <a name="what-is-azure-service-health"></a>Was ist Azure Service Health?

Azure stellt eine Reihe von Benutzeroberflächen bereit, in denen Sie ständig über den Zustand Ihrer Cloudressourcen informiert werden. Diese Informationen beinhalten aktuelle und zukünftige Probleme, etwa Ereignisse, die sich auf die Dienstverfügbarkeit auswirken, geplante Wartung und andere Änderungen, die die Verfügbarkeit Ihres Systems beeinflussen können.

Azure Service Health ist eine Kombination aus drei getrennten kleineren Diensten.

[Azure-Status](azure-status-overview.md) informiert Sie über Dienstausfälle in Azure auf der **[Seite „Azure-Status“](https://status.azure.com)** . Die Seite ist eine globale Ansicht der Integrität von allen Azure-Diensten in allen Azure-Regionen. Die Statusseite ist eine gute Referenz für Vorfälle mit umfangreichen Auswirkungen. Aktuellen Azure-Benutzer wird jedoch dringend empfohlen, Azure Service Health zu nutzen, um über Azure-Vorfälle und -Wartungen auf dem Laufenden zu bleiben.

[Service Health](service-health-overview.md) bietet eine personalisierte Darstellung der Integrität der von Ihnen verwendeten Azure-Dienste und -Regionen. In Service Health finden Sie beispielsweise Informationen zu Ausfällen und geplanten Wartungen sowie andere Integritätsempfehlungen, da die authentifizierte Service Health-Umgebung weiß, welche Dienste und Ressourcen Sie derzeit verwenden. Um Service Health optimal nutzen zu können, wird die Einrichtung von Service Health-Warnungen empfohlen, die Sie über Ihren bevorzugten Kommunikationskanal informieren, wenn Dienstprobleme, geplante Wartungen oder andere Änderungen möglicherweise die von Ihnen verwendeten Azure-Dienste und -Regionen beeinträchtigen.

[Resource Health](resource-health-overview.md) enthält Informationen zur Integrität von einzelnen Cloudressourcen, etwa eine bestimmte Instanz eines virtuellen Computers. Mithilfe von Azure Monitor können Sie darüber hinaus Warnungen konfigurieren, die Sie über Änderungen der Verfügbarkeit Ihrer Cloudressourcen informieren. Dank Benachrichtigungen von Resource Health und Azure Monitor sind Sie jederzeit bestens über die Verfügbarkeit Ihrer Ressourcen informiert und können schnell erkennen, ob ein Problem auf einen Fehler auf Ihrer Seite oder auf ein Ereignis der Azure-Plattform zurückzuführen ist.

Zusammen bieten Ihnen diese Oberflächen einen umfassenden Einblick in die Integrität von Azure mit der Granularität, die für Sie relevant ist.

**Sehen Sie sich eine Übersicht über die Seite „Azure-Status“ sowie über Azure Service Health und Azure Resource Health an.**

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OgX6]

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]