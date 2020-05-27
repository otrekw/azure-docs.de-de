---
title: Konfigurieren des automatischen Herunterfahrens von VMs in Azure Lab Services
description: In diesem Artikel wird beschrieben, wie Sie das automatische Herunterfahren von VMs im Lab-Konto konfigurieren.
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
ms.date: 05/15/2020
ms.author: spelluru
ms.openlocfilehash: afe74e5c3aec2519ec22eee9573ae7b47c1c73f8
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588206"
---
# <a name="configure-automatic-shutdown-of-vms-on-disconnect-setting-for-a-lab-account"></a>Konfigurieren des automatischen Herunterfahrens von VMs mit der Trennen-Einstellung für ein Lab-Konto.
In diesem Artikel erfahren Sie, wie Sie das automatische Herunterfahren von virtuellen Windows-Lab-Computern (VM-Vorlage oder VM für Schüler/Studenten) aktivieren oder deaktivieren, nachdem eine Remotedesktopverbindung getrennt wurde. Sie können auch angeben, wie lange Lab Services warten soll, bis die Verbindung des Benutzers wieder hergestellt wird, bevor sie automatisch heruntergefahren werden.

## <a name="enable-automatic-shutdown"></a>Aktivieren des automatischen Herunterfahrens

1. Wählen Sie auf der Seite **Labkonto** im Menü auf der linken Seite die Option **Labeinstellungen**.
2. Wählen Sie die Option **Herunterfahren beim Trennen aktivieren** aus.
3. Geben Sie an, wie lange Lab Services warten soll, bis die Verbindung des Benutzers wieder hergestellt wird, bevor die VMs automatisch heruntergefahren werden.

    ![Einstellung für automatisches Herunterfahren in einem Labkonto](../media/how-to-configure-lab-accounts/automatic-shutdown-vm-disconnect.png)

    Diese Einstellung gilt für alle im Labkonto erstellten Labs. Ein Lab-Ersteller (Lehrer/Dozent) kann diese Einstellung auf Lab-Ebene außer Kraft setzen. Die Änderung an dieser Einstellung im Labkonto wirkt sich nur auf Labs aus, die nach der Änderung erstellt werden.

    Um diese Einstellung zu deaktivieren, deaktivieren Sie das Kontrollkästchen für die Option **Herunterfahren beim Trennen aktivieren** auf dieser Seite. 

## <a name="next-steps"></a>Nächste Schritte
Informationen dazu, wie ein Lab-Besitzer diese Einstellung auf Lab-Ebene konfigurieren oder außer Kraft setzen kann, finden Sie in [diesem Artikel](how-to-enable-shutdown-disconnect.md).
