---
title: Einrichten von SnapCenter zum Weiterleiten von Datenverkehr von Azure an Oracle BareMetal-Server
description: Erfahren Sie, wie Sie NetApp SnapCenter einrichten, um Datenverkehr von Azure an Oracle BareMetal-Infrastrukturserver weiterzuleiten.
ms.topic: how-to
ms.subservice: baremetal-oracle
ms.date: 05/05/2021
ms.openlocfilehash: 9d8f83712822dcf556efe1098eadd1cfdb4867e6
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110576228"
---
# <a name="set-up-netapp-snapcenter-to-route-traffic-from-azure-to-oracle-baremetal-servers"></a>Einrichten von NetApp SnapCenter zum Weiterleiten von Datenverkehr von Azure an Oracle BareMetal-Server

In diesem Artikel erfahren Sie, wie Sie NetApp SnapCenter einrichten, um Datenverkehr von Azure an Oracle BareMetal-Infrastrukturserver weiterzuleiten. 

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> - **Systemanforderungen SnapCenter-Server**: Azure Windows 2019 oder höher mit 4 vCPU, 16 GB RAM und mindestens 500 GB verwaltetem SSD Premium-Speicher.
> - **ExpressRoute-Netzwerkanforderungen**: Ein SnapCenter-Benutzer für Oracle BareMetal muss mit Microsoft Operations zusammenarbeiten, um die Netzwerkkonten zu erstellen, sodass die Kommunikation zwischen Ihrem persönlichen virtuellen Speichercomputer (VM) und der SnapCenter-VM in Azure möglich wird.
> - **Java 1.8 auf BareMetal-Instanzen**: Für die SnapCenter-Plug-Ins muss Java 1.8 auf den BareMetal-Instanzen installiert sein.

## <a name="steps-to-integrate-snapcenter"></a>Schritte zum Integrieren von SnapCenter

Erfahren Sie, wie Sie NetApp SnapCenter einrichten, um Datenverkehr von Azure an Oracle BareMetal-Infrastrukturserver weiterzuleiten. 

1. Erstellen Sie eine Supportticketanforderung, um den benutzergenerierten öffentlichen Schlüssel dem Microsoft-Betriebsteam mitzuteilen. Um den Zugriff auf das Speichersystem für den SnapCenter-Benutzer einzurichten, ist Support erforderlich. 

2. Erstellen Sie einen virtuellen Computer in Ihrem VNet (Azure Virtual Network), der Zugriff auf Ihre BareMetal-Instanzen hat. Dieser virtuelle Computer wird für SnapCenter verwendet. 

3. Herunterladen und Installieren von SnapCenter. 

4. Sicherungs- und Wiederherstellungsvorgänge. 

>[!NOTE]
> Bei diesen Schritten wird davon ausgegangen, dass Sie bereits eine ExpressRoute-Verbindung zwischen Ihrem Abonnement in Azure und Ihrem Mandanten in Oracle HaaS erstellt haben.

## <a name="create-a-support-ticket-for-user-role-storage-setup"></a>Erstellen eines Supportticket für die Einrichtung der des Speichers für die Benutzerrolle

1. Rufen Sie das Azure-Portal auf und gehen Sie zur Seite Abonnements. Wählen Sie Ihr BareMetal-Abonnement aus.
2. Auf der Seite Ihres BareMetal-Abonnements wählen Sie die Option **Ressourcengruppen** aus.
3. Wählen Sie eine geeignete Ressourcengruppe in einer Region aus.
    
    :::image type="content" source="media/netapp-snapcenter-integration-oracle-baremetal/select-resource-group.png" alt-text="Screenshot: Ressourcengruppen auf der Abonnementseite":::

4. Wählen Sie einen SKU-Eintrag aus, der SAP HANA in Azure Storage entspricht. 

    :::image type="content" source="media/netapp-snapcenter-integration-oracle-baremetal/select-sku.png" alt-text="Screenshot: Ressourcengruppe mit einer in Azure hervorgehobenen SKU vom Typ „SAP HANA“":::

5. Wählen Sie **Neue Supportanfrage** aus.

6. Geben Sie die folgenden Informationen für das Ticket auf der Registerkarte **Grundeinstellungen** an:
    - **Problemtyp**: Technisch
    -   **Abonnement:** Ihr Abonnement
    -   **Dienst**: BareMetal-Infrastruktur
    -   **Ressource**: Ihre Ressourcengruppe
    -   **Zusammenfassung**: Einrichten des SnapCenter-Zugriffs
    -   **Problemtyp**: Konfiguration und Setup
    -   **Problemuntertyp**: Einrichten von SnapCenter

7. Fügen Sie in der  **Beschreibung** des Supporttickets in der Registerkarte  **Details**  den Inhalt einer *.pem-Datei in das Textfeld ein, um weitere Details anzuzeigen. Sie können auch eine *.pem-Datei zippen und hochladen. snapcenter.pem ist Ihr öffentlicher Schlüssel für den SnapCenter-Benutzer. Verwenden Sie das folgende Beispiel, um eine *.pem-Datei aus einer Ihrer BareMetal-Instanzen zu erstellen. 

    :::image type="content" source="media/netapp-snapcenter-integration-oracle-baremetal/pem.png" alt-text="Screenshot: Beispielinhalt einer .pem-Datei":::

    >[!NOTE]
    >Der Name der Datei „snapcenter“ ist der Benutzername, der zum Durchführen von REST-API-Aufrufen erforderlich ist. Daher wird empfohlen, den Dateinamen deskriptiv zu gestalten.

