---
title: Konfigurieren von Bot-Schutz für Web Application Firewall mit Azure Front Door (Vorschau)
description: Hier erfahren Sie, wie Sie eine Bot-Schutzregel in Azure Web Application Firewall (WAF) für Front Door über das Azure-Portal konfigurieren.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 2357c51f47bcb9bd8bbc6c408cb6d8edbab4d10e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91267005"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Konfigurieren von Bot-Schutz für Web Application Firewall (Vorschau)
In diesem Artikel erfahren Sie, wie Sie eine Bot-Schutzregel in Azure Web Application Firewall (WAF) für Front Door über das Azure-Portal konfigurieren. Die Bot-Schutzregel kann auch über die Befehlszeilenschnittstelle, mit Azure PowerShell oder einer Azure Resource Manager-Vorlage konfiguriert werden.

> [!IMPORTANT]
> Bot-Schutzregelsatz befindet sich derzeit in der öffentlichen Vorschau und wird mit einer Vorschau-SLA (Vereinbarung zum Servicelevel) bereitgestellt. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.  Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie eine grundlegende WAF-Richtlinie für Front Door, indem Sie die Anweisungen ausführen, die unter [Erstellen einer WAF-Richtlinie für Azure Front Door im Azure-Portal](waf-front-door-create-portal.md) beschrieben sind.

## <a name="enable-bot-protection-rule-set"></a>Aktivieren des Bot-Schutzregelsatzes

Suchen Sie beim Erstellen einer Web Application Firewall-Richtlinie auf der Seite **Verwaltete Regeln** zuerst den Abschnitt **Verwalteter Regelsatz**, aktivieren Sie das Kontrollkästchen vor der Regel **Microsoft_BotManager_1 0** im Dropdownmenü, und wählen Sie dann **Überprüfen und erstellen** aus.

   ![Bot-Schutzregel](.././media/waf-front-door-configure-bot-protection/botmanager112019.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [WAF überwachen](waf-front-door-monitor.md).
