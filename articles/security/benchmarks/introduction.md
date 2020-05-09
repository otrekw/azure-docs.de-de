---
title: Einführung zum Azure Security-Vergleichstest
description: Einführung zum Security-Vergleichstest
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 883042c94576014c3bdeabd38516d9531c8fd3dc
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82606856"
---
# <a name="azure-security-benchmark-introduction"></a>Einführung zum Azure Security-Vergleichstest

Sie haben möglicherweise mehrere Jahre oder sogar Jahrzehnte Erfahrung mit dem lokalen Computing. Sie wissen, wie Sie diese Bereitstellungen absichern, aber die Cloud ist anders. Wie wissen Sie, ob Ihre Cloudbereitstellungen sicher sind? Was sind die Unterschiede zwischen den Sicherheitsmethoden für lokale Systeme und Cloudbereitstellungen?

Es gibt eine umfangreiche Sammlung von Whitepapers, Best Practices, Referenzarchitekturen, Webanleitungen, Open-Source-Tools, kommerziellen Lösungen, Intelligence-Feeds und mehr, die zur Sicherung der Cloud genutzt werden können. Welche Option sollten Sie nutzen? Was können Sie tun, um ein akzeptables Maß an Sicherheit in der Cloud zu erzielen? 

Eine der besten Möglichkeiten zum Schutz Ihrer Cloudbereitstellungen besteht darin, sich auf die Empfehlungen eines Vergleichstest für Cloudsicherheit zu konzentrieren. Die Empfehlungen von Vergleichstests für die Sicherung jeglicher Dienste beginnen mit grundlegenden Informationen über Cybersicherheitsrisiken und deren Handhabung. Anschließend können Sie diese Informationen nutzen, indem Sie anhand der Sicherheitsempfehlungen aus den Vergleichstests Ihres Clouddienstanbieters bestimmte Sicherheitskonfigurationseinstellungen in Ihrer Umgebung auswählen. 

Der Azure Security-Vergleichstest enthält eine Sammlung von wirkungsvollen Sicherheitsempfehlungen, mit denen Sie den Großteil der von Ihnen in Azure genutzten Dienste sichern können. Sie können diese Empfehlungen als „allgemein“ oder „organisatorisch“ betrachten, da sie auf die meisten Azure-Dienste anwendbar sind. Die Empfehlungen des Azure Security-Vergleichstest werden anschließend für jeden Azure-Service angepasst. Diese individuelle Anleitung ist dann in den Artikeln über die Dienstempfehlungen enthalten. 

In der Dokumentation zum Azure Security-Vergleichstest werden Sicherheitskontrollen und Dienstempfehlungen angegeben.

- **Sicherheitskontrollelement**: Die Empfehlungen des Azure Security-Vergleichstests werden nach Sicherheitskontrollelementen kategorisiert. Sicherheitskontrollelemente stellen von Anbietern unabhängige allgemeine Sicherheitsanforderungen dar, wie z. B. Netzwerksicherheit und Datenschutz. Jedes Sicherheitskontrollelement enthält eine Reihe von Sicherheitsempfehlungen und Anweisungen, mit denen Sie diese Empfehlungen implementieren können. 
- **Sicherheitsempfehlungen**: Wenn verfügbar, enthalten die Empfehlungen des Vergleichstests für Azure-Dienste auch Empfehlungen des Azure Security-Vergleichstests, die speziell auf diesen Dienst zugeschnitten sind. 

Die Begriffe „Kontrollelement“, „Vergleichstest“ und „Baseline“ kommen in der Dokumentation des Azure Security-Vergleichstests häufig vor, und es ist wichtig, zu verstehen, wie Azure diese Begriffe verwendet. 

| Begriff | BESCHREIBUNG | Beispiel |
|--|--|--|
| Control | Ein **Kontrollelement** ist eine allgemeine Beschreibung eines Features oder einer Aktivität, das bzw. die berücksichtigt werden muss, und ist nicht spezifisch für eine Technologie oder Implementierung. | Datenschutz ist eines dieser Sicherheitskontrollelemente. Dieses Kontrollelement enthält spezifische Maßnahmen, die zur Gewährleistung des Datenschutzes erforderlich sind. |
| Vergleichstest | Ein **Vergleichstest** enthält Sicherheitsempfehlungen für eine bestimmte Technologie, wie z. B. Azure. Die Empfehlungen werden nach dem Kontrollelement kategorisiert, zu dem sie gehören. | Der Azure Security-Vergleichstest umfasst die für die Azure-Plattform spezifischen Sicherheitsempfehlungen.  |
| Grundwert | Die **Baseline** sind Sicherheitsanforderungen für eine Organisation. Die Sicherheitsanforderungen basieren auf Empfehlungen aus Vergleichstests. Jede Organisation entscheidet, welche Vergleichstestempfehlungen in ihre Baseline aufgenommen werden sollen. | Das Unternehmen Contoso erstellt seine Sicherheitsbaseline, indem es im Azure Security-Vergleichstest bestimmte Empfehlungen als erforderlich festlegt. |

Wir freuen uns über Ihre Rückmeldungen zum Azure Security-Vergleichstest! Im Feedbackbereich unten können Sie uns gern Ihre Kommentare hinterlassen. Wenn Sie Ihr Feedback lieber diskreter an das Team des Azure Security-Vergleichstest weitergeben möchten, können Sie gern das Formular unter https://aka.ms/AzSecBenchmark ausfüllen. 
