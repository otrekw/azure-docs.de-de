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
ms.openlocfilehash: d022b1124146a1e506401e6cee257805e3a38fd3
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77526546"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Bekannte Probleme und Problembehandlung für Azure Data Science Virtual Machine

Dieser Artikel hilft Ihnen, Fehler oder Ausfälle bei der Verwendung von Azure Data Science Virtual Machine zu ermitteln und zu beheben.

## <a name="python-package-installation-issues"></a>Probleme bei der Installation von Python-Paketen

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>Beschädigung von Abhängigkeiten unter Linux beim Installieren von Paketen mit pip

Verwenden Sie beim Installieren von Paketen `sudo pip install` anstelle von `pip install`.

## <a name="disk-encryption-issues"></a>Probleme mit der Datenträgerverschlüsselung

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Nicht erfolgreiche Datenträgerverschlüsselung in der Ubuntu-DSVM-Instanz

Azure Disk Encryption (ADE) wird aktuell für Ubuntu-DSVM-Instanzen nicht unterstützt. Zur Umgehung dieses Problems können Sie ggf. die [Azure Storage-Verschlüsselung mit kundenseitig verwalteten Schlüsseln konfigurieren](../../storage/common/storage-encryption-keys-portal.md).

## <a name="tool-appears-disabled"></a>Tool anscheinend deaktiviert

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V funktioniert in der Windows-DSVM-Instanz nicht

Dies ist zu erwarten, da einige Dienste deaktiviert wurden, um die Leistung beim Start zu verbessern. Öffnen Sie die Suchleiste Ihrer Windows-DSVM-Instanz, geben Sie „Dienste“ ein, und legen Sie alle Hyper-V-Dienste auf „Manuell“ sowie „Hyper-V-Verwaltung für virtuelle Computer" auf „Automatisch“ fest, um sie wieder zu aktivieren.

Ihr Bildschirm sollte dann wie folgt aussehen:

   ![Aktivieren von Hyper-V](./media/workaround/hyperv-enable-dsvm.png)

