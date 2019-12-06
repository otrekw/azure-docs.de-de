---
title: Konfigurieren von Bot-Schutz für WAF mit Azure Front Door (Vorschau)
description: Erfahren Sie mehr über Web Application Firewall (WAF).
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 95660f764d28172ecb55a4952b785fea5f2aa4bb
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186714"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Konfigurieren von Bot-Schutz für Web Application Firewall (Vorschau)
In diesem Artikel wird erläutert, wie Sie Bot-Schutzregeln in Azure Web Application Firewall (WAF) für Front Door über die Azure-Befehlszeilenschnittstelle, Azure PowerShell oder eine Azure Resource Manager-Vorlage konfigurieren.

Ein verwalteter Bot-Schutzregelsatz kann für Ihre WAF aktiviert werden, damit bei Anforderungen von IP-Adressen, die als schädlich bekannt sind, benutzerdefinierte Aktionen ausgeführt werden können. Die IP-Adressen stammen aus dem Microsoft Threat Intelligence-Feed. Microsoft Threat Intelligence basiert auf [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) und wird von mehreren Diensten einschließlich Azure Security Center verwendet.

> [!IMPORTANT]
> Bot-Schutzregelsatz befindet sich derzeit in der öffentlichen Vorschau und wird mit einer Vorschau-SLA (Vereinbarung zum Servicelevel) bereitgestellt. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.  Weitere Informationen finden Sie unter [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie eine grundlegende WAF-Richtlinie für Front Door, indem Sie die Anweisungen ausführen, die unter [Erstellen einer WAF-Richtlinie für Azure Front Door im Azure-Portal](waf-front-door-create-portal.md) beschrieben sind.

## <a name="enable-bot-protection-rule-set"></a>Aktivieren des Bot-Schutzregelsatzes

1. Klicken Sie auf der Seite für die grundlegende Richtlinie, die Sie im vorherigen Abschnitt erstellt haben, unter **Einstellungen** auf **Regeln**.
2. Aktivieren Sie auf der Seite mit den Details unter dem Abschnitt **Regeln verwalten** im Dropdownmenü das Kontrollkästchen vor der Regel **BotProtection-preview-0.1**, und wählen Sie dann **Speichern** darüber aus.
    
   ![Bot-Schutzregel](.././media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [WAF überwachen](waf-front-door-monitor.md).
