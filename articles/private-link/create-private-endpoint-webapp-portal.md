---
title: Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts
description: In diesem Artikel wird erläutert, wie eine private Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts hergestellt wird.
author: ericgre
ms.assetid: b8c5c7f8-5e90-440e-bc50-38c990ca9f14
ms.topic: how-to
ms.date: 10/07/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: 3e0f05240aba9b5c92689315e409aaabe793b3f4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91772837"
---
# <a name="connect-privately-to-a-web-app-by-using-azure-private-endpoint"></a>Herstellen einer privaten Verbindung mit einer Web-App mithilfe eines privaten Azure-Endpunkts

Der private Azure-Endpunkt ist der grundlegende Baustein für Azure Private Link. Mithilfe von Private Endpoint können Sie eine private Verbindung mit einer Web-App herstellen. In diesem Artikel erfahren Sie, wie Sie eine Web-App mithilfe von Private Endpoint bereitstellen und von einem virtuellen Computer aus eine Verbindung mit dieser Web-App herstellen.

Weitere Informationen hierzu finden Sie unter [Verwenden privater Endpunkte für eine Azure-Web-App][privateendpointwebapp].

> [!Note]
> Der private Endpunkt ist in öffentlichen Regionen für Windows- und Linux-Web-Apps mit den Tarifen PremiumV2 und PremiumV3 sowie dem Premium-Tarif für Azure Functions (auch als Elastic Premium-Tarif bezeichnet) verfügbar. 

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com) an.

## <a name="create-a-virtual-network-and-virtual-machine"></a>Erstellen eines virtuellen Netzwerks und eines virtuellen Computers

In diesem Abschnitt erfahren Sie, wie ein virtuelles Netzwerk und Subnetz erstellt werden, um einen virtuellen Computer zu hosten, der für den Zugriff auf eine Web-App über einen privaten Endpunkt verwendet wird.

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

Gehen Sie wie folgt vor, um das virtuelle Netzwerk und Subnetz zu erstellen:

1. Wählen Sie im linken Bereich **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.

1. Wählen Sie im Bereich **Virtuelles Netzwerk erstellen** die Registerkarte **Grundlagen** aus, und geben Sie die hier angegebenen Informationen ein:

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Bereich „Virtuelles Netzwerk erstellen“ im Azure-Portal][1]

1. Wählen Sie die Registerkarte **IP-Adressen** aus, und geben Sie die hier angegebenen Informationen ein:

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Registerkarte „IP-Adressen“ im Bereich „Virtuelles Netzwerk erstellen“][2]

1. Wählen Sie im Abschnitt **Subnetz** die Option **Subnetz hinzufügen** aus, geben Sie die hier angegebenen Informationen ein, und wählen Sie **Hinzufügen** aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Bereich „Subnetz hinzufügen“][3]

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Wählen Sie nach erfolgreicher Überprüfung **Erstellen** aus.

### <a name="create-the-virtual-machine"></a>Erstellen des virtuellen Computers

Gehen Sie wie folgt vor, um den virtuellen Computer zu erstellen:

1. Wählen Sie im Azure-Portal im linken Bereich **Ressource erstellen** > **Compute** > **Virtueller Computer** aus.

1. Geben Sie im Bereich **Virtuellen Computer erstellen – Grundlagen** die hier angegebenen Informationen ein:

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Bereich „Virtuellen Computer erstellen“][4]

1. Klicken Sie auf **Weiter: Datenträger**.

1. Übernehmen Sie die Standardeinstellungen im Bereich **Datenträger**, und wählen Sie **Weiter: Netzwerk**.

1. Geben Sie im Bereich **Netzwerk** die hier angegebenen Informationen ein:

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Registerkarte „Netzwerk“ im Bereich „Virtuellen Computer erstellen“][5]

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Wählen Sie nach erfolgreicher Überprüfung **Erstellen** aus.

## <a name="create-a-web-app-and-a-private-endpoint"></a>Erstellen einer Web-App und eines privaten Endpunkts

In diesem Abschnitt erstellen Sie eine private Web-App, die einen privaten Endpunkt verwendet.

