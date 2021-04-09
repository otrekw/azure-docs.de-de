---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 11/21/2019
ms.author: alkohli
ms.openlocfilehash: 0c6845f081ccbe42e70964eaa939d58597e3b69b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "96185538"
---
1. Navigieren Sie zur lokalen Webbenutzeroberfläche Ihres Geräts, und melden Sie sich bei Ihrem Gerät an. Stellen Sie sicher, dass das Gerät entsperrt ist.

2. Wechseln Sie zur Seite **Netzwerkeinstellungen**. Notieren Sie sich die IP-Adresse des Geräts für die Netzwerkschnittstelle, die zum Herstellen der Verbindung mit dem Client verwendet wird.

3. Wenn Sie mit einem Windows-Remoteclient arbeiten, starten Sie **Editor** als Administrator, und öffnen dann in `C:\Windows\System32\Drivers\etc` die Datei „hosts“.

4. Fügen Sie Ihrer hosts-Datei den folgenden Eintrag hinzu: `<Device IP address> <Blob service endpoint>`.

    Sie haben den Blob-Dienstendpunkt über das im Azure-Portal erstellte Edge-Speicherkonto erhalten. Sie verwenden nur das Suffix des Blob-Dienstendpunkts.

    Verwenden Sie die folgende Abbildung als Referenz. Speichern Sie die Datei `hosts`.

    ![Ändern der Datei „hosts“ auf dem Windows-Client](media/azure-stack-edge-gateway-add-device-ip-address-blob-service-endpoint/hosts-file-1.png)