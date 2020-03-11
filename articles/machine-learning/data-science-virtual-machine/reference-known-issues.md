---
title: 'Referenz: Bekannte Probleme und Problembehandlung'
titleSuffix: Azure Data Science Virtual  Machine
description: Hier finden Sie eine Liste mit bekannten Problemen, Problemumgehungen und Problembehandlungsinformationen für Azure Data Science Virtual Machine.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: reference
ms.date: 10/10/2019
ms.openlocfilehash: b83effa95b17d712d4019f8ab5bf13c4f02a7d2b
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2020
ms.locfileid: "78206519"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Bekannte Probleme und Problembehandlung für Azure Data Science Virtual Machine

Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu ermitteln und zu beheben, auf die Sie bei der Verwendung der Azure Data Science Virtual Machine stoßen könnten.

## <a name="python-package-installation-issues"></a>Probleme bei der Installation von Python-Paketen

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>Beschädigung von Abhängigkeiten unter Linux beim Installieren von Paketen mit pip

Verwenden Sie beim Installieren von Paketen `sudo pip install` anstelle von `pip install`.

## <a name="disk-encryption-issues"></a>Probleme mit der Datenträgerverschlüsselung

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Nicht erfolgreiche Datenträgerverschlüsselung in der Ubuntu-DSVM-Instanz

Azure Disk Encryption (ADE) wird aktuell für Ubuntu-DSVM-Instanzen nicht unterstützt. Als Problemumgehung sollten Sie die Konfiguration der [serverseitigen Verschlüsselung von verwalteten Azure-Datenträgern](../../virtual-machines/windows/disk-encryption.md) in Erwägung ziehen.

## <a name="tool-appears-disabled"></a>Tool anscheinend deaktiviert

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V funktioniert in der Windows-DSVM-Instanz nicht

Dass Hyper-V unter Windows zunächst nicht funktioniert, ist ein erwartetes Verhalten. Für die Startleistung haben wir einige Dienste deaktiviert. So aktivieren Sie Hyper-V

1. Öffnen Sie die Suchleiste auf Ihrer Windows-DSVM.
1. Geben Sie „Dienste“ ein.
1. Legen Sie alle Hyper-V-Dienste auf „Manuell“ fest.
1. Legen Sie „Verwaltung virtueller Hyper-V-Computer“ auf „Automatisch“ fest.

Ihr Bildschirm sollte dann wie folgt aussehen:

   ![Aktivieren von Hyper-V](./media/workaround/hyperv-enable-dsvm.png)

