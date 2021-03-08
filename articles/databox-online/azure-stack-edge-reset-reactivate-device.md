---
title: Zurücksetzen und Reaktivieren Ihres Azure Stack Edge Pro-Geräts | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie die Daten auf Ihrem Azure Stack Edge Pro-Gerät löschen und es dann reaktivieren.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/01/2020
ms.author: alkohli
ms.openlocfilehash: 4026bac9818b14c33c05d99caff4052adad196c3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743476"
---
# <a name="reset-and-reactivate-your-azure-stack-edge-pro-device"></a>Zurücksetzen und Reaktivieren Ihres Azure Stack Edge Pro-Geräts

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie ein Azure Stack Edge Pro-Gerät zurücksetzen, neu konfigurieren und reaktivieren, wenn Sie Probleme mit dem Gerät haben oder es aus einem anderen Grund neu starten müssen.

Nachdem Sie das Gerät zurückgesetzt haben, um die Daten zu entfernen, müssen Sie es als neue Ressource reaktivieren. Wenn Sie ein Gerät zurücksetzen, wird die Gerätekonfiguration entfernt, sodass Sie es über die lokale Webbenutzeroberfläche neu konfigurieren müssen.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
>
> * Löschen der Daten von den Datenträgern des Geräts
> * Reaktivieren des Geräts, indem Sie einen neuen Auftrag erstellen, das Gerät neu konfigurieren und dann aktivieren

## <a name="reset-data-from-the-device"></a>Zurücksetzen von Daten auf dem Gerät

Um die Daten von den Datenträgern Ihres Geräts zu löschen, müssen Sie Ihr Gerät zurücksetzen. 

Erstellen Sie bei Bedarf vor dem Zurücksetzen eine Kopie der lokalen Daten auf dem Gerät. Sie können die Daten vom Gerät in einen Azure Storage-Container kopieren.

>[!IMPORTANT]
> Wenn Sie Ihr Gerät zurücksetzen, werden alle darauf gespeicherten lokalen Daten und Workloads gelöscht. Dies kann nicht rückgängig gemacht werden. Setzen Sie deshalb Ihr Gerät nur zurück, wenn Sie es neu starten möchten.

Sie können Ihr Gerät über die lokale Webbenutzeroberfläche oder in PowerShell zurücksetzen. PowerShell-Anweisungen finden Sie unter [Zurücksetzen Ihres Geräts](./azure-stack-edge-connect-powershell-interface.md#reset-your-device).

[!INCLUDE] [Zurücksetzen von Daten auf dem Gerät](../../includes/azure-stack-edge-device-reset.md)

## <a name="reactivate-device"></a>Reaktivieren eines Geräts

Nachdem Sie das Gerät zurückgesetzt haben, müssen Sie es als neue Ressource reaktivieren. Nachdem Sie einen neuen Auftrag erteilt haben, müssen Sie die neue Ressource neu konfigurieren und dann reaktivieren.

Führen Sie zum Reaktivieren Ihres vorhandenen Geräts die folgenden Schritte aus:

1. Erstellen Sie einen neuen Auftrag für das vorhandene Gerät, indem Sie die Schritte in [Erstellen einer neuen Ressource](azure-stack-edge-gpu-deploy-prep?tabs=azure-portal#create-a-new-resource) ausführen. Wählen Sie auf der Registerkarte **Versandadresse** die Option **Ich habe bereits ein Gerät** aus.

   ![In der Versandadresse kein neues Gerät angeben](./media/azure-stack-edge-reset-reactivate-device/create-resource-with-no-new-device.png)

1. [Rufen Sie den Aktivierungsschlüssel ab](azure-stack-edge-gpu-deploy-prep?tabs=azure-portal#get-the-activation-key).

1. [Stellen Sie eine Verbindung mit dem Gerät her](azure-stack-edge-gpu-deploy-connect.md).

1. [Konfigurieren Sie das Netzwerk für das Gerät](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).

1. [Konfigurieren Sie die Geräteeinstellungen](azure-stack-edge-gpu-deploy-set-up-device-update-time.md).

1. [Konfigurieren Sie Zertifikate](azure-stack-edge-gpu-deploy-configure-certificates.md).

1. [Aktivieren Sie das Gerät](databox-online/azure-stack-edge-gpu-deploy-activate.md).

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich darüber, wie Sie [eine Verbindung mit einem Azure Stack Edge Pro-Gerät herstellen](azure-stack-edge-gpu-deploy-connect.md) können.
