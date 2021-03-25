---
title: 'Häufig gestellte Fragen: SRE und Programmierung | Microsoft-Dokumentation'
titleSuffix: Azure
description: 'Häufig gestellte Fragen: Grundlegendes zur Beziehung zwischen SRE (Site Reliability Engineering) und Programmierung'
author: dnblankedelman
manager: ScottCa
ms.service: site-reliability-engineering
ms.topic: article
ms.date: 09/14/2020
ms.author: dnb
ms.openlocfilehash: b8865fdd53f4947b17a3621a128fc83f3d93d3e2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90089070"
---
# <a name="frequently-asked-questions-do-i-need-to-know-how-to-code-to-get-involved-with-sre"></a>Häufig gestellte Fragen: Muss ich mich mit Programmierung auskennen, um mich mit SRE zu befassen?

Wenn Einzelne erwägen, sich mit SRE zu befassen, und Teams über die Einführung von SRE-Praktiken nachdenken, stellt sich häufig die Frage: „Sind Programmierkenntnisse erforderlich?“

Die kurze Antwort lautet ja. 

Die vollständige Antwort ist jedoch etwas nuancierter. Werfen wir einen Blick auf drei Bereiche, in denen die Programmierung beim Site Reliability Engineering ins Spiel kommt, sowie auf das für jeden Bereich erforderliche Maß an Programmiererfahrung. Diese Liste ist nicht vollständig, doch diese Szenarien gehören zu den gängigsten Anwendungsfällen.

## <a name="scenario-1-removing-toil-through-automation"></a>Szenario 1: Weniger Mühsal durch Automatisierung

Site Reliability Engineers und andere, die SRE-Praktiken umsetzen, versuchen, mühselige Aufgaben nach Möglichkeit zu beseitigen. „Mühsal“ bedeutet in SRE etwas ganz Spezifisches. Sie bezieht sich auf Betriebsaufgaben, die von einem Menschen ausgeführt werden und bestimmte Eigenschaften aufweisen. Mühsal hat keinen langfristigen kompensierenden Wert. Sie bringt den Dienst in keiner sinnvollen Weise voran. Es handelt sich oft um sich wiederholende und weitgehend manuelle Vorgänge (auch wenn sie automatisiert werden könnten). Da der Dienst oder die Systeme im Laufe der Zeit immer größer werden, wird wahrscheinlich auch die Anzahl der Anforderungen an dieses System proportional ansteigen und noch mehr manuelle Tätigkeiten erfordern.

Wenn ein Dienst es beispielsweise erforderlich macht, dass das SRE-Team jede Woche etwas zurücksetzt, neue Konten und Speicherplatz von Hand bereitstellt oder wiederholt den Dienst manuell neu startet, dann ist diese betriebliche Last eine Mühsal. Die Durchführung dieser Aufgaben hat den Dienst in keiner Weise langfristig und nachhaltig verbessert. Diese Handlungen müssen wahrscheinlich immer und immer wieder wiederholt werden.

Site Reliability Engineers lehnen Mühsal ab. Sie arbeiten daran, sie zu beseitigen, wann immer dies möglich und angemessen ist. Dies ist einer der Bereiche, in denen Automatisierung bei SRE ins Spiel kommt. Wenn diese Anforderungen automatisch erfüllt werden können, hat das Team mehr Freiraum, an lohnenderen und wirkungsvolleren Projekten zu arbeiten.

*Programmiererfahrung*: Automatisierung erfordert ein gewisses Maß an Programmiererfahrung, setzt aber nicht unbedingt umfassende Kenntnisse im Bereich der Softwareentwicklung voraus. Wenn Sie kleine Skripts schreiben können (vielleicht mit PowerShell oder der Bourne-Shell) oder sogar eine [https://docs.microsoft.com/azure/logic-apps/logic-apps-overview](Azure Logic app) praktisch ohne Code erstellen können, kann diese App immer noch helfen, Mühsal zu vermeiden.

## <a name="scenario-2-control-through-apisdomain-specific-languages-dslstemplates"></a>Szenario 2: Kontrolle über APIs/domänenspezifische Sprachen/Vorlagen

Auch wenn dies für den Erfolg von SRE nicht unbedingt erforderlich ist, können Site Reliability Engineers ihre Aufgaben mithilfe von APIs, domänenspezifischen Sprachen und Vorlagen (insbesondere in Cloudumgebungen) hochskalieren. Das Bereitstellen bzw. Aufheben der Bereitstellung von Infrastrukturen, Konfigurieren der Überwachung und Integrieren mehrerer Dienste wird dank Programmierung wesentlich effizienter.

*Programmiererfahrung*: Wie im vorherigen Szenario erfordert Automatisierung ein gewisses Maß an Programmiererfahrung, setzt aber nicht unbedingt umfassende Kenntnisse im Bereich der Softwareentwicklung voraus. Zusätzlich zu den zuvor erwähnten Skripts und Logik-Apps können auch [https://docs.microsoft.com/azure/azure-resource-manager/templates/overview](Azure Resource Manager Templates) mit minimaler Programmiererfahrung verwendet werden.

## <a name="scenario-3-fixing-the-code"></a>Szenario 3: Korrigieren des Codes

Site Reliability Engineers arbeiten ständig an der Verbesserung der Zuverlässigkeit eines Systems. Für dieses Ziel ist es bisweilen erforderlich, in den Quellcode eines Systems einzutauchen, das Problem zu ermitteln und häufig eine Korrektur zur Codebasis beizutragen. Obwohl der Grad der Komplexität dieser Aufgabe je nach Situation sehr unterschiedlich sein kann, ist in diesen Fällen Programmiererfahrung unverzichtbar.

*Programmiererfahrung*: In diesem Szenario sind häufig umfassende Kenntnisse im Bereich der Softwareentwicklung erforderlich.


## <a name="next-steps"></a>Nächste Schritte

Möchten Sie noch mehr über Site Reliability Engineering oder Aufgaben mit geringem Programmieraufwand erfahren? Informieren Sie sich in der Dokumentationszentrale für [Site Reliability Engineering](../index.yml).