8.  Wählen Sie **Überprüfen + erstellen** aus, um das Supportticket zu überprüfen.

9.  Nachdem das Zertifikat für öffentliche Schlüssel übermittelt wurde, richtet Microsoft den SnapCenter-Benutzernamen für Ihren Mandanten sowie die SVM-IP-Adresse (IP-Adresse des virtuellen Speichercomputers) ein. Microsoft stellt Ihnen die SVM-IP-Adresse zur Verfügung.

10. Nachdem Sie die SVM-IP-Adresse erhalten haben, legen Sie ein Kennwort für den Zugriff auf die SVM fest, die Ihrer Kontrolle unterliegt.

    Im Folgenden finden Sie ein Beispiel für den REST-Aufruf von HANA (große Instanzen) oder einer VM im virtuellen Netzwerk, die Zugriff auf die Umgebung von HANA (große Instanzen) hat und verwendet wird, um das Kennwort festzulegen.
    
    :::image type="content" source="media/netapp-snapcenter-integration-oracle-baremetal/sample-rest-call.png" alt-text="Screenshot: REST-Beispielaufruf":::

11. Stellen Sie sicher, dass auf der BareMetal-Instanz, die zum Erstellen der *.pem-Datei verwendet wird, keine Proxyvariable aktiv ist.

     :::image type="content" source="media/netapp-snapcenter-integration-oracle-baremetal/ensure-no-proxy.png" alt-text="Screenshot: Nicht konfigurierter HTTP-Proxy, um sicherzustellen, dass beim Erstellen der *.pem-Datei keine Proxyvariable auf der BareMetal-Instanz aktiv ist":::

12. Auf dem Clientcomputer ist es jetzt möglich, Befehle ohne einen Benutzername/Kennwort für aktivierte REST-Befehle auszuführen. Testen Sie die Verbindung: 

    Kein Proxy:

    :::image type="content" source="media/netapp-snapcenter-integration-oracle-baremetal/connection-test.png" alt-text="Screenshot: Test der Verbindung und der aktivierten REST-Befehle ohne Benutzername/Kennwort":::


       >[!NOTE]
       > Bei beiden curl-Befehlen kann „--verbose“ hinzugefügt werden, um weitere Details darüber zu liefern, warum ein Befehl nicht erfolgreich war.

## <a name="install-snapcenter"></a>Installieren von SnapCenter

1. Navigieren Sie auf dem bereitgestellten virtuellen Windows-Computer zur [NetApp-Website](https://mysupport.netapp.com/site/products/all/details/snapcenter/downloads-tab).

2. Melden Sie sich an, und laden Sie SnapCenter herunter. Download > SnapCenter > **Version 4.4**.

3. Installieren Sie SnapCenter 4.4. Klicken Sie auf **Weiter**.

    Das Installationsprogramm überprüft die Voraussetzungen der VM. Achten Sie auf die Größe der VM, insbesondere in größeren Umgebungen. Es ist in Ordnung, die Installation auch dann fortzusetzen, wenn ein Neustart aussteht.

4. Konfigurieren Sie die Benutzeranmeldeinformationen für SnapCenter. Standardmäßig wird hier der Windows-Benutzer eingetragen, der die Anwendung für die Installation gestartet hat. Es wird empfohlen, die Standardports zu verwenden, es sei denn, es liegt ein Portkonflikt vor.

    Der Installations-Assistent kann einige Zeit benötigen und zeigt den Fortschritt an.
 
5. Klicken Sie nach Abschluss der Installation auf **Fertigstellen**.  Notieren Sie sich die Webadresse für das SnapCenter-Webportal.  Sie können das SnapCenter-Webportal auch erreichen, indem Sie auf das SnapCenter-Symbol doppelklicken, das nach Abschluss der Installation auf dem Desktop angezeigt wird.
 
## <a name="disable-enhanced-messaging-service-ems-messages-to-netapp-auto-support"></a>Deaktivieren der automatischen NetApp-Unterstützung für EMS-Nachrichten (Enhanced Messaging Service)

Die EMS-Datenerfassung ist standardmäßig aktiviert und wird alle sieben Tage nach dem Installationsdatum ausgeführt. Sie können die Datenerfassung jederzeit deaktivieren.

1. Richten Sie über eine PowerShell-Befehlszeile eine Sitzung mit SnapCenter ein:

   ```powershell-interactive
   Open-SmConnection
   ```

2. Melden Sie sich mit Ihren Anmeldeinformationen an.

3. EMS-Datenerfassung deaktivieren 

   ```powershell-interactive
   Disable-SmDataCollectionEms
   ```
   
## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie SnapCenter konfigurieren können:

> [!div class="nextstepaction"]
> [Konfigurieren von SnapCenter](configure-snapcenter-oracle-baremetal.md)
