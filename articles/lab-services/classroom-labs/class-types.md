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
ms.topic: article
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 27619a69a1f7fbded8ce6430afc2b8e9a8b4a00c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79296726"
---
# <a name="class-types-overview---azure-lab-services"></a>Übersicht über Klassentypen: Azure Lab Services

Mithilfe von Azure Lab Services können Sie schnell eine Classroom-Lab-Umgebung in der Cloud einrichten. Die Artikel in diesem Abschnitt bieten Anleitungen zum Einrichten verschiedener Typen von Classroom-Labs mithilfe von Azure Lab Services.

## <a name="deep-learning-in-natural-language-processing"></a>Deep Learning mit Verarbeitung natürlicher Sprache

Sie können ein Lab mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services einrichten. NLP ist eine Form der künstlichen Intelligenz (KI), die Computern Übersetzungs-, Spracherkennungs- und andere Sprachverständnisfunktionen bietet. Kursteilnehmer, die einen Kurs zur Verarbeitung natürlicher Sprache belegen, erhalten einen virtuellen Linux-Computer, auf dem Sie lernen, wie neuronale Netzwerkalgorithmen angewendet werden, um Deep Learning-Modelle zum Analysieren geschriebener menschlicher Sprache zu entwickeln.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services](class-type-deep-learning-natural-processing.md).

## <a name="shell-scripting-on-linux"></a>Shellskripts unter Linux

Sie können ein Lab zur Schulung in Shellskripts unter Linux einrichten. Die Skripterstellung empfiehlt sich bei der Systemverwaltung, um Administratoren sich wiederholende Aufgaben abzunehmen. In diesem beispielhaften Kurs geht es um klassische Bash-Skripts und erweiterte Skripts. Erweiterte Skripts sind Skripts, die Bash-Befehle und Ruby kombinieren. Dieser Ansatz ermöglicht Ruby das Weiterleiten von Daten, während Bash-Befehle mit der Shell interagieren können.

Kursteilnehmer bekommen einen virtuellen Linux-Computer zugewiesen, um die Grundlagen von Linux zu erlernen und sich mit der Erstellung von Bash-Skripts vertraut zu machen. Bei dem virtuellen Linux-Computer ist der Remotedesktopzugriff aktiviert und die Text-Editoren [gedit](https://help.gnome.org/users/gedit/stable/) und [Visual Studio Code](https://code.visualstudio.com/) sind installiert.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs zur Schulung in Shellskripts unter Linux](class-type-shell-scripting-linux.md).

## <a name="ethical-hacking"></a>„Ethisches“ Hacken

Sie können ein Lab für eine Klasse einrichten, die sich auf die forensischen Aspekte des ethischen Hackens konzentriert. Bei Penetrationstests, eine von der Community für ethisches Hacken verwendete Vorgehensweise, versucht eine Person, auf das System oder Netzwerk zuzugreifen, um Schwachstellen zu demonstrieren, die ein böswilliger Angreifer ausnutzen könnte.

In einem Kurs für ethisches Hacken erlernen die Kursteilnehmer moderne Techniken zum Schützen vor Schwachstellen kennen. Jeder Kursteilnehmer erhält einen virtuellen Windows Server-Hostcomputer mit zwei geschachtelten virtuellen Computern: einem virtuellen Computer mit einem [Metaspoiltable3](https://github.com/rapid7/metasploitable3)-Image und einem anderen Computer mit einem [Kali Linux](https://www.kali.org/)-Image. Der virtuelle Metasploitable-Computer wird angegriffen.  Der virtuelle Kali-Computer stellt die Tools zur Verfügung, die zum Ausführen forensischer Aufgaben erforderlich sind.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs zum Vermitteln von Kursen für ethisches Hacken](class-type-ethical-hacking.md).

## <a name="database-management"></a>Datenbankverwaltung
Datenbankkonzepte werden in den Einführungskursen vermittelt, die in den meisten Informatikfachbereichen an Hochschulen angeboten werden. Sie können ein Lab für einen Grundkurs zur Datenbankverwaltung in Azure Lab Services einrichten. Sie können beispielsweise eine Vorlage für virtuelle Computer in einem Lab mit einem [MySQL](https://www.mysql.com/)-Datenbankserver oder einem [SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)-Server einrichten.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs zum Vermitteln von Kenntnissen zur Verwaltung von relationalen Datenbanken](class-type-database-management.md).

## <a name="python-and-jupyter-notebooks"></a>Python und Jupyter Notebook-Instanzen
Sie können einen Vorlagencomputer in Azure Lab Services mit den Tools einrichten, die erforderlich sind, um Kursteilnehmern die Verwendung von [Jupyter Notebooks](http://jupyter-notebook.readthedocs.io) zu vermitteln. Jupyter Notebooks ist ein Open-Source-Projekt, mit dem Sie problemlos Rich-Text und ausführbaren [Python](https://www.python.org/)-Quellcode in einem einzelnen Zeichenbereich kombinieren können, der als Notebook bezeichnet wird. Das Ausführen eines Notebooks führt zu einem linearen Datensatz von Eingaben und Ausgaben.  Diese Ausgaben können Text, Tabellen mit Informationen, Punktdiagramme und mehr enthalten.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs zum Vermitteln von Data Science mit Python und Jupyter Notebooks](class-type-jupyter-notebook.md).

## <a name="mobile-app-development-with-android-studio"></a>Entwicklung mobiler Apps mit Android Studio
Sie können ein Lab in Azure Lab Services einrichten, um eine Einführungsklasse für die Entwicklung mobiler Anwendungen zu unterrichten. Diese Klasse befasst sich mit mobilen Android-Anwendungen, die im [Google Play Store](https://play.google.com/store/apps) veröffentlicht werden können.  Die Kursteilnehmer lernen, wie Anwendungen mit [Android Studio](https://developer.android.com/studio) erstellt werden.  Für das lokale Testen der Anwendung wird der [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) verwendet.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs zum Vermitteln der Entwicklung mobiler Anwendungen mit Android Studio](class-type-mobile-dev-android-studio.md).


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Artikeln:

- [Einrichten eines Labs mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services](class-type-deep-learning-natural-processing.md)
- [Shellskripts unter Linux](class-type-shell-scripting-linux.md)
- [„Ethisches“ Hacken](class-type-ethical-hacking.md)