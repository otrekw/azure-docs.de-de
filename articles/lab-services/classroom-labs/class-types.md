---
title: Beispielklassentypen für Azure Lab Services | Microsoft-Dokumentation
description: In diesem Artikel sind einige Typen von Klassen aufgeführt, für die Sie mithilfe von Azure Lab Services Labs einrichten können.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: e1c5504b30c2784e8657ccc0dc4ec18689fe2a68
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806812"
---
# <a name="class-types-overview---azure-lab-services"></a>Übersicht über Klassentypen: Azure Lab Services

Mithilfe von Azure Lab Services können Sie schnell eine Classroom-Lab-Umgebung in der Cloud einrichten. Die Artikel in diesem Abschnitt bieten Anleitungen zum Einrichten verschiedener Typen von Classroom-Labs mithilfe von Azure Lab Services.

## <a name="deep-learning-in-natural-language-processing"></a>Deep Learning mit Verarbeitung natürlicher Sprache

Sie können ein Lab mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services einrichten. Verarbeitung natürlicher Sprache ist eine Form der künstlichen Intelligenz (KI), die Computern Übersetzungs-, Spracherkennungs- und andere Sprachverständnisfunktionen bietet. Kursteilnehmer, die einen Kurs zur Verarbeitung natürlicher Sprache belegen, erhalten einen virtuellen Linux-Computer, auf dem Sie lernen, wie neuronale Netzwerkalgorithmen angewendet werden, um Deep Learning-Modelle zum Analysieren geschriebener menschlicher Sprache zu entwickeln.

Ausführliche Informationen zum Einrichten dieser Art von Lab finden Sie unter [Einrichten eines Labs mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services](class-type-deep-learning-natural-processing.md).

## <a name="shell-scripting-on-linux"></a>Shellskripts unter Linux

Sie können ein Lab zur Schulung in Shellskripts unter Linux einrichten. Die Skripterstellung empfiehlt sich bei der Systemverwaltung, um Administratoren sich wiederholende Aufgaben abzunehmen. In diesem beispielhaften Kurs geht es um klassische Bash-Skripts und erweiterte Skripts. Erweiterte Skripts sind Skripts, die Bash-Befehle und Ruby kombinieren. Dieser Ansatz ermöglicht Ruby das Weiterleiten von Daten, während Bash-Befehle mit der Shell interagieren können.

Kursteilnehmer bekommen einen virtuellen Linux-Computer zugewiesen, um die Grundlagen von Linux zu erlernen und sich mit der Erstellung von Bash-Skripts vertraut zu machen. Bei dem virtuellen Linux-Computer ist der Remotedesktopzugriff aktiviert und die Text-Editoren [gedit](https://help.gnome.org/users/gedit/stable/) und [Visual Studio Code](https://code.visualstudio.com/) sind installiert.

Ausführliche Informationen zum Einrichten dieser Art von Lab finden Sie unter [Shellskripts unter Linux](class-type-shell-scripting-linux.md).

## <a name="ethical-hacking"></a>„Ethisches“ Hacken

Sie können ein Lab für eine Klasse einrichten, die sich auf die forensischen Aspekte des ethischen Hackens konzentriert. Bei Penetrationstests, eine von der Community für ethisches Hacken verwendende Vorgehensweise, versucht eine Person, auf das System oder Netzwerk zuzugreifen, um Sicherheitsrisiken zu demonstrieren, die ein böswilliger Angreifer ausnutzen könnte.

In einem Kurs für ethisches Hacken erlernen die Kursteilnehmer moderne Techniken zum Schützen vor Schwachstellen kennen. Jeder Kursteilnehmer erhält einen virtuellen Windows Server-Hostcomputer mit zwei geschachtelten virtuellen Computern: einem virtuellen Computer mit einem [Metaspoiltable3](https://github.com/rapid7/metasploitable3)-Image und einem anderen Computer mit einem [Kali Linux](https://www.kali.org/)-Image. Der virtuelle Metasploitable-Computer wird angegriffen.  Der virtuelle Kali-Computer stellt die Tools zur Verfügung, die zum Ausführen forensischer Aufgaben erforderlich sind.

Ausführliche Informationen zum Einrichten dieser Art von Lab finden Sie unter [Einrichten eines Labs zum Unterrichten des ethischen Hackens](class-type-ethical-hacking.md).

## <a name="next-steps"></a>Nächste Schritte

Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Einrichten eines Labs mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services](class-type-deep-learning-natural-processing.md)
- [Shellskripts unter Linux](class-type-shell-scripting-linux.md)
- [„Ethisches“ Hacken](class-type-ethical-hacking.md)