> [!Note]
> Das Feature „Privater Endpunkt“ ist nur für die Tarife PremiumV2 und PremiumV3 verfügbar.

### <a name="create-the-web-app"></a>Erstellen der Web-App

1. Wählen Sie im Azure-Portal im linken Bereich **Ressource erstellen** > **Web** > **Web-App** aus.

1. Wählen Sie im Bereich **Web-App** die Registerkarte **Grundlagen** aus, und geben Sie die hier angegebenen Informationen ein:

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Registerkarte „Grundlagen“ im Bereich „Web-App“][6]

1. Klicken Sie auf **Überprüfen + erstellen**.

1. Wählen Sie nach erfolgreicher Überprüfung **Erstellen** aus.

### <a name="create-the-private-endpoint"></a>Erstellen des privaten Endpunkts

1. Wählen Sie in Web-App-Eigenschaften unter **Einstellungen** die Option **Netzwerk** aus, und klicken Sie anschließend unter **Verbindungen mit privaten Endpunkten** auf die Option **Verbindungen mit privaten Endpunkten konfigurieren**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Link „Verbindungen mit privaten Endpunkten konfigurieren“ im Bereich „Netzwerk“ der Web-App][7]

1. Wählen Sie im Assistent **Verbindungen mit privaten Endpunkten** die Option **Hinzufügen** aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot der Schaltfläche „Hinzufügen“ im Assistenten „Verbindungen mit privaten Endpunkten“][8]

1. Wählen Sie in den Dropdownlisten **Abonnement**, **Virtuelles Netzwerk** und **Subnetz** die entsprechenden Informationen und anschließend **OK** aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot des Bereichs „Privaten Endpunkt hinzufügen“][9]

1. Überwachen Sie den Fortschritt bei der Erstellung des privaten Endpunkts.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Fortschritt beim Hinzufügen des privaten Endpunkts][10]
   > ![Screenshot: Neu erstellter privater Endpunkt][11]

## <a name="connect-to-the-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

1. Geben Sie im Feld **Suchen** des Azure-Portals **myVm** ein.
1. Wählen Sie **Verbinden** und dann **RDP** aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Schaltfläche „RDP“ im Bereich „myVM“][12]

1. Wählen Sie im Bereich **Verbindung mit RDP herstellen** die Option **RDP-Datei herunterladen** aus.  

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Schaltfläche „RDP-Datei herunterladen“ im Bereich „Verbindung mit RDP herstellen“][13]

   Azure erstellt eine RDP-Datei (Remotedesktopprotokoll) und lädt sie auf Ihren Computer herunter.   

1. Öffnen Sie die heruntergeladene RDP-Datei.

   a. Wählen Sie an der Eingabeaufforderung **Verbinden** aus.  
   b. Geben Sie den Benutzernamen und das Kennwort an, die Sie beim Erstellen des virtuellen Computers angegeben haben.

     > [!Note]
     > Zur Verwendung dieser Anmeldeinformationen müssen Sie möglicherweise **Weitere Optionen** > **Anderes Konto verwenden** auswählen.

1. Klicken Sie auf **OK**.

   > [!Note]
   > Wenn während des Anmeldevorgangs eine Zertifikatwarnung angezeigt wird, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald das Fenster mit dem Desktop des virtuellen Computers angezeigt wird, minimieren Sie es, um zu Ihrem lokalen Desktop zurückzukehren.

## <a name="access-the-web-app-privately-from-the-vm"></a>Privates Zugreifen auf die Web-App über den virtuellen Computer

In diesem Abschnitt stellen Sie unter Verwendung des privaten Endpunkts eine private Verbindung mit der Web-App her.

1. Rufen Sie die private IP-Adresse Ihres privaten Endpunkts ab, indem Sie im Feld **Suchen** **Private Link** eingeben und in der Ergebnisliste **Private Link** auswählen.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Link „Private Link“ in der Liste mit den Suchergebnissen][14]

1. Wählen Sie im Private Link-Center im linken Bereich die Option **Private Endpunkte** aus, um Ihre privaten Endpunkte anzuzeigen.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Liste mit privaten Endpunkten im Private Link-Center][15]

1. Wählen Sie den privaten Endpunkt aus, mit dem eine Verknüpfung mit Ihrer Web-App und Ihrem Subnetz hergestellt wird.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Bereich mit den Eigenschaften für einen privaten Endpunkt][16]

1. Kopieren Sie die private IP-Adresse Ihres privaten Endpunkts und den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) Ihrer Web-App. Die private IP-Adresse im vorstehenden Beispiel lautet *`webappdemope.azurewebsites.net 10.10.2.4`* .

1. Vergewissern Sie sich im Bereich **myVM**, dass auf die Web-App nicht über die öffentliche IP-Adresse zugegriffen werden kann. Öffnen Sie hierzu einen Browser, und fügen Sie den Namen der Web-App ein. Auf der Seite muss die Meldung „Fehler 403 – Verboten“ angezeigt werden.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Seite mit der Fehlermeldung „Fehler 403 – Verboten“][17]

   Für den DNS-Eintrag können Sie eine der folgenden Optionen ausführen:
 
   - Verwenden Sie den Dienst für private Zonen von Azure DNS.  

     a. Erstellen Sie eine private DNS-Zone mit dem Namen *`privatelink.azurewebsites.net`* , und verknüpfen Sie sie mit dem virtuellen Netzwerk.  
     b. Erstellen Sie mit der IP-Adresse Ihres privaten Endpunkts zwei A-Datensätze (d. h. Name der App und Name des Dienststeuerungs-Managers [Service Control Manager, SCM]).  
     > [!div class="mx-imgBorder"]
     > ![Screenshot: Datensätze der privaten DNS-Zone][21]  

   - Verwenden Sie die Datei *hosts* des virtuellen Computers.  

     a. Erstellen Sie einen Hosteintrag. Öffnen Sie hierzu den Datei-Explorer, und suchen Sie nach der Datei *hosts*.  
     > [!div class="mx-imgBorder"]
     > ![Screenshot: Datei „hosts“ im Datei-Explorer][18]  
     b. Bearbeiten Sie die Datei *hosts* in einem Text-Editor, indem Sie einen Eintrag hinzufügen, der die private IP-Adresse und den öffentlichen Namen Ihrer Web-App enthält.  
     > [!div class="mx-imgBorder"]
     > ![Screenshot: Text der Datei „hosts“][19]  
     c. Speichern Sie die Datei.

1. Geben Sie in einem Browser die URL Ihrer Web-App ein.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Browser, in dem eine Web-App angezeigt wird][20]

Greifen Sie nun über den privaten Endpunkt auf Ihre Web-App zu.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie Ihre Arbeit mit dem privaten Endpunkt, der Web-App und dem virtuellen Computer abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen.

1. Geben Sie im Azure-Portal im Feld **Suchen** den Namen **ready-rg** ein, und wählen Sie anschließend in der Ergebnisliste **ready-rg** aus.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

1. Geben Sie unter **Ressourcengruppennamen eingeben** den Namen **ready-rg** ein, und wählen Sie anschließend **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

Anhand dieses Artikels haben Sie einen virtuellen Computer in einem virtuellen Netzwerk, eine Web-App und einen privaten Endpunkt erstellt. Sie haben aus dem Internet eine Verbindung mit einem virtuellen Computer hergestellt und über Private Link sicher mit der Web-App kommuniziert. 

Weitere Informationen zu privaten Endpunkten finden Sie unter [Was ist ein privater Endpunkt in Azure?][privateendpoint].

<!--Image references-->
[1]: ./media/create-private-endpoint-webapp-portal/createnetwork.png
[2]: ./media/create-private-endpoint-webapp-portal/ipaddresses.png
[3]: ./media/create-private-endpoint-webapp-portal/subnet.png
[4]: ./media/create-private-endpoint-webapp-portal/virtual-machine.png
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
[21]: ./media/create-private-endpoint-webapp-portal/dns-private-zone-records.png


<!--Links-->
[privateendpointwebapp]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
[privateendpoint]: https://docs.microsoft.com/azure/private-link/private-endpoint-overview
