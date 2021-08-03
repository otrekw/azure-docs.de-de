---
title: Erstellen einer bedarfsbasierten Sicherung Ihrer Oracle Database-Instanz in SnapCenter
description: In diesem Artikel erfahren Sie, wie Sie eine bedarfsbasierte Sicherung Ihrer Oracle Database-Instanz in SnapCenter für Ihre Oracle-Bare-Metal-Infrastruktur erstellen.
ms.topic: how-to
ms.subservice: baremetal-oracle
ms.date: 05/07/2021
ms.openlocfilehash: 699c070a3eeca2904f9620ca5f12c95c509210ab
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579154"
---
# <a name="create-on-demand-backup-of-your-oracle-database-in-snapcenter"></a>Erstellen einer bedarfsbasierten Sicherung Ihrer Oracle Database-Instanz in SnapCenter

In diesem Artikel erfahren Sie, wie Sie eine bedarfsbasierte Sicherung Ihrer Oracle Database-Instanz in SnapCenter erstellen. 

Sobald Sie [SnapCenter konfiguriert](configure-snapcenter-oracle-baremetal.md) haben, werden Sicherungen Ihrer Datendateien, Steuerungsdateien und Archivprotokolle basierend auf dem Zeitplan durchgeführt, den Sie bei Erstellung der Ressourcengruppen festgelegt haben. Im Rahmen des regulären Schutzes für Ihre Datenbank sollten Sie jedoch auch die Möglichkeit zu bedarfsbasierten Sicherungen implementieren.

## <a name="steps-to-create-an-on-demand-backup"></a>Schritte zum Erstellen einer bedarfsbasierten Sicherung

1. Klicken Sie im linken Menü auf die Option **Ressourcen**. Klicken Sie dann im Dropdownmenü neben **Ansicht** auf **Ressourcengruppe**. Wählen Sie den Ressourcengruppennamen entsprechend der bedarfsbasierten Sicherung aus, die Sie erstellen möchten.

2. Klicken Sie oben rechts auf **Jetzt sichern**.

3. Überprüfen Sie, ob die Ressourcengruppe und die Schutzrichtlinie für die bedarfsbasierte Sicherung korrekt sind. Setzen Sie das Häkchen im Kontrollkästchen für **Daten nach der Sicherung überprüfen**, wenn diese Sicherung überprüft werden soll. Wählen Sie **Sichern** aus.

Nach Abschluss der Sicherung steht sie in der Liste der Sicherungen unter **Ressourcen** zur Verfügung. Wählen Sie die der Ressourcengruppe zugeordneten Datenbanken aus, die Sie gesichert haben. Diese Sicherung wird entsprechend der Aufbewahrungsrichtlinie für bedarfsbasierte Sicherungen aufbewahrt, die Sie beim Erstellen der Schutzrichtlinie festgelegt haben.

## <a name="next-steps"></a>Nächste Schritte

Hier erfahren Sie, wie eine Oracle Database-Instanz in SnapCenter wiederhergestellt wird:

> [!div class="nextstepaction"]
> [Wiederherstellen einer Oracle Database-Instanz](restore-oracle-database-baremetal.md)
