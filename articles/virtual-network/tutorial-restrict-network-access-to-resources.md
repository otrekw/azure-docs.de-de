---
title: Einschränken des Zugriffs auf PaaS-Ressourcen – Tutorial – Azure-Portal
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe des Azure-Portals den Netzwerkzugriff auf Azure-Ressourcen wie Azure Storage und Azure SQL-Datenbank mit VNET-Dienstendpunkten einschränken können.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: azure-resource-manager
Customer intent: I want only resources in a virtual network subnet to access an Azure PaaS resource, such as an Azure Storage account.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/11/2020
ms.author: kumud
ms.openlocfilehash: cb3a4b6a726ee9163582b15586c65fc750712c63
ms.sourcegitcommit: 1bdcaca5978c3a4929cccbc8dc42fc0c93ca7b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/13/2020
ms.locfileid: "97368244"
---
# <a name="tutorial-restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Tutorial: Einschränken des Netzwerkzugriffs auf PaaS-Ressourcen mit VNET-Dienstendpunkten mithilfe des Azure-Portals

VNET-Dienstendpunkte ermöglichen es Ihnen, den Netzwerkzugriff auf einige Azure-Dienstressourcen auf ein Subnetz eines virtuellen Netzwerks einzuschränken. Sie können auch den Internetzugriff auf die Ressourcen entfernen. Dienstendpunkte ermöglichen eine direkte Verbindung zwischen Ihrem virtuellen Netzwerk und unterstützten Azure-Diensten, sodass Sie mithilfe des privaten Adressraums Ihres virtuellen Netzwerks auf die Azure-Dienste zugreifen können. Datenverkehr, der über Dienstendpunkte für Azure-Ressourcen bestimmt ist, verbleibt immer im Microsoft Azure-Backbonenetzwerk. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks mit einem Subnetz
> * Hinzufügen eines Subnetzes und Aktivieren eines Dienstendpunkts
> * Erstellen einer Azure-Ressource und Zulassen des Netzwerkzugriffs darauf ausschließlich aus einem Subnetz
> * Bereitstellen eines virtuellen Computers für jedes Subnetz
> * Bestätigen des Zugriffs auf eine Ressource aus einem Subnetz
> * Bestätigen, dass der Zugriff auf eine Ressource aus einem Subnetz und dem Internet verweigert wird

Dieser Artikel kann auch mit der [Azure-Befehlszeilenschnittstelle](tutorial-restrict-network-access-to-resources-cli.md) oder mit [Azure PowerShell](tutorial-restrict-network-access-to-resources-powershell.md) durchgearbeitet werden.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Netzwerk** und anschließend **Virtuelle Netzwerke** aus.
3. Klicken Sie auf **+ Hinzufügen**, und geben Sie die folgenden Informationen ein: 

   |Einstellung|Wert|
   |----|----|
   |Subscription| Wählen Sie Ihr Abonnement aus.|
   |Resource group | Klicken Sie auf **Neu erstellen**, und geben Sie *myResourceGroup* ein.|
   |Name| Geben Sie *myVirtualNetwork* ein. |
   |Region| Wählen Sie **USA, Osten** aus. |

   ![Eingeben grundlegender Informationen zu Ihrem virtuellen Netzwerk](./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png)

4. Klicken Sie auf **Weiter: IP-Adressen >**
   
   |Einstellung|Wert|
   |----|----|
   |IPv4-Adressraum| Behalten Sie den Standard bei. |
   |Subnetzname| Klicken Sie auf **Standard**, und ändern Sie den Namen von „Standard“ in „Öffentlich“.|
   |Subnetzadressbereich| Behalten Sie den Standard bei.|

5. Klicken Sie auf **Weiter: Sicherheit >** 
   
   |Einstellung|Wert|
   |----|----|   
   |BastionHost| Deaktivieren|
   |DDoS-Schutz| Deaktivieren|
   |Firewall| Deaktivieren|

6. Wenn Sie fertig sind, klicken Sie auf **Überprüfen und erstellen**.
7. Wenn die Validierungsprüfungen erfolgreich waren, klicken Sie auf **Erstellen**.
8. Warten Sie, bis die Bereitstellung beendet ist, und klicken Sie dann auf **Zu Ressource wechseln**, oder fahren Sie mit dem nächsten Abschnitt fort. 

