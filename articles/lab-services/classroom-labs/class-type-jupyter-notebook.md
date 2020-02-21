---
title: Einrichten eines Labs zum Vermitteln von Data Science mit Python und Jupyter Notebooks | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Lab einrichten, um Data Science mit Python und Jupyter Notebooks zu vermitteln.
services: lab-services
documentationcenter: na
author: emaher
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2019
ms.author: enewman
ms.openlocfilehash: dfb133f9aa3dd9b76f8b4ea4c6188cfaf9a67b75
ms.sourcegitcommit: 6e87ddc3cc961945c2269b4c0c6edd39ea6a5414
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2020
ms.locfileid: "77444110"
---
# <a name="set-up-a-lab-to-teach-data-science-with-python-and-jupyter-notebooks"></a>Einrichten eines Labs zum Vermitteln von Data Science mit Python und Jupyter Notebooks

In diesem Artikel wird beschrieben, wie Sie einen Vorlagencomputer in Lab Services mit den Tools einrichten, die erforderlich sind, um Kursteilnehmern die Verwendung von [Jupyter Notebooks](http://jupyter-notebook.readthedocs.io) zu vermitteln.  Jupyter Notebooks ist ein Open-Source-Projekt, mit dem Sie problemlos Rich-Text und ausführbaren [Python](https://www.python.org/)-Quellcode in einem einzelnen Zeichenbereich kombinieren können, der als Notebook bezeichnet wird.  Das Ausführen eines Notebooks führt zu einem linearen Datensatz von Eingaben und Ausgaben.  Diese Ausgaben können Text, Tabellen mit Informationen, Punktdiagramme und mehr enthalten.

## <a name="lab-configuration"></a>Labkonfiguration

Zum Einrichten dieses Labs benötigen Sie zunächst ein Azure-Abonnement und ein Lab-Konto. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Sobald Sie ein Azure-Abonnement erhalten, können Sie ein neues Lab-Konto in Azure Lab Services erstellen. Weitere Informationen zum Erstellen eines neuen Lab-Kontos finden Sie im [Tutorial zum Einrichten eines Lab-Kontos](tutorial-setup-lab-account.md).  Sie können auch ein vorhandenes Lab-Konto verwenden.

### <a name="lab-account-settings"></a>Lab-Kontoeinstellungen

Aktivieren Sie die Einstellungen für das Lab-Konto, die in der nachfolgenden Tabelle beschrieben werden. Weitere Informationen zum Aktivieren von Marketplace-Images finden Sie unter [Angeben von für Lab-Ersteller verfügbaren Marketplace-Images](specify-marketplace-images.md).

| Lab-Kontoeinstellungen | Instructions |
| ------------------- | ------------ |
| Marketplace-Image | Aktivieren Sie das Image [Data Science Virtual Machine – Windows 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) zur Verwendung in Ihrem Lab-Konto. |

>[!TIP]
>Dieser Artikel konzentriert sich auf das Konfigurieren eines Vorlagencomputers, der das Windows Server-Betriebssystem verwendet.  Es ist auch möglich, einen Data Science-Kurs mit Python und Jupyter Notebooks einzurichten, indem Sie eins der Images [Data Science Virtual Machine für Linux (CentOS)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm) oder [Data Science Virtual Machine für Linux (Ubuntu)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.linux-data-science-vm-ubuntu) aus dem Azure Marketplace verwenden.

### <a name="lab-settings"></a>Lab-Einstellungen

Verwenden Sie beim Einrichten eines Classroom-Labs die Einstellungen in der unten stehenden Tabelle.  Weitere Informationen zum Erstellen eines Classroom-Labs finden Sie im Tutorial [Einrichten eines Classroom-Labs](tutorial-setup-classroom-lab.md).

| Lab-Einstellungen | Wert/Anweisungen |
| ------------ | ------------------ |
|VM-Größe| Kleine GPU (Compute). Diese Größe eignet sich am besten für rechenintensive und netzwerkintensive Anwendungen wie künstliche Intelligenz und Deep Learning. |
|VM-Image| Data Science Virtual Machine – Windows 2016|

## <a name="template-machine"></a>Vorlagencomputer

Das Image [Data Science Virtual Machine – Windows 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) bietet die für Kurse dieser Art notwendigen Deep Learning-Frameworks und -Tools.  Das Image enthält Jupyter Notebooks und Visual Studio Code.  [Jupyter Notebooks](http://jupyter-notebook.readthedocs.io) ist eine Webanwendung, mit der Datenanalysten Berechnungen an Rohdaten durchführen und die Ergebnisse in derselben Umgebung anzeigen können.  Für den Vorlagencomputer wird die Webanwendung lokal ausgeführt.  [Visual Studio Code](https://code.visualstudio.com/) ist eine IDE, die eine umfangreiche interaktive Erfahrung zum Schreiben und Testen eines Notebooks bietet.  Weitere Informationen finden Sie unter [Arbeiten mit Jupyter-Notebooks in Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support).

Die verbleibende Aufgabe zum Einrichten des Kurses besteht darin, lokale Notebooks bereitzustellen.  Anweisungen zum Verwenden der Azure Machine Learning-Beispiele finden Sie unter [Konfigurieren einer Umgebung mit Jupyter Notebooks](../../machine-learning/how-to-configure-environment.md#jupyter).  Sie können auch Ihre eigenen Notebooks auf dem Vorlagencomputer bereitstellen.  Die Notebooks werden auf alle Kursteilnehmercomputer kopiert, wenn die Vorlage veröffentlicht wird.

## <a name="cost-estimate"></a>Kostenschätzung

Betrachten wir eine mögliche Kostenschätzung für diesen Kurs.  Wir setzen einen Kurs mit 25 Kursteilnehmern an.  Es ist eine Kursdauer von 20 Stunden geplant.  Zudem erhält jeder Kursteilnehmer ein Kontingent von 10 Stunden für Hausaufgaben und Aufgaben außerhalb der regulären Kurszeiten.  Als Größe des virtuellen Computers wurde „Kleine GPU (Compute)“ ausgewählt, was 139 Lab-Einheiten entspricht.

Hier ist ein Beispiel für eine mögliche Kostenschätzung für diesen Kurs:

25 Kursteilnehmer \* (20 geplante Stunden + 10 Kontingentstunden) \* 139 Lab-Einheiten \* 0,01 USD pro Stunde = 1042,5 USD

Weitere Informationen zu den Preisen finden Sie unter [Azure Lab Services-Preise](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Zusammenfassung

In diesem Artikel haben wir Sie durch die Schritte zum Erstellen eines Labs für einen Jupyter Notebooks-Kurs geführt. Sie können ein ähnliches Setup für andere Machine Learning-Kurse verwenden.

## <a name="next-steps"></a>Nächste Schritte

Die nächsten Einrichtungsschritte sind für alle Labs gleich.

- [Erstellen und Verwalten einer Vorlage](how-to-create-manage-template.md)
- [Hinzufügen von Benutzern](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kontingent festlegen](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zeitplan festlegen](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Registrierungslinks per E-Mail an Kursteilnehmer senden](how-to-configure-student-usage.md#send-invitations-to-users)
