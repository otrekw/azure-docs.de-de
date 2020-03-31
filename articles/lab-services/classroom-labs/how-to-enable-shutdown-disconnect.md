---
title: 'Azure Lab Services: Aktivieren des Herunterfahrens von VMs beim Trennen | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das automatische Herunterfahren von VMs aktivieren oder deaktivieren, wenn eine Remotedesktopverbindung getrennt wird.
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
ms.date: 12/13/2019
ms.author: spelluru
ms.openlocfilehash: 68a27a325a0ef02c6eeea9867a21ba0e24ab5321
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77117135"
---
# <a name="enable-automatic-shutdown-of-vms-on-disconnect"></a>Aktivieren des automatischen Herunterfahrens von VMs beim Trennen
In diesem Artikel erfahren Sie, wie Sie das automatische Herunterfahren von **Windows 10**-Lab-VMs (VM-Vorlage oder VM für Schüler und Studenten) aktivieren oder deaktivieren, nachdem eine Remotedesktopverbindung getrennt wurde. Sie können auch angeben, wie lange die VMs warten sollen, bis die Verbindung des Benutzers wieder hergestellt wird, bevor sie automatisch heruntergefahren werden.

Ein Labkontoadministrator kann diese Einstellung für das Labkonto konfigurieren, in dem Sie Labs erstellen. Weitere Informationen finden Sie unter [Konfigurieren des automatischen Herunterfahrens von VMs beim Trennen für ein Labkonto](how-to-configure-lab-accounts.md#automatic-shutdown-of-vms-on-disconnect). Als Labbesitzer können Sie die Einstellung beim oder nach dem Erstellen eines Labs außer Kraft setzen. 

## <a name="configure-when-creating-a-lab"></a>Konfigurieren beim Erstellen eines Labs
Auf der Seite „Schritt 3“ des Laberstellungsassistenten können Sie dieses Feature aktivieren oder deaktivieren. Außerdem können Sie angeben, wie lange die VM warten soll, bis die Verbindung des Benutzers wieder hergestellt wird, bevor sie automatisch heruntergefahren wird. 

![Konfigurieren zum Zeitpunkt der Laberstellung](../media/how-to-enable-shutdown-disconnect/configure-lab-creation.png)

## <a name="configure-after-the-lab-is-created"></a>Konfigurieren nach Erstellen des Labs
Sie können diese Einstellung auf der Seite **Einstellungen** konfigurieren, wie in der folgenden Abbildung dargestellt: 

![Konfigurieren nach Erstellen des Labs](../media/how-to-enable-shutdown-disconnect/configure-lab-automatic-shutdown.png)

> [!WARNING]
> Wenn Sie das Windows-Betriebssystem einer VM herunterfahren, bevor Sie eine RDP-Sitzung mit der VM trennen, funktioniert das automatische Herunterfahren nicht ordnungsgemäß.  

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- [Dashboard für Classroom-Labs](use-dashboard.md)