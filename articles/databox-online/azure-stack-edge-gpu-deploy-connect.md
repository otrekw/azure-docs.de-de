---
title: Tutorial zum Verbinden, Konfigurieren und Aktivieren eines Azure Stack Edge Pro-Geräts mit GPU im Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie über die lokale Webbenutzeroberfläche eine Verbindung mit Ihrem Azure Stack Edge-Gerät mit eingebauter GPU herstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 08/29/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Azure Stack Edge Pro so I can use it to transfer data to Azure.
ms.openlocfilehash: f069f711ccf5672041ea6f3b15a52fa2b3ba2022
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90900030"
---
# <a name="tutorial-connect-to-azure-stack-edge-pro-with-gpu"></a>Tutorial: Herstellen einer Verbindung mit Azure Stack Edge Pro mit GPU

In diesem Tutorial wird erläutert, wie Sie auf der lokalen Webbenutzeroberfläche eine Verbindung mit Ihrem Azure Stack Edge Pro-Gerät mit eingebauter GPU herstellen.

Der Verbindungsvorgang dauert ca. 5 Minuten.

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
>
> * Voraussetzungen
> * Herstellen einer Verbindung mit einem physischen Gerät


## <a name="prerequisites"></a>Voraussetzungen

Überprüfen Sie Folgendes, bevor Sie ein Azure Stack Edge Pro-Gerät mit GPU konfigurieren und einrichten:

* Sie haben Ihr physisches Gerät gemäß der Anleitung unter [Installieren von Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-install.md) installiert.


## <a name="connect-to-the-local-web-ui-setup"></a>Herstellen einer Verbindung mit der lokalen Webbenutzeroberfläche

1. Konfigurieren Sie den Ethernet-Adapter auf Ihrem Computer, um das Azure Stack Edge Pro-Gerät mit der statischen IP-Adresse 192.168.100.5 und dem Subnetz 255.255.255.0 zu verbinden.

2. Verbinden Sie den Computer mit PORT 1 auf Ihrem Gerät. Wenn Sie den Computer direkt (ohne Switch) mit dem Gerät verbinden, verwenden Sie ein Kreuzkabel oder einen USB-Ethernet-Adapter. Verwenden Sie die folgende Abbildung, um PORT 1 auf Ihrem Gerät zu identifizieren.

    ![Rückseite eines verkabelten Geräts](./media/azure-stack-edge-gpu-deploy-install/ase-two-pci-slots.png)

    Die Rückseite des Geräts kann je nach dem Modell, das Sie erhalten haben, etwas anders aussehen. Weitere Informationen finden Sie unter [Verkabeln des Geräts](azure-stack-edge-gpu-deploy-install.md#cable-the-device).


3. Öffnen Sie ein Browserfenster, und greifen Sie unter `https://192.168.100.10` auf die lokale Webbenutzeroberfläche des Geräts zu.  
    Diese Aktion dauert einige Minuten, nachdem Sie das Gerät aktiviert haben.

    Es wird eine Fehlermeldung oder eine Warnung mit dem Hinweis angezeigt, dass ein Problem mit dem Sicherheitszertifikat der Website vorliegt. 
   
    ![Meldung zum Sicherheitszertifikatfehler auf der Website](./media/azure-stack-edge-deploy-connect-setup-activate/image2.png)

4. Klicken Sie auf **Mit dieser Webseite fortfahren**.  
    Diese Schritte können sich je nach verwendetem Browser unterscheiden.

5. Melden Sie sich bei der Webbenutzeroberfläche des Geräts an. Das Standardkennwort lautet *Password1*. 
   
    ![Anmeldeseite für das Azure Stack Edge Pro-Gerät](./media/azure-stack-edge-deploy-connect-setup-activate/image3.png)

6. Ändern Sie das Geräteadministratorkennwort in der Eingabeaufforderung.  
    Das neue Kennwort muss zwischen acht und 16 Zeichen lang sein. Es muss drei der folgenden Zeichen enthalten: Großbuchstaben, Kleinbuchstaben, Ziffern und Sonderzeichen.

Sie befinden sich jetzt auf der Seite **Übersicht** Ihres Geräts. Im nächsten Schritt werden die Netzwerkeinstellungen Ihres Geräts konfiguriert.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Voraussetzungen
> * Herstellen einer Verbindung mit einem physischen Gerät


Informationen zum Konfigurieren von Netzwerkeinstellungen auf Ihrem Azure Stack Edge Pro-Gerät finden Sie unter:

> [!div class="nextstepaction"]
> [Konfigurieren des Netzwerks](./azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md)
