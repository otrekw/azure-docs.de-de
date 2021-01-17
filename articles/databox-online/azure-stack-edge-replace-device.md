---
title: Ersetzen Ihres Azure Stack Edge Pro-Geräts | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie ein Azure Stack Edge Pro-Ersatzgerät erhalten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 01/07/2021
ms.author: alkohli
ms.openlocfilehash: 501136c6f5015dc8c68a1aba004432900675b11c
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98017968"
---
# <a name="replace-your-azure-stack-edge-pro-device"></a>Ersetzen Ihres Azure Stack Edge Pro-Geräts

In diesem Artikel wird beschrieben, wie Sie ein Azure Stack Edge Pro-Ersatzgerät erhalten. Es wird ein Ersatzgerät benötigt, wenn das vorhandene Gerät einen Hardwarefehler aufweist oder ein Upgrade erforderlich ist. 


In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
>
> * Öffnen eines Supporttickets bei einem Hardwareproblem
> * Erstellen einer neuen Ressource für das Ersatzgerät im Azure-Portal
> * Installieren und Aktivieren des Ersatzgeräts
> * Zurückgeben des Originalgeräts

## <a name="open-a-support-ticket"></a>Öffnen eines Supporttickets

Wenn Ihr vorhandenes Gerät einen Hardwarefehler hat, öffnen Sie ein Supportticket. Der Microsoft-Support prüft dann, ob ein Ersatzteil für die betreffende Instanz verfügbar oder ein Hardwareupgrade nötig ist. In beiden Fällen bestellt der Support ein Ersatzgerät.

1. Öffnen Sie ein Supportticket beim Microsoft-Support, um anzugeben, dass Sie das Gerät zurückgeben möchten. Wählen Sie den Problemtyp **Azure Stack Edge Pro-Hardware** aus.

    ![Öffnen eines Supporttickets](media/azure-stack-edge-replace-device/open-support-ticket-1.png)  

2. Sie werden durch einen Microsoft-Supporttechniker kontaktiert. Halten Sie die Versanddetails bereit.
<!--3. If you need a return shipping box, you can request it. Answer **Yes** to the question **Need an empty box to return**.-->


## <a name="create-a-resource-for-replacement-device"></a>Erstellen einer Ressource für das Ersatzgerät

> [!NOTE]
> Erstellen Sie eine neue Ressource zur Aktivierung Ihres Ersatzgeräts. Die Aktivierung des Ersatzgeräts für eine vorhandene Ressource wird nicht unterstützt.

Führen Sie die folgenden Schritte zum Erstellen einer Ressource aus.

1. Führen Sie die Schritte in [Erstellen einer neuen Ressource](azure-stack-edge-deploy-prep.md#create-a-new-resource) aus, um eine Ressource für das Ersatzgerät zu erstellen. 

2. Aktivieren Sie unbedingt das Kontrollkästchen für die Option **Ich habe ein Azure Stack Edge Pro-Gerät**. 

    ![Ressource für Ersatzgerät](media/azure-stack-edge-replace-device/replace-resource-1.png)  

## <a name="install-and-activate-the-replacement-device"></a>Installieren und Aktivieren des Ersatzgeräts

Führen Sie die folgenden Schritte zum Installieren und Aktivieren des Ersatzgeräts aus:

1. [Installieren Sie Ihr Gerät](azure-stack-edge-deploy-install.md).

2. [Aktivieren Sie Ihr Gerät](azure-stack-edge-deploy-connect-setup-activate.md) für die neue Ressource, die Sie zuvor erstellt haben.

## <a name="return-your-existing-device"></a>Zurückgeben Ihres vorhandenen Geräts

Gehen Sie wie folgt vor, um das Originalgerät zurückzugeben:

1. [Löschen Sie alle Daten von dem Gerät.](azure-stack-edge-return-device.md#erase-data-from-the-device)
2. [Initiieren Sie die Geräterückgabe](azure-stack-edge-return-device.md#initiate-device-return) für das Originalgerät.
3. [Planen Sie die Abholung.](azure-stack-edge-return-device.md#schedule-a-pickup)
4. [Löschen Sie die Ressource](azure-stack-edge-return-device.md#delete-the-resource), die dem zurückgegebenen Gerät zugeordnet ist.


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich darüber, wie Sie [ein Azure Stack Edge Pro-Gerät zurückgeben](azure-stack-edge-return-device.md) können.
