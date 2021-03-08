---
title: Sprache „Bicep“ für Azure Resource Manager-Vorlagen
description: Hier wird die Sprache „Bicep“ für das Bereitstellen der Infrastruktur in Azure über Azure Resource Manager-Vorlagen beschrieben.
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 6a2750dc99e82c9cf8c9b8b97d156d3a9fe30f31
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743534"
---
# <a name="what-is-bicep-preview"></a>Was ist Bicep (Vorschau)?

Bicep ist eine Sprache für die deklarative Bereitstellung von Azure-Ressourcen. Sie vereinfacht die Dokumenterstellung, indem eine präzise Syntax und bessere Unterstützung für Modularität und die Wiederverwendung von Code bereitgestellt wird. Bicep ist eine domänenspezifische Sprache (Domain-specific Language, DSL), was bedeutet, dass sie für ein bestimmtes Szenario oder eine bestimmte Domäne entworfen wurde. Sie ist nicht als allgemeine Programmiersprache zum Schreiben von Anwendungen vorgesehen.

Die Sprache ist eine transparente Abstraktion über Azure Resource Manager-Vorlagen (ARM-Vorlagen). Jede Bicep-Datei wird in eine ARM-Standardvorlage kompiliert. In einer ARM-Vorlage gültige Ressourcentypen, API-Versionen und Eigenschaften sind auch in einer Bicep-Datei gültig.

[!INCLUDE [Bicep preview](../../../includes/resource-manager-bicep-preview.md)]

## <a name="get-started"></a>Erste Schritte

[Installieren Sie die Tools](https://github.com/Azure/bicep/blob/main/docs/installing.md), um Bicep verwenden zu können.

Nachdem Sie die Tools installiert haben, können Sie das [Bicep-Tutorial](./bicep-tutorial-create-first-bicep.md) testen. In der Tutorialreihe werden die Struktur und die Funktionen von Bicep erläutert. Sie stellen Bicep-Dateien bereit und konvertieren eine ARM-Vorlage in die entsprechende Bicep-Datei.

Mit dem [Bicep Playground](https://aka.ms/bicepdemo) können Sie die entsprechenden JSON- und Bicep-Dateien zum Vergleich nebeneinander anzeigen.

Wenn Sie eine vorhandene ARM-Vorlage in Bicep konvertieren möchten, finden Sie unter [Dekompilieren von JSON in Bicep](compare-template-syntax.md#decompile-json-to-bicep) weitere Informationen.

## <a name="bicep-improvements"></a>Bicep-Verbesserungen

Bicep bietet im Vergleich zum entsprechenden JSON-Code eine einfachere und präzisere Syntax. Sie verwenden keine `[...]`-Ausdrücke. Stattdessen rufen Sie Funktionen direkt auf, rufen Werte von Parametern und Variablen ab und verweisen auf Ressourcen. Einen vollständigen Vergleich der Syntax finden Sie unter [Vergleichen von JSON und Bicep für Vorlagen](compare-template-syntax.md).

Bicep verwaltet Abhängigkeiten zwischen Ressourcen automatisch. Sie können die Einstellung `dependsOn` vermeiden, wenn der symbolische Name einer Ressource in einer anderen Ressourcendeklaration verwendet wird.

Mit Bicep können Sie Ihr Projekt in mehrere Module aufteilen.

Die Struktur der Bicep-Datei ist flexibler als die JSON-Vorlage. Sie können an beliebiger Stelle in der Datei Parameter, Variablen und Ausgaben deklarieren. In JSON müssen Sie alle Parameter, Variablen und Ausgaben in den entsprechenden Abschnitten der Vorlage deklarieren.

Die VS Code-Erweiterung für Bicep bietet eine umfassendere Validierung sowie IntelliSense. Die Erweiterung bietet beispielsweise die Verwendung von IntelliSense zum Abrufen von Eigenschaften einer Ressource.

## <a name="faq"></a>Häufig gestellte Fragen

**Warum wird eine neue Sprache entwickelt, anstatt eine vorhandene zu verwenden?**

Sie können sich Bicep eher als Revision der vorhandenen ARM-Vorlagensprache als eine neue Sprache vorstellen. Die Syntax wurde geändert, aber die Kernfunktionen und die Runtime bleiben unverändert.

Vor der Entwicklung von Bicep wurde die Verwendung einer vorhandenen Programmiersprache in Betracht gezogen. Wir sind der Ansicht, dass unserer Zielgruppe das Erlernen der Bicep-Sprache leichter fällt als das Erlernen einer völlig anderen Sprache.

**Warum liegt der Fokus nicht auf Terraform oder anderen Infrastructure-as-Code-Angeboten von Drittanbietern?**

Verschiedene Benutzer bevorzugen unterschiedliche Konfigurationssprachen und -tools. Wir möchten sicherstellen, dass alle diese Tools eine optimale Nutzung in Azure bieten. Bicep ist Teil dieser Lösung.

Wenn Sie mit Terraform zufrieden sind, gibt es keinen Grund für einen Wechsel. Microsoft ist bestrebt, die optimale Nutzung von Terraform in Azure zu bieten.

Für Kunden, die ARM-Vorlagen ausgewählt haben, verbessert Bicep die Dokumenterstellung. Außerdem hilft Bicep Kunden beim Produktübergang, wenn diese keine Infrastructure-as-Code-Lösung verwendet haben.

**Kann Bicep nur in Azure verwendet werden?**

Bicep ist eine domänenspezifische Sprache mit dem Fokus auf der Bereitstellung von Komplettlösungen in Azure. Damit dieses Ziel erreicht werden kann, ist die Verwendung einiger APIs außerhalb von Azure nötig. Erwartungsgemäß werden Erweiterbarkeitspunkte für diese Szenarios bereitgestellt.

**Was geschieht mit meinen vorhandenen ARM-Vorlagen?**

Diese funktionieren weiterhin genau so wie zuvor. Sie müssen keine Änderungen vornehmen. Die JSON-Sprache wird für zugrunde liegende ARM-Vorlagen weiterhin unterstütz. Bicep-Dateien werden in JSON kompiliert, und dieser JSON-Code wird für die Bereitstellung an Azure gesendet.

Sie können die [JSON-Dateien umgehend in Bicep-Dateien konvertieren](compare-template-syntax.md#decompile-json-to-bicep).

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem [Bicep-Tutorial](./bicep-tutorial-create-first-bicep.md).
