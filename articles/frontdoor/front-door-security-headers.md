---
title: 'Hinzufügen von Sicherheitsheadern mit Regel-Engines: Azure Front Door'
description: In diesem Artikel erfahren Sie, wie Sie einen Sicherheitsheader über das Regelmodul in Azure Front Door konfigurieren.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 6/22/2020
ms.author: duau
ms.openlocfilehash: ad1e8a8a2162ece69af9904d76a394d4bad5de23
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399139"
---
# <a name="add-security-headers-with-rules-engine"></a>Hinzufügen von Sicherheitsheadern mit Regel-Engines

Implementieren Sie Sicherheitsheader, um browserbasierte Sicherheitsrisiken zu vermeiden, z. B. HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy, oder X-Frame-Options. Sicherheitsbasierte Attribute können auch mit Cookies definiert werden.

Das folgende Beispiel zeigt Ihnen, wie Sie einen Content-Security-Policy-Header zu allen eingehenden Anforderungen hinzufügen, die dem Pfad entsprechen, der in der Route definiert ist, mit der Ihre Regelmodulkonfiguration verknüpft ist. Hier wird in unseren Anwendungen nur die Ausführung von Skripts von unserer vertrauenswürdigen Website **https://apiphany.portal.azure-api.net** zugelassen.

## <a name="add-a-content-security-policy-header-in-azure-portal"></a>Hinzufügen eines Content-Security-Policy-Headers im Azure-Portal

1. Wenn Sie entweder AFD oder das Regelmodul-Feature zum ersten Mal verwenden, sollten Sie vor dem Erstellen dieser speziellen Regel lernen, wie Sie [eine Frontdoor erstellen](quickstart-create-front-door.md) oder wie Sie [ein Regelmodul erstellen](front-door-tutorial-rules-engine.md).

2. Klicken Sie auf **Hinzufügen**, um eine neue Regel hinzuzufügen. Geben Sie der Regel einen Namen, und klicken Sie dann auf **Aktion hinzufügen** > **Antwortheader**.

3. Legen Sie den Operator auf **Append** fest, damit dieser Header als Antwort auf alle unter dieser Route eingehenden Anforderungen hinzugefügt wird.

4. Fügen Sie den Headernamen **Content-Security-Policy** hinzu, und definieren Sie die Werte, die dieser Header akzeptieren soll. In diesem Szenario haben wir *„script-src ‚self‘ https://apiphany.portal.azure-api.net“ ausgewählt.*

5. Nachdem Sie alle gewünschten Regeln zu Ihrer Konfiguration hinzugefügt haben, vergessen Sie nicht, zu Ihrer bevorzugten Route zu navigieren und Ihre Regelmodulkonfiguration mit Ihrer Routenregel zu verknüpfen. Dieser Schritt ist erforderlich, damit die Regel funktioniert. 

![Portalbeispiel](./media/front-door-rules-engine/rules-engine-security-header-example.png)

> [!NOTE]
> In diesem Szenario haben wir der Regel keine [Übereinstimmungsbedingungen](front-door-rules-engine-match-conditions.md) hinzugefügt. Für alle eingehenden Anforderungen, die dem in der Routenregel definierten Pfad entsprechen, wird diese Regel angewendet. Wenn Sie die Regel nur auf eine Teilmenge dieser Anforderungen anwenden möchten, fügen Sie dieser Regel unbedingt Ihre spezifischen Übereinstimmungsbedingungen hinzu.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [AFD-Regelmodul](front-door-rules-engine.md). 
- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
- Weitere Informationen zu [Übereinstimmungsbedingungen des Regelmoduls](front-door-rules-engine-match-conditions.md)
- Weitere Informationen finden Sie in der [CLI-Referenz](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/rules-engine?view=azure-cli-latest) des AFD-Regelmoduls. 
- Weitere Informationen finden Sie in der [PowerShell-Referenz](https://docs.microsoft.com/powershell/module/az.frontdoor/?view=azps-3.8.0) des AFD-Regelmoduls. 
