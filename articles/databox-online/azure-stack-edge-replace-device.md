---
title: Ersetzen Ihres Azure Stack Edge Pro-Geräts | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie ein Azure Stack Edge Pro-Ersatzgerät erhalten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 01/22/2021
ms.author: alkohli
ms.openlocfilehash: 91cb446da31f353162f90778d855056a9697d455
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98726570"
---
# <a name="replace-your-azure-stack-edge-pro-device"></a>Ersetzen Ihres Azure Stack Edge Pro-Geräts

In diesem Artikel wird beschrieben, wie Sie ein Azure Stack Edge Pro-Gerät ersetzen. Es wird ein Ersatzgerät benötigt, wenn das vorhandene Gerät einen Hardwarefehler aufweist oder ein Upgrade erforderlich ist. 


In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
>
> * Öffnen eines Supporttickets bei einem Hardwareproblem
> * Erstellen einer neuen Bestellung für ein Ersatzgerät im Azure-Portal
> * Installieren und Aktivieren des Ersatzgeräts
> * Zurückgeben des Originalgeräts

## <a name="open-a-support-ticket"></a>Öffnen eines Supporttickets

Wenn Ihr vorhandenes Gerät einen Hardwarefehler aufweist, öffnen Sie anhand der folgenden Schritte ein Supportticket:

1. Öffnen Sie ein Supportticket beim Microsoft-Support, um anzugeben, dass Sie das Gerät zurückgeben möchten. Wählen Sie als Problemtyp **Azure Stack Edge Pro-Hardware** und als Untertyp **Hardwareprobleme** aus.  

    ![Öffnen eines Supporttickets](media/azure-stack-edge-replace-device/open-support-ticket-1.png)  

2. Ein Microsoft-Support-Techniker setzt sich mit Ihnen in Verbindung, um herauszufinden, ob das Problem durch ein Ersatzteil (Field Replacement Unit, FRU) behoben werden kann und ob ein solches für diese Instanz verfügbar ist. Wenn kein Ersatzteil verfügbar ist oder das Gerät ein Hardwareupgrade benötigt, werden Sie vom Support aufgefordert, eine neue Bestellung aufzugeben und Ihr altes Gerät einzureichen.

## <a name="create-a-new-order"></a>Erstellen einer neuen Bestellung

Erstellen Sie eine neue Ressource für die Aktivierung Ihres Ersatzgeräts, indem Sie die Schritte in [Erstellen einer neuen Ressourcen](azure-stack-edge-gpu-deploy-prep.md#create-a-new-resource) ausführen.

> [!NOTE]
> Die Aktivierung eines Ersatzgeräts für eine vorhandene Ressource wird nicht unterstützt. Die neue Ressource wird als neue Bestellung betrachtet. Die Abrechnung beginnt 14 Tage, nachdem das Gerät an Sie gesendet wurde.

## <a name="install-and-activate-the-replacement-device"></a>Installieren und Aktivieren des Ersatzgeräts

Führen Sie die folgenden Schritte zum Installieren und Aktivieren des Ersatzgeräts aus:

1. [Installieren Sie Ihr Gerät](azure-stack-edge-deploy-install.md).
2. [Aktivieren Sie Ihr Gerät](azure-stack-edge-deploy-connect-setup-activate.md) für die neue Ressource, die Sie zuvor erstellt haben.

## <a name="return-your-existing-device"></a>Zurückgeben Ihres vorhandenen Geräts

Gehen Sie wie folgt vor, um das Originalgerät zurückzugeben:

1. [Löschen Sie alle Daten von dem Gerät.](azure-stack-edge-return-device.md#erase-data-from-the-device)
2. [Initiieren Sie die Geräterückgabe](azure-stack-edge-return-device.md#initiate-device-return) für das Originalgerät.
3. [Planen Sie die Abholung.](azure-stack-edge-return-device.md#schedule-a-pickup)
4. Nachdem das Gerät bei Microsoft eingegangen ist, können Sie die dem zurückgegebenen Gerät zugeordnete [Ressource löschen](azure-stack-edge-return-device.md#delete-the-resource).


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich darüber, wie Sie [ein Azure Stack Edge Pro-Gerät zurückgeben](azure-stack-edge-return-device.md) können.
