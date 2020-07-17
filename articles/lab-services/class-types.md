---
title: Beispielklassentypen für Azure Lab Services | Microsoft-Dokumentation
description: In diesem Artikel sind einige Typen von Klassen aufgeführt, für die Sie mithilfe von Azure Lab Services Labs einrichten können.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 8564f5ece9d6df7db852c5ac16f00f1455d013ca
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85443756"
---
# <a name="class-types-overview---azure-lab-services"></a>Übersicht über Klassentypen: Azure Lab Services

Mithilfe von Azure Lab Services können Sie schnell eine Classroom-Lab-Umgebung in der Cloud einrichten. Die Artikel in diesem Abschnitt bieten Anleitungen zum Einrichten verschiedener Typen von Classroom-Labs mithilfe von Azure Lab Services.

## <a name="deep-learning-in-natural-language-processing"></a>Deep Learning mit Verarbeitung natürlicher Sprache

Sie können ein Lab mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services einrichten. NLP ist eine Form der künstlichen Intelligenz (KI), die Computern Übersetzungs-, Spracherkennungs- und andere Sprachverständnisfunktionen bietet. Kursteilnehmer, die einen Kurs zur Verarbeitung natürlicher Sprache belegen, erhalten einen virtuellen Linux-Computer, auf dem Sie lernen, wie neuronale Netzwerkalgorithmen angewendet werden, um Deep Learning-Modelle zum Analysieren geschriebener menschlicher Sprache zu entwickeln.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services](class-type-deep-learning-natural-language-processing.md).

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

## <a name="big-data-analytics"></a>Big Data-Analyse
Sie können für einen Big Data-Analysekurs ein GPU-Lab einrichten. Bei dieser Art von Kurs lernen die Kursteilnehmer, mit großen Datenmengen umzugehen und maschinelle und statistische Lernalgorithmen anzuwenden, um Erkenntnisse aus Daten zu gewinnen. Ein wichtiges Ziel für Kursteilnehmer ist es, den Umgang mit Datenanalysetools zu erlernen, z. B. mit dem Open-Source-Softwarepaket von Apache Hadoop, das Tools für die Speicherung, Verwaltung und Verarbeitung von Big Data bereitstellt. 

Ausführliche Informationen zum Einrichten dieser Art von Lab finden Sie unter [Einrichten eines Labs für Big Data-Analysen mithilfe der Docker-Bereitstellung von Hortonworks Data Platform](class-type-big-data-analytics.md).

## <a name="matlab"></a>MATLAB
[MATLAB](https://www.mathworks.com/products/matlab.html) steht für „Matrix Laboratory“ und bezeichnet eine Programmierplattform von [MathWorks](https://www.mathworks.com/).  Sie kombiniert Rechenleistung und Visualisierung, was sie in den Bereichen Mathematik, Ingenieurwesen, Physik und Chemie zu einem beliebten Tool macht.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs zum Kennenlernen von MATLAB](class-type-matlab.md).

## <a name="solidworks-computer-aided-design-cad"></a>SolidWorks: CAD (Computer-Aided Design)
Sie können ein GPU-Lab einrichten, mit dem Studenten der Ingenieurswissenschaften Zugriff auf [SolidWorks](https://www.solidworks.com/) erhalten.  SolidWorks stellt eine 3D-CAD-Umgebung für die Modellierung von Festkörpern dar.  Mit SolidWorks können Engineers ihre Entwürfe problemlos erstellen, visualisieren, simulieren und dokumentieren.

Ausführliche Informationen zum Einrichten derartiger Labs finden Sie unter [Einrichten eines Labs für Engineeringkurse mithilfe von SolidWorks](class-type-solidworks.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in folgenden Artikeln:

- [Einrichten eines Labs mit Schwerpunkt auf Deep Learning mit Verarbeitung natürlicher Sprache mithilfe von Azure Lab Services](class-type-deep-learning-natural-language-processing.md)
- [Shellskripts unter Linux](class-type-shell-scripting-linux.md)
- [„Ethisches“ Hacken](class-type-ethical-hacking.md)