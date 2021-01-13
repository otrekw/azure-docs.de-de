---
title: Verschieben von Azure-Ressourcen in eine andere Region
description: Bietet eine Übersicht über das Verschieben von Azure-Ressourcen zwischen Azure-Regionen.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: 7a71502ec361004079e0962d8bc6433316a4ba81
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90007637"
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

## <a name="move-resources-with-resource-mover"></a>Verschieben von Ressourcen mit Resource Mover

Ressourcen können mit [Azure Resource Mover](../../resource-mover/overview.md) in eine andere Region verschoben werden. Resource Mover bietet Folgendes:

- Einen einzelnen Hub für das regionsübergreifende Verschieben von Ressourcen
- Schnellere Verschiebung und geringere Komplexität Alles, was Sie brauchen, an einem zentralen Ort
- Ein einfaches und konsistentes Verfahren zum Verschieben verschiedener Arten von Azure-Ressourcen
- Eine einfache Möglichkeit zum Erkennen von Abhängigkeiten zwischen den zu verschiebenden Ressourcen. Dies hilft Ihnen, zusammenhängende Ressourcen gemeinsam zu verschieben, damit nach der Verschiebung in der Zielregion alles wie erwartet funktioniert.
- Automatische Bereinigung der Ressourcen in der Quellregion, falls Sie sie nach dem Verschieben löschen möchten
- Tests. Sie können eine Verschiebung testen und ggf. verwerfen, wenn Sie keine vollständige Verschiebung durchführen möchten.

Für die Verschiebung von Ressourcen in eine andere Region stehen verschiedene Methoden zur Verfügung:

- **Starten der Ressourcenverschiebung über eine Ressourcengruppe:** Bei dieser Methode wird die Regionsverschiebung innerhalb einer Ressourcengruppe initiiert. Nach dem Auswählen der zu verschiebenden Ressourcen wird der Prozess im Resource Mover-Hub fortgesetzt, um die Ressourcenabhängigkeiten zu überprüfen und den Verschiebungsprozess zu orchestrieren. [Weitere Informationen](../../resource-mover/move-region-within-resource-group.md)
- **Starten der Ressourcenverschiebung direkt über den Resource Mover-Hub:** Bei dieser Methode wird der Regionsverschiebungsprozess direkt im Hub gestartet. [Weitere Informationen](../../resource-mover/tutorial-move-region-virtual-machines.md)


## <a name="support-for-region-move"></a>Unterstützung von Regionsverschiebungen

Mit Resource Mover können derzeit folgende Ressourcen in eine andere Region verschoben werden:

- Virtuelle Azure-Computer und zugehörige Datenträger
- NICs
- Verfügbarkeitsgruppen
- Virtuelle Azure-Netzwerke
- Öffentliche IP-Adressen
- Netzwerksicherheitsgruppen (NSGs)
- Interne und externe Lastenausgleichsmodule
- Azure SQL-Datenbank-Instanzen und Pools für elastische Datenbanken

## <a name="region-move-process"></a>Regionsverschiebungsprozess

Der tatsächliche Prozess für die Verschiebung von Ressourcen in andere Regionen hängt von den zu verschiebenden Ressourcen ab. Es gibt jedoch einige allgemeine wichtige Schritte:

1. **Überprüfen der Voraussetzungen**: Zu den Voraussetzungen gehören die Sicherstellung, dass die benötigten Ressourcen in der Zielregion verfügbar sind, die Überprüfung, ob Sie über ein ausreichendes Kontingent verfügen, und die Überprüfung, ob Ihr Abonnement auf die Zielregion zugreifen kann.
2. **Analysieren von Abhängigkeiten**: Ihre Ressourcen haben möglicherweise Abhängigkeiten von anderen Ressourcen. Ermitteln Sie vor dem Verschieben Abhängigkeiten, damit die verschobenen Ressourcen auch nach dem Verschieben wie erwartet funktionieren.
3. **Vorbereitung der Verschiebung**: Dies sind die Schritte, die Sie in Ihrer primären Region vor dem Verschieben durchführen. Beispielsweise kann es erforderlich sein, eine Azure Resource Manager-Vorlage zu exportieren oder Ressourcen von der Quelle zum Ziel zu replizieren.
4. **Verschieben der Ressourcen**: Wie Sie Ressourcen verschieben, hängt davon ab, um welche Ressourcen es sich handelt. Möglicherweise müssen Sie eine Vorlage in der Zielregion bereitstellen oder ein Failover der Ressourcen auf das Ziel ausführen.
5. **Verwerfen von Zielressourcen**: Nach dem Verschieben von Ressourcen sollten Sie sich die Ressourcen ansehen, die sich nun in der Zielregion befinden, und entscheiden, ob nicht erforderliche Ressourcen vorhanden sind.
6. **Commit der Verschiebung**: Nach dem Überprüfen der Ressourcen in der Zielregion kann für einige Ressourcen eine abschließende Commitaktion erforderlich sein. Beispielsweise müssen Sie für eine Zielregion, die nun die primäre Region ist, möglicherweise Notfallwiederherstellung in einer neuen sekundären Region einrichten. 
7. **Bereinigen der Quelle**: Nachdem alles in der neuen Region eingerichtet wurde und aktiv ist, können Sie die Ressourcen, die Sie für die Verschiebung erstellt haben, und die Ressourcen in Ihrer primären Region bereinigen und außer Betrieb nehmen.



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verschiebungsprozess in Resource Mover finden Sie [hier](../../resource-mover/about-move-process.md).
