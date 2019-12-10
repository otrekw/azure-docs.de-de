---
title: Verschieben von Azure-Ressourcen in eine andere Region
description: Bietet eine Übersicht über das Verschieben von Azure-Ressourcen zwischen Azure-Regionen.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: raynew
ms.openlocfilehash: 22d8bcee96b4ac52641d4f0841267195f44fe15a
ms.sourcegitcommit: 8a2949267c913b0e332ff8675bcdfc049029b64b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/21/2019
ms.locfileid: "74308085"
---
# <a name="moving-azure-resources-across-regions"></a>Verschieben von Azure-Ressourcen zwischen Regionen

Dieser Artikel enthält Informationen zum Verschieben von Azure-Ressourcen zwischen Azure-Regionen.

Geografische Azure-Regionen, Regionen und Verfügbarkeitszonen bilden die Grundlage der globalen Azure-Infrastruktur. [Geografische Azure-Regionen](https://azure.microsoft.com/global-infrastructure/geographies/) enthalten in der Regel mindestens zwei [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/). Eine Region ist ein Bereich in einer geografischen Region, die Verfügbarkeitszonen und mehrere Rechenzentren enthält. 

Nach dem Bereitstellen von Ressourcen in einer bestimmten Azure-Region gibt es verschiedene Gründe, warum Sie Ressourcen in eine andere Region verschieben möchten.

- **Ausrichtung an einer neuen Region**: Verschieben von Ressourcen in eine neu eingeführte Azure-Region, die zuvor noch nicht verfügbar war.
- **Ausrichtung für Dienste/Features**: Verschieben von Ressourcen, um die Vorteile der Dienste oder Features zu nutzen, die in einer bestimmten Region verfügbar sind.
- **Reagieren auf Geschäftsentwicklungen**: Verschieben von Ressourcen in eine Region als Reaktion auf Geschäftsänderungen, z.B. Fusionen oder Akquisitionen.
- **Ausrichtung an einer nahegelegenen Region**: Verschieben von Ressourcen in eine Region, die für Ihr Unternehmen lokal ist.
- **Erfüllen von Datenanforderungen**: Verschieben von Ressourcen, um Anforderungen an Datenresidenz oder an Datenklassifizierung zu erfüllen. [Weitere Informationen](https://azure.microsoft.com/mediahandler/files/resourcefiles/achieving-compliant-data-residency-and-security-with-azure/Achieving_Compliant_Data_Residency_and_Security_with_Azure.pdf)
- **Reagieren auf Bereitstellungsanforderungen**: Verschieben von Ressourcen, die fehlerhaft bereitgestellt wurden, oder als Reaktion auf Kapazitätsanforderungen. 
- **Reagieren auf Außerbetriebnahmen**: Verschieben von Ressourcen aufgrund der Außerbetriebnahme von Regionen.

## <a name="move-process"></a>Verschiebungsvorgang

Der eigentliche Verschiebungsvorgang ist von den Ressourcen abhängig, die Sie verschieben. Es gibt jedoch einige allgemeine wichtige Schritte:

- **Überprüfen der Voraussetzungen**: Zu den Voraussetzungen gehören die Sicherstellung, dass die benötigten Ressourcen in der Zielregion verfügbar sind, die Überprüfung, ob Sie über ein ausreichendes Kontingent verfügen, und die Überprüfung, ob Ihr Abonnement auf die Zielregion zugreifen kann.
- **Analysieren von Abhängigkeiten**: Ihre Ressourcen haben möglicherweise Abhängigkeiten von anderen Ressourcen. Ermitteln Sie vor dem Verschieben Abhängigkeiten, damit die verschobenen Ressourcen auch nach dem Verschieben wie erwartet funktionieren.
- **Vorbereitung der Verschiebung**: Dies sind die Schritte, die Sie in Ihrer primären Region vor dem Verschieben durchführen. Beispielsweise kann es erforderlich sein, eine Azure Resource Manager-Vorlage zu exportieren oder Ressourcen von der Quelle zum Ziel zu replizieren.
- **Verschieben der Ressourcen**: Wie Sie Ressourcen verschieben, hängt davon ab, um welche Ressourcen es sich handelt. Möglicherweise müssen Sie eine Vorlage in der Zielregion bereitstellen oder ein Failover der Ressourcen auf das Ziel ausführen.
- **Verwerfen von Zielressourcen**: Nach dem Verschieben von Ressourcen sollten Sie sich die Ressourcen ansehen, die sich nun in der Zielregion befinden, und entscheiden, ob nicht erforderliche Ressourcen vorhanden sind.
- **Commit der Verschiebung**: Nach dem Überprüfen der Ressourcen in der Zielregion kann für einige Ressourcen eine abschließende Commitaktion erforderlich sein. Beispielsweise müssen Sie für eine Zielregion, die nun die primäre Region ist, möglicherweise Notfallwiederherstellung in einer neuen sekundären Region einrichten. 
- **Bereinigen der Quelle**: Nachdem alles in der neuen Region eingerichtet wurde und aktiv ist, können Sie die Ressourcen, die Sie für die Verschiebung erstellt haben, und die Ressourcen in Ihrer primären Region bereinigen und außer Betrieb nehmen.



## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Ressourcen, die regionsübergreifendes Verschieben unterstützen, finden Sie unter [Unterstützung des Verschiebungsvorgangs für Ressourcen](region-move-support.md).
