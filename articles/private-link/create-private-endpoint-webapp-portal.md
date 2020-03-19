---
title: Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts
description: Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: article
ms.date: 03/12/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: bb78536326885e043279de1ff77e6e8efcd95193
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "79037148"
---
# <a name="connect-privately-to-a-web-app-using-azure-private-endpoint-preview"></a>Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts (Vorschau)

Der private Azure-Endpunkt ist der grundlegende Baustein für Private Link in Azure. Er ermöglicht das Herstellen einer privaten Verbindung mit Ihrer Web-App.
In dieser Schnellstartanleitung erfahren Sie, wie Sie eine Web-App mit einem privaten Endpunkt bereitstellen und von einem virtuellen Computer aus eine Verbindung mit dieser Web-App herstellen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="virtual-network-and-virtual-machine"></a>Virtuelles Netzwerk und virtueller Computer

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Web-App über den privaten Endpunkt verwendet wird.

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und ein Subnetz.

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus, oder suchen Sie über das Suchfeld nach **Virtuelles Netzwerk**.

1. Geben Sie unter **Virtuelles Netzwerk erstellen** auf der Registerkarte „Grundlagen“ die folgenden Informationen ein, oder wählen Sie sie aus:

 ![Erstellen eines virtuellen Netzwerks][1]

1. Klicken Sie auf **Weiter: IP-Adressen >** , und geben Sie die folgenden Informationen ein, oder wählen Sie sie aus:

![Konfigurieren von IP-Adressen][2]

1. Klicken Sie im Abschnitt für das Subnetz auf **+ Subnetz hinzufügen**, geben Sie die folgenden Informationen ein, und klicken Sie auf **Hinzufügen**.

![Hinzufügen eines Subnetzes][3]

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Klicken Sie nach erfolgreicher Überprüfung auf **Erstellen**.

### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Compute** > **Virtueller Computer** aus.

1. Geben Sie unter „Virtuellen Computer erstellen – Grundlagen“ die folgenden Informationen ein, oder wählen Sie sie aus:

![Virtueller Computer – Grundlagen ][4]

1. Wählen Sie **Weiter: Datenträger** aus.

Übernehmen Sie die Standardeinstellungen.

1. Wählen Sie **Weiter: Netzwerk** und dann die folgenden Informationen aus:

![Netzwerk ][5]

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Wenn die Meldung über die erfolgreiche Überprüfung angezeigt wird, klicken Sie auf **Erstellen**.

## <a name="create-your-web-app-and-private-endpoint"></a>Erstellen Ihrer Web-App und des privaten Endpunkts

In diesem Abschnitt erstellen Sie eine private Web-App und verwenden dafür einen privaten Endpunkt.

> [!Note]
>Das Feature „Privater Endpunkt“ ist nur für Premium V2 und Isolated mit externer ASE-SKU verfügbar.

### <a name="web-app"></a>Web App

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Web** > **Web-App** aus.

1. Geben Sie unter „Web-App erstellen – Grundlagen“ die folgenden Informationen ein, oder wählen Sie sie aus:

![Web-App – Grundlagen ][6]

1. Wählen Sie **Überprüfen + erstellen** aus.

1. Wenn die Meldung über die erfolgreiche Überprüfung angezeigt wird, klicken Sie auf **Erstellen**.

### <a name="create-the-private-endpoint"></a>Erstellen des privaten Endpunkts

1. Wählen Sie unter den Web-App-Eigenschaften die Option **Einstellungen** > **Netzwerk** aus, und klicken Sie auf **Verbindungen mit privaten Endpunkten konfigurieren**.

![Web-App – Netzwerk][7]

1. Klicken Sie im Assistenten auf **+ Hinzufügen**.

![Web-App – Privater Endpunkt][8]

1. Geben Sie die Informationen zu Abonnement, virtuellem Netzwerk und Subnetz ein, und klicken Sie auf **OK**.

![Web-App-Netzwerk][9]

1. Überprüfen Sie die Erstellung des privaten Endpunkts.

![Überprüfen][10]
![Endansicht des privaten Endpunkts][11]

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

1. Geben Sie in der Suchleiste des Portals **myVm** ein.
1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche „Verbinden“ wird „Verbindung mit virtuellem Computer herstellen“ geöffnet. Wählen Sie **RDP** aus.

![Schaltfläche „RDP“][12]

1. Azure erstellt eine Remotedesktopprotokoll-Datei (RDP) und lädt sie auf Ihren Computer herunter, nachdem Sie auf **RDP-Datei herunterladen** geklickt haben.

![Herunterladen der RDP-Datei][13]

1. Öffnen Sie die heruntergeladene RDP-Datei.

- Wenn Sie dazu aufgefordert werden, wählen Sie „Verbinden“ aus.
- Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

> [!Note]
> Unter Umständen müssen Sie „Weitere Optionen > Anderes Konto verwenden“ auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

