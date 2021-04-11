---
title: Tutorial zum Verbinden, Konfigurieren und Aktivieren eines Azure Stack Edge Pro R-Geräts im Azure-Portal | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie über die lokale Webbenutzeroberfläche eine Verbindung mit Ihrem Azure Stack Edge Pro R-Gerät herstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/15/2020
ms.author: alkohli
ms.openlocfilehash: 1ded68783ec45c649ab4aa41996cb0113a7fa42d
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106059920"
---
# <a name="tutorial-connect-to-azure-stack-edge-pro-r"></a>Tutorial: Herstellen einer Verbindung mit Azure Stack Edge Pro R

In diesem Tutorial erfahren Sie, wie Sie über die lokale Webbenutzeroberfläche eine Verbindung mit Ihrem Azure Stack Edge Pro R-Gerät herstellen.

Der Verbindungsvorgang dauert ca. 5 Minuten.

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
>
> * Voraussetzungen
> * Herstellen einer Verbindung mit einem physischen Gerät


## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich, dass folgende Voraussetzungen erfüllt sind, bevor Sie ein Azure Stack Edge Pro R-Gerät konfigurieren und einrichten:

* Sie haben das physische Gerät gemäß der Anleitung unter [Tutorial: Installieren von Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-install.md) installiert.


## <a name="connect-to-the-local-web-ui-setup"></a>Herstellen einer Verbindung mit der lokalen Webbenutzeroberfläche

1. Konfigurieren Sie den Ethernet-Adapter auf Ihrem Computer für die Verbindungsherstellung mit dem Azure Stack Edge Pro R-Gerät mit der statischen IP-Adresse 192.168.100.5 und dem Subnetz 255.255.255.0.

2. Verbinden Sie den Computer mit PORT 1 auf Ihrem Gerät. Wenn Sie den Computer direkt (ohne Switch) mit dem Gerät verbinden, verwenden Sie ein Kreuzkabel oder einen USB-Ethernet-Adapter. Verwenden Sie die folgende Abbildung, um PORT 1 auf Ihrem Gerät zu identifizieren.

    ![Rückseite eines verkabelten Geräts](./media/azure-stack-edge-pro-r-deploy-install/backplane-cabled.png)


3. Öffnen Sie ein Browserfenster, und greifen Sie unter `https://192.168.100.10` auf die lokale Webbenutzeroberfläche des Geräts zu.  
    Diese Aktion dauert einige Minuten, nachdem Sie das Gerät aktiviert haben.

    Es wird eine Fehlermeldung oder eine Warnung mit dem Hinweis angezeigt, dass ein Problem mit dem Sicherheitszertifikat der Website vorliegt. 
   
    ![Meldung zum Sicherheitszertifikatfehler auf der Website](media/azure-stack-edge-pro-r-deploy-connect/connect-web-ui-1.png)


4. Klicken Sie auf **Mit dieser Webseite fortfahren**.  
    Diese Schritte können sich je nach verwendetem Browser unterscheiden.

5. Melden Sie sich bei der Webbenutzeroberfläche des Geräts an. Das Standardkennwort lautet *Password1*. 
   
    ![Anmeldeseite für Azure Stack Edge-Gerät](media/azure-stack-edge-pro-r-deploy-connect/connect-web-ui-3.png)

6. Ändern Sie das Geräteadministratorkennwort in der Eingabeaufforderung.  
    Das neue Kennwort muss zwischen acht und 16 Zeichen lang sein. Es muss drei der folgenden Zeichen enthalten: Großbuchstaben, Kleinbuchstaben, Ziffern und Sonderzeichen.

Sie befinden sich jetzt auf der Seite **Übersicht** Ihres Geräts. Im nächsten Schritt werden die Netzwerkeinstellungen Ihres Geräts konfiguriert.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Voraussetzungen
> * Herstellen einer Verbindung mit einem physischen Gerät


Informationen zum Konfigurieren der Netzwerkeinstellungen für Ihr Azure Stack Edge Pro R-Gerät finden Sie hier:

> [!div class="nextstepaction"]
> [Konfigurieren des Netzwerks](./azure-stack-edge-pro-r-deploy-configure-network-compute-web-proxy.md)