## <a name="enable-a-service-endpoint"></a>Aktivieren eines Dienstendpunkts

Dienstendpunkte werden pro Dienst und pro Subnetz aktiviert. So erstellen Sie ein Subnetz und fügen einen Dienstendpunkt für das Subnetz hinzu

1. Wenn Sie sich noch nicht auf der Seite der virtuellen Netzwerkressource befinden, können Sie im Feld **Ressourcen, Dienste und Dokumente durchsuchen** oben im Portal nach dem neu erstellten Netzwerk suchen, *myVirtualNetwork* eingeben und es in der Liste auswählen.
2. Wählen Sie im Menü **Einstellungen** (links) die Option **Teilnetze** und dann die Option **+ Subnetz** wie gezeigt aus:

    ![Hinzufügen des Subnetzes](./media/tutorial-restrict-network-access-to-resources/add-subnet.png) 

3. Wählen Sie unter **Subnetz hinzufügen** die folgenden Informationen aus, oder geben Sie sie ein, und klicken Sie auf **OK**:

    |Einstellung|Wert|
    |----|----|
    |Name| Privat |
    |Adressbereich| Behalten Sie den Standard bei.|
    |Dienstendpunkte| Wählen Sie **Microsoft.Storage** aus.|
    |Dienstendpunkt-Richtlinien | Lassen Sie den Standardwert 0 unverändert. |

> [!CAUTION]
> Lesen Sie vor der Aktivierung eines Dienstendpunkts für ein vorhandenes Subnetz, das Ressourcen enthält, die Informationen unter [Ändern von Subnetzeinstellungen](virtual-network-manage-subnet.md#change-subnet-settings).

4. Klicken Sie auf **Speichern**, und schließen Sie dann das Subnetzfenster auf der rechten Seite. Das neu erstellte Subnetz sollte in der Liste angezeigt werden.

## <a name="restrict-network-access-for-a-subnet"></a>Einschränken des Netzwerkzugriffs für ein Subnetz

Standardmäßig können alle Instanzen von virtuellen Computern in einem Subnetz mit allen Ressourcen kommunizieren. Sie können die bidirektionale Kommunikation mit allen Ressourcen in einem Subnetz einschränken, indem Sie eine Netzwerksicherheitsgruppe erstellen und dem Subnetz zuordnen:

1. Wählen Sie links oben im Azure-Portal **Alle Dienste** aus.
2. Wählen Sie die Option **Netzwerk** und dann **Netzwerksicherheitsgruppen** aus (oder suchen Sie danach).
3. Klicken Sie auf der Seite **Netzwerksicherheitsgruppen** auf **+ Hinzufügen**.
4. Geben Sie die folgenden Informationen ein: 

    |Einstellung|Wert|
    |----|----|
    |Subscription| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe | Wählen Sie *myResourceGroup* in der Liste aus.|
    |Name| Geben Sie **myNsgPrivate** ein. |
    |Standort| Wählen Sie **USA, Osten** aus. |

5. Klicken Sie auf **Überprüfen und erstellen**, und wenn die Validierungsprüfung bestanden ist, klicken Sie auf **Erstellen**.
6. Nachdem die Netzwerksicherheitsgruppe erstellt wurde, klicken Sie auf **Zu Ressource wechseln**, oder suchen Sie nach *myNsgPrivate*.
7. Wählen Sie links unter **Einstellungen** die Option **Ausgangssicherheitsregeln** aus.
8. Wählen Sie **+ Hinzufügen**.
9. Erstellen Sie eine Regel, die ausgehende Kommunikation mit dem Azure Storage-Dienst zulässt. Geben Sie die folgenden Informationen ein (oder wählen Sie sie aus), und klicken Sie anschließend auf **Hinzufügen**:

    |Einstellung|Wert|
    |----|----|
    |`Source`| Wählen Sie **VirtualNetwork** aus. |
    |Source port ranges| * |
    |Destination | Wählen Sie **Diensttag** aus.|
    |Zieldiensttag | Wählen Sie **Storage** aus.|
    |Zielportbereiche| Lassen Sie den Standardwert *8080* unverändert. |
    |Protocol|Any|
    |Aktion|Allow|
    |Priority|100|
    |Name|Benennen Sie es in **Allow-Storage-All** um.|

10. Erstellen Sie eine weitere Ausgangssicherheitsregel, die Kommunikation mit dem Internet verweigert. Diese Regel überschreibt eine Standardregel in allen Netzwerksicherheitsgruppen, die ausgehende Internetkommunikation zulässt. Wiederholen Sie die obigen Schritte 6–9 mit folgenden Werten:

    |Einstellung|Wert|
    |----|----|
    |`Source`| Wählen Sie **VirtualNetwork** aus. |
    |Source port ranges| * |
    |Destination | Wählen Sie **Diensttag** aus.|
    |Zieldiensttag| Wählen Sie **Internet** aus.|
    |Zielportbereiche| * |
    |Protocol|Any|
    |Aktion|**Ändern Sie den Standardwert in *Ablehnen*.** |
    |Priority|110|
    |Name|Ändern Sie den Wert in *Deny-Internet-All*.|

11. Erstellen Sie eine *Eingangssicherheitsregel*, die RDP-Datenverkehr (Remote Desktop Protocol) an das Subnetz von überall erlaubt. Die Regel setzt eine Standardsicherheitsregel außer Kraft, die jeglichen eingehenden Verkehr aus dem Internet abweist. Remotedesktopverbindungen in das Subnetz sind zulässig, sodass die Konnektivität in einem späteren Schritt getestet werden kann. 
12. Wählen Sie unter **Einstellungen** die Option **Eingangssicherheitsregeln**.
13. Wählen Sie **+ Hinzufügen** aus, und verwenden Sie die folgenden Werte:

    |Einstellung|Wert|
    |----|----|
    |`Source`| Any |
    |Source port ranges| * |
    |Destination | Wählen Sie **VirtualNetwork** aus.|
    |Zielportbereiche| Ändern Sie den Wert in *3389*. |
    |Protocol|Any|
    |Aktion|Allow|
    |Priority|120|
    |Name|Ändern Sie den Wert in *Allow-RDP-All*.|

   >[!WARNING] 
   > RDP-Port 3389 wird für das Internet verfügbar gemacht. Dies wird nur für Tests empfohlen. Für *Produktionsumgebungen* empfehlen wir die Verwendung einer VPN- oder privaten Verbindung.

1.  Wählen Sie unter **Einstellungen** die Option **Subnetze** aus.
2.  Klicken Sie auf **+ Zuordnen**.
3.  Wählen Sie unter **Virtuelles Netzwerk** die Option **myVirtualNetwork** aus.
4.  Wählen Sie unter **Subnetz** die Option **Private** und dann **OK** aus.

## <a name="restrict-network-access-to-a-resource"></a>Einschränken des Netzwerkzugriffs auf eine Ressource

Die Schritte, die erforderlich sind, um den Netzwerkzugriff auf Ressourcen einzuschränken, die durch Azure-Dienste erstellt und für Dienstendpunkte aktiviert wurden, sind je nach Dienst unterschiedlich. Informationen zu den Schritten für einzelne Dienste finden Sie in der Dokumentation des jeweiligen Diensts. Im weiteren Verlauf dieses Tutorials wird als Beispiel der Netzwerkzugriff für ein Azure Storage-Konto eingeschränkt.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Geben Sie „Speicherkonto“ in die Suchleiste ein, und wählen Sie es in der Dropdownliste aus.
3. Klicken Sie auf **+ Hinzufügen**.
4. Geben Sie Folgendes ein:

    |Einstellung|Wert|
    |----|----|
    |Subscription| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe| Wählen Sie *myResourceGroup* aus.|
    |Speicherkontoname| Geben Sie einen Namen ein, der an allen Azure-Standorten eindeutig, zwischen 3 und 24 Zeichen lang ist und nur aus Ziffern und Kleinbuchstaben besteht.|
    |Standort| Wählen Sie **USA, Osten** aus. |
    |Leistung|Standard|
    |Kontoart| StorageV2 (allgemein, Version 2)|
    |Replikation| Lokal redundanter Speicher (LRS)|

5. Wählen Sie **Erstellen und überprüfen** aus, und wenn die Validierungsprüfungen bestanden sind, klicken Sie auf **Erstellen**. 

>[!NOTE] 
> Die Bereitstellung kann einige Minuten in Anspruch nehmen.

6. Klicken Sie nach dem Erstellen des neuen Speicherkontos auf **Zu Ressource wechseln**.

### <a name="create-a-file-share-in-the-storage-account"></a>Erstellen einer Dateifreigabe im Speicherkonto

1. Wechseln Sie zur Übersichtsseite Ihres Speicherkontos.
2. Wählen Sie das App-Symbol **Dateifreigaben**, und klicken Sie dann auf **+ Dateifreigabe**.

    |Einstellung|Wert|
    |----|----|
    |Name| my-file-share|
    |Kontingent| „Auf Maximum festlegen“ |

   ![Speicherkonto](./media/tutorial-restrict-network-access-to-resources/storage-account.png) 

3. Klicken Sie auf **Erstellen**.
4. Die Dateifreigabe sollte im Azure-Fenster angezeigt werden. Wenn dies nicht der Fall ist, klicken Sie auf **Aktualisieren**.

### <a name="restrict-network-access-to-a-subnet"></a>Einschränken des Netzwerkzugriffs auf ein Subnetz

Standardmäßig akzeptieren Speicherkonten Netzwerkverbindungen von Clients in allen Netzwerken, einschließlich des Internets. Sie können den Zugriff über das Internet sowie alle anderen Subnetze in allen virtuellen Netzwerken einschränken (außer über das Subnetz *Private* im virtuellen Netzwerk *myVirtualNetwork*). So schränken Sie den Netzwerkzugriff auf ein Subnetz ein

1. Wählen Sie unter **Einstellungen** für Ihr (eindeutig benanntes) Speicherkonto die Option **Netzwerke** aus.
2. Klicken Sie auf **Ausgewählte Netzwerke**.
3. Wählen Sie **+ Vorhandenes virtuelles Netzwerk hinzufügen** aus.
4. Wählen Sie unter **Netzwerke hinzufügen** die folgenden Werte und dann **Hinzufügen** aus:

    |Einstellung|Wert|
    |----|----|
    |Subscription| Wählen Sie Ihr Abonnement aus.|
    |Virtuelle Netzwerke| **myVirtualNetwork**|
    |Subnetze| **Privat**|

    ![Screenshot: Bereich „Netzwerke hinzufügen“, in dem Sie die angegebenen Werte eingeben können](./media/tutorial-restrict-network-access-to-resources/virtual-networks.png)

5. Klicken Sie auf **Hinzufügen** und dann sofort auf das Symbol **Speichern**, um die Änderungen zu speichern.
6. Wählen Sie unter **Einstellungen** für das Speicherkonto **Zugriffsschlüssel** aus, wie in der folgenden Abbildung dargestellt:

      ![Screenshot, auf dem unter „Einstellungen“ die Option „Zugriffsschlüssel“ ausgewählt ist, unter der Sie einen Schlüssel abrufen können](./media/tutorial-restrict-network-access-to-resources/storage-access-key.png)

7. Klicken Sie auf **Schlüssel anzeigen**, und notieren Sie sich die Werte von **Schlüssel**, da Sie „Schlüssel1“ in einem späteren Schritt manuell eingeben müssen, wenn Sie die Dateifreigabe einem Laufwerksbuchstaben in einer VM zuordnen.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Zum Testen des Netzwerkzugriffs auf ein Speicherkonto stellen Sie einen virtuellen Computer für jedes Subnetz bereit.

### <a name="create-the-first-virtual-machine"></a>Erstellen des ersten virtuellen Computers

1. Suchen Sie in der Leiste „Ressourcen suchen. . ." nach **Virtuelle Computer**.
2. Wählen Sie **+ Hinzufügen > Virtueller Computer** aus. 
3. Geben Sie die folgenden Informationen ein:

   |Einstellung|Wert|
   |----|----|
   |Subscription| Wählen Sie Ihr Abonnement aus.|
   |Ressourcengruppe| Wählen Sie **myResourceGroup aus, die zuvor erstellt wurde.|
   |Name des virtuellen Computers| Geben Sie *myVmPublic* ein.|
   |Region | (USA) USA, Osten
   |Verfügbarkeitsoptionen| Verfügbarkeitszone|
   |Verfügbarkeitszone | 1 |
   |Image | Windows Server 2019 Datacenter – Gen 1 |
   |Size | Wählen Sie die Größe der VM-Instanz aus, die Sie verwenden möchten. |
   |Username|Geben Sie den gewünschten Benutzernamen ein.|
   |Kennwort| Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
   |Öffentliche Eingangsports | Ausgewählte Ports zulassen |
   |Eingangsports auswählen | Belassen Sie die Standardeinstellung *RDP (3389)* . |

   ![Auswählen eines virtuellen Netzwerks](./media/tutorial-restrict-network-access-to-resources/virtual-machine-settings.png)
  
4. Wählen Sie die Registerkarte **Netzwerk** und dann **myVirtualNetwork** aus. 
5. Wählen Sie das Subnetz *Öffentlich* aus.
6. Wählen Sie unter **NIC-Netzwerksicherheitsgruppe** die Option **Erweitert** aus. Das Portal erstellt automatisch eine Netzwerksicherheitsgruppe, die den Port 3389 zulässt. Dieser Port muss geöffnet werden, um in einem späteren Schritt eine Verbindung mit dem virtuellen Computer herstellen zu können. 

   ![Eingeben von grundlegenden Informationen zu einem virtuellen Computer](./media/tutorial-restrict-network-access-to-resources/virtual-machine-basics.png)

7. Wählen Sie **Überprüfen und erstellen**, dann **Erstellen** aus und warten Sie, bis die Bereitstellung abgeschlossen ist.
8. Klicken Sie auf **Zu Ressource wechseln**, oder öffnen Sie die Seite **Startseite > Virtuelle Computer**, und wählen Sie die soeben erstellte VM *myVmPublic* aus, die gestartet sein sollte.

### <a name="create-the-second-virtual-machine"></a>Erstellen des zweiten virtuellen Computers

1. Führen Sie die Schritte 1–8 erneut aus, aber nennen Sie in Schritt 3 den virtuellen Computer *myVmPrivate*, und legen Sie **Öffentlicher Eingangsport** auf „Kein“ fest. 
2. Wählen Sie in Schritt 4–5 das Subnetz **Private** aus.

>[!NOTE]
> Die Einstellungen **NIC-Netzwerksicherheitsgruppe** und **Öffentlicher Eingangsport** sollten die unten gezeigte Abbildung widerspiegeln, einschließlich des blauen Bestätigungsfensters mit dem Hinweis, dass der gesamte öffentliche Datenverkehr standardmäßig blockiert wird.

   ![Erstellen eines privaten virtuellen Computers](./media/tutorial-restrict-network-access-to-resources/create-private-virtual-machine.png)

3. Wählen Sie **Überprüfen und erstellen**, dann **Erstellen** aus und warten Sie, bis die Bereitstellung abgeschlossen ist. 

>[!WARNING]
> Fahren Sie erst mit dem nächsten Schritt fort, wenn die Bereitstellung abgeschlossen ist.

4. Warten Sie auf das unten gezeigte Bestätigungsfenster, und klicken Sie auf **Zu Ressource wechseln**.

   ![Erstellen eines Bestätigungsfensters für den privaten virtuellen Computer](./media/tutorial-restrict-network-access-to-resources/create-vm-confirmation-window.png)

## <a name="confirm-access-to-storage-account"></a>Bestätigen des Zugriffs auf das Speicherkonto

1. Sobald die der virtuelle Computer *myVmPrivate* erstellt wurde, klicken Sie auf **Zu Ressource wechseln**. 
2. Stellen Sie eine Verbindung zum virtuellen Computer her, indem Sie die Option **Verbinden > RDP** auswählen.
3. Nachdem Sie die Schaltfläche **Verbinden** ausgewählt haben, wird eine RDP-Datei (Remotedesktopprotokoll) erstellt. Klicken Sie auf **RDP-Datei herunterladen**, um die Datei auf Ihren Computer herunterzuladen.  
4. Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben. Unter Umständen müssen Sie auf **Weitere Optionen** und anschließend auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. Geben Sie in das E-Mail-Feld die Anmeldeinformationen für „Administratorkonto: Benutzername“ ein, die Sie zuvor angegeben haben. 
5. Klicken Sie auf **OK**.
6. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn eine Warnung angezeigt wird, klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren. Sie sollten sehen, dass die VM wie gezeigt startet:

   ![Ausgeführten privaten virtuellen Computer anzeigen](./media/tutorial-restrict-network-access-to-resources/virtual-machine-running.png)

7. Öffnen Sie im VM-Fenster eine PowerShell CLI-Instanz.
8. Ordnen Sie mithilfe des folgenden Skripts die Azure-Dateifreigabe dem Laufwerk „Z“ zu, indem Sie PowerShell verwenden. Ersetzen Sie vor dem Ausführen der folgenden Befehle `<storage-account-key>` und die beiden `<storage-account-name>`-Felder durch die von Ihnen zuvor in [Erstellen eines Speicherkontos](#create-a-storage-account) angegebenen und abgerufenen Werte.

   ```powershell
   $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
   $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
   New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
   ```

   PowerShell gibt eine Ausgabe ähnlich der folgenden Beispielausgabe zurück:

   ```powershell
   Name           Used (GB)     Free (GB) Provider      Root
   ----           ---------     --------- --------      ----
   Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
   ```

   Die Azure-Dateifreigabe wurde dem Laufwerk Z erfolgreich zugeordnet.

9.   Schließen Sie die Remotedesktopsitzung mit der VM *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Bestätigen, dass der Zugriff auf das Speicherkonto verweigert wird

1. Geben Sie oben im Portal im Feld *Ressourcen, Dienste und Dokumente durchsuchen* **myVmPublic** ein.
2. Wenn **myVmPublic** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
3. Führen Sie in [Bestätigen des Zugriffs auf das Speicherkonto](#confirm-access-to-storage-account) für die VM *myVmPublic* die obigen Schritte 1–8 aus.

   Nach einer kurzen Wartezeit tritt ein Fehler vom Typ `New-PSDrive : Access is denied` auf. Der Zugriff wird verweigert, da der virtuelle Computer *myVmPublic* im Subnetz *Public* bereitgestellt wird. Das Subnetz *Public* verfügt nicht über einen Dienstendpunkt, der für Azure Storage aktiviert ist. Das Speicherkonto lässt Netzwerkzugriff nur über das Subnetz *Private*, nicht jedoch über das Subnetz *Public* zu.

4. Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmPublic*.
5. Wechseln Sie dann im Azure-Portal zu dem eindeutig benannten Speicherkonto, das Sie zuvor erstellt haben.
6. Wählen Sie unter „Dateidienst“ die Option **Dateifreigaben** und dann die zuvor erstellte Option *my-file-share* aus.
7. Es sollte die folgende Fehlermeldung angezeigt werden:

   ![Zugriffsfehler](./media/tutorial-restrict-network-access-to-resources/access-denied-error.png)
   
>[!NOTE] 
> Der Zugriff wird verweigert, da sich Ihr Computer nicht im Subnetz *Private* des Netzwerks *MyVirtualNetwork* befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen:

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Dienstendpunkt für ein Subnetz eines virtuellen Netzwerks aktiviert. Sie haben erfahren, dass Dienstendpunkte für Ressourcen aktiviert werden können, die über mehrere Azure-Dienste bereitgestellt werden. Sie haben ein Azure Storage-Konto erstellt und den Netzwerkzugriff auf das Speicherkonto ausschließlich auf Ressourcen im Subnetz eines virtuellen Netzwerks eingeschränkt. Weitere Informationen zu Dienstendpunkten finden Sie unter [Dienstendpunkte im virtuellen Netzwerk](virtual-network-service-endpoints-overview.md) und [Hinzufügen, Ändern oder Löschen von Subnetzen virtueller Netzwerke](virtual-network-manage-subnet.md).

Wenn Sie mehrere virtuelle Netzwerke in Ihrem Konto verwenden, können Sie zwei virtuelle Netzwerke miteinander verbinden, damit die Ressourcen in jedem virtuellen Netzwerk miteinander kommunizieren können. Im nächsten Tutorial erfahren Sie, wie Sie Verbindungen zwischen virtuellen Netzwerken herstellen.

> [!div class="nextstepaction"]
> [Herstellen von Verbindungen zwischen virtuellen Netzwerken](./tutorial-connect-virtual-networks-portal.md)