- Wählen Sie „OK“ aus.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie „Ja“ oder „Weiter“ aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.

## <a name="access-web-app-privately-from-the-vm"></a>Privates Zugreifen auf die Web-App vom virtuellen Computer

In diesem Abschnitt stellen Sie unter Verwendung des privaten Endpunkts eine private Verbindung mit der Web-App her.

1. Rufen Sie die private IP-Adresse Ihres privaten Endpunkts ab. Dazu geben Sie in der Suchleiste **Private Link** ein und wählen „Private Link“ aus.

![Private Link][14]

1. Wählen Sie im Private Link-Center die Option **Private Endpunkte** aus, um alle Ihre privaten Endpunkte aufzulisten.

![Private Link-Center][15]

1. Wählen Sie den Privater Endpunkt-Link zu Ihrer Web-App und Ihrem Subnetz aus.

![Eigenschaften eines privaten Endpunkts][16]

1. Kopieren Sie die private IP-Adresse Ihres privaten Endpunkts und den FQDN Ihrer Web-App (in diesem Fall webappdemope.azurewebsites.net 10.10.2.4).

1. Vergewissern Sie sich in „myVM“, dass auf die Web-App nicht über die öffentliche IP-Adresse zugegriffen werden kann. Öffnen Sie einen Browser, und kopieren Sie den Namen der Web-App. Es muss eine Fehlerseite vom Typ „403 Verboten“ angezeigt werden.

![Verboten][17]

> [!Note]
> Da sich dieses Feature in der Vorschauphase befindet, müssen Sie den DNS-Eintrag manuell verwalten.

1. Erstellen Sie den Hosteintrag. Dazu öffnen Sie den Datei-Explorer und suchen die Datei „hosts“.

![Datei „hosts“][18]

1. Bearbeiten Sie die Datei „hosts“ im Editor, und fügen Sie einen Eintrag mit der privaten IP-Adresse und dem öffentlichen Namen Ihrer Web-App hinzu.

![Inhalt von „hosts“][19]

1. Speichern Sie die Datei.

1. Öffnen Sie einen Browser, und geben Sie die URL Ihrer Web-App ein.

![Website mit PE][20]

1. Sie greifen über den privaten Endpunkt auf Ihre Web-App zu.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ihre Arbeit mit dem privaten Endpunkt, der Web-App und dem virtuellen Computer abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen:

1. Geben Sie oben im Portal im Suchfeld die Zeichenfolge „ready-rg“ ein, und wählen Sie in den Suchergebnissen den Eintrag „ready-rg“ aus.
1. Wählen Sie „Ressourcengruppe löschen“ aus.
1. Geben Sie für „GEBEN SIE DEN RESSOURCENGRUPPENNAMEN EIN:“ die Zeichenfolge „ready-rg“ ein, und wählen Sie „Löschen“ aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen virtuellen Computer in einem virtuellen Netzwerk, eine Web-App und einen privaten Endpunkt erstellt. Sie haben aus dem Internet eine Verbindung mit einem virtuellen Computer hergestellt und über Private Link sicher mit der Web-App kommuniziert. Weitere Informationen zu privaten Endpunkten finden Sie unter [Was ist ein privater Endpunkt in Azure?][privateendpoint].

<!--Image references-->
[1]: ./media/create-private-endpoint-webapp-portal/createnetwork.png
[2]: ./media/create-private-endpoint-webapp-portal/ipaddresses.png
[3]: ./media/create-private-endpoint-webapp-portal/subnet.png
[4]: ./media/create-private-endpoint-webapp-portal/virtualmachine.png
[5]: ./media/create-private-endpoint-webapp-portal/vmnetwork.png
[6]: ./media/create-private-endpoint-webapp-portal/webapp.png
[7]: ./media/create-private-endpoint-webapp-portal/webappnetworking.png
[8]: ./media/create-private-endpoint-webapp-portal/webapppe.png
[9]: ./media/create-private-endpoint-webapp-portal/webapppenetwork.png
[10]: ./media/create-private-endpoint-webapp-portal/inprogress.png
[11]: ./media/create-private-endpoint-webapp-portal/webapppefinal.png
[12]: ./media/create-private-endpoint-webapp-portal/rdp.png
[13]: ./media/create-private-endpoint-webapp-portal/rdpdownload.png
[14]: ./media/create-private-endpoint-webapp-portal/pl.png
[15]: ./media/create-private-endpoint-webapp-portal/plcenter.png
[16]: ./media/create-private-endpoint-webapp-portal/privateendpointproperties.png
[17]: ./media/create-private-endpoint-webapp-portal/forbidden.png
[18]: ./media/create-private-endpoint-webapp-portal/explorer.png
[19]: ./media/create-private-endpoint-webapp-portal/hosts.png
[20]: ./media/create-private-endpoint-webapp-portal/webappwithpe.png

<!--Links-->
[privateendpoint]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview
