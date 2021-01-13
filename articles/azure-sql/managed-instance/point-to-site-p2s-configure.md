---
title: Konfigurieren der Point-to-Site-Konnektivität mit SSMS
titleSuffix: Azure SQL Managed Instance
description: Stellen Sie mithilfe von SQL Server Management Studio (SSMS) und einer Point-to-Site-Verbindung eine Verbindung von einem lokalen Clientcomputer zu einer verwalteten Azure SQL-Instanz her.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, jovanpop
ms.date: 03/13/2019
ms.openlocfilehash: 3baf2d7ed7c326895ae40948fc2d0a4cc03021f9
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92788365"
---
# <a name="quickstart-configure-a-point-to-site-connection-to-azure-sql-managed-instance-from-on-premises"></a>Schnellstart: Konfigurieren einer Point-to-Site-Verbindung von einem lokalen Computer zu einer verwalteten Azure SQL-Instanz
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In dieser Schnellstartanleitung wird gezeigt, wie Sie von einem lokalen Clientcomputer eine Verbindung zu einer verwalteten Azure SQL-Instanz herstellen und dazu [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS) und eine Point-to-Site-Verbindung verwenden. Weitere Informationen zu Point-to-Site-Verbindungen finden Sie unter [Informationen zu Point-to-Site-VPN](../../vpn-gateway/point-to-site-about.md).

## <a name="prerequisites"></a>Voraussetzungen

In dieser Schnellstartanleitung gilt Folgendes:

- Als Ausgangspunkt werden die in [Erstellen einer verwalteten Instanz](instance-create-quickstart.md) erstellten Ressourcen verwendet.
- Erforderlich sind PowerShell 5.1 und Azure PowerShell 1.4.0 oder höher auf Ihrem lokalen Clientcomputer. Sehen Sie sich ggf. die Anweisungen zum [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps#install-the-azure-powershell-module) an.
- Erforderlich ist die neueste Version von [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS) auf Ihrem lokalen Clientcomputer.

## <a name="attach-a-vpn-gateway-to-a-virtual-network"></a>Anfügen eines VPN-Gateways an ein virtuelles Netzwerk

1. Öffnen Sie PowerShell auf Ihrem lokalen Clientcomputer.

2. Kopieren Sie dieses PowerShell-Skript. Mit diesem Skript wird ein VPN-Gateway an das virtuelle Netzwerk der verwalteten SQL-Instanz angefügt, die Sie in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](instance-create-quickstart.md) erstellt haben. Dieses Skript verwendet das Az-Modul von Azure PowerShell und führt für Windows- oder Linux-basierte Hosts die folgenden Aktionen aus:

   - Erstellen und Installieren von Zertifikaten auf einem Clientcomputer
   - Berechnen des Subnetz-IP-Adressbereichs für den zukünftigen VPN-Gateway
   - Erstellen des Gatewaysubnetzes
   - Bereitstellen der Azure Resource Manager-Vorlage zum Anfügen des VPN-Gateways an das VPN-Subnetz

     ```powershell
     $scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/attach-vpn-gateway'

     $parameters = @{
       subscriptionId = '<subscriptionId>'
       resourceGroupName = '<resourceGroupName>'
       virtualNetworkName = '<virtualNetworkName>'
       certificateNamePrefix  = '<certificateNamePrefix>'
       }

     Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/attachVPNGateway.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters, $scriptUrlBase
     ```

3. Fügen Sie das Skript in Ihr PowerShell-Fenster ein, und geben Sie die erforderlichen Parameter an. Die Werte für `<subscriptionId>`, `<resourceGroup>` und `<virtualNetworkName>` sollten den Werten entsprechen, die in der Schnellstartanleitung [Erstellen einer verwalteten Instanz](instance-create-quickstart.md) verwendet wurden. Der Wert für `<certificateNamePrefix>` kann eine Zeichenfolge Ihrer Wahl sein.

4. Führen Sie das PowerShell-Skript aus.

> [!IMPORTANT]
> Setzen Sie den Vorgang nicht fort, bis das PowerShell-Skript abgeschlossen ist.

## <a name="create-a-vpn-connection"></a>Erstellen einer VPN-Verbindung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Öffnen Sie die Ressourcengruppe, in der Sie den Gateway für das virtuelle Netzwerk erstellt haben, und öffnen Sie die Gatewayressource für das virtuelle Netzwerk.
3. Klicken Sie auf **Punkt-zu-Standort-Konfiguration** und dann auf **VPN-Client herunterladen** .

    ![VPN-Client herunterladen](./media/point-to-site-p2s-configure/download-vpn-client.png)  
4. Extrahieren Sie auf Ihrem lokalen Clientcomputer die Dateien aus der ZIP-Datei, und öffnen Sie dann den Ordner mit den extrahierten Dateien.
5. Öffnen Sie den Ordner **WindowsAmd64** und dann die Datei **VpnClientSetupAmd64.exe** .
6. Wenn Sie die Nachricht **Windows hat Ihren PC geschützt** erhalten, klicken Sie auf **Weitere Informationen** und dann auf **Trotzdem ausführen** .

    ![Installieren des VPN-Clients](./media/point-to-site-p2s-configure/vpn-client-defender.png)
7. Klicken Sie im Dialogfeld für die Benutzerkontensteuerung auf **Ja** , um den Vorgang fortzusetzen.
8. Wählen Sie in dem Dialogfeld, das auf Ihr virtuelles Netzwerk verweist, **Ja** aus, um den VPN-Client für Ihr virtuelles Netzwerk zu installieren.

## <a name="connect-to-the-vpn-connection"></a>Verbinden mit der VPN-Verbindung

1. Wechseln Sie auf Ihrem lokalen Clientcomputer unter **Netzwerk und Internet** zu **VPN** , und wählen Sie das virtuelle Netzwerk Ihrer verwalteten SQL-Instanz aus, um eine Verbindung zu diesem virtuellen Netzwerk herzustellen. In der folgenden Abbildung heißt das virtuelle Netzwerk **MyNewVNet** .

    ![VPN-Verbindung](./media/point-to-site-p2s-configure/vpn-connection.png)  
2. Wählen Sie **Verbinden** .
3. Klicken Sie im Dialogfeld auf **Verbinden** .

    ![Screenshot mit hervorgehobener Schaltfläche „Verbinden“](./media/point-to-site-p2s-configure/vpn-connection2.png)  
4. Klicken Sie in der Eingabeaufforderung, die besagt, dass der Verbindungs-Manager zum Aktualisieren Ihrer Routingtabelle erhöhte Rechte benötigt, auf **Weiter** .
5. Klicken Sie im Dialogfeld für die Benutzerkontensteuerung auf **Ja** , um den Vorgang fortzusetzen.

   Sie haben eine VPN-Verbindung zum virtuellen Netzwerk Ihrer verwalteten SQL-Instanz hergestellt.

    ![Screenshot mit hervorgehobener Meldung „Verbunden“, nachdem Sie die Verbindung hergestellt haben](./media/point-to-site-p2s-configure/vpn-connection-succeeded.png)  

## <a name="connect-with-ssms"></a>Herstellen einer Verbindung mit SSMS

1. Öffnen Sie auf dem lokalen Clientcomputer SQL Server Management Studio (SSMS).
2. Geben Sie im Dialogfeld **Mit Server verbinden** im Feld **Servername** den vollqualifizierten **Hostnamen** für Ihre verwaltete Instanz ein.
3. Wählen Sie **SQL Server-Authentifizierung** aus, geben Sie Ihren Benutzernamen und Ihr Kennwort ein, und wählen Sie anschließend **Verbinden** aus.

    ![SSMS-Verbindung](./media/point-to-site-p2s-configure/ssms-connect.png)  

Nachdem die Verbindung hergestellt wurde, können Sie Ihre System- und Benutzerdatenbanken im Knoten „Datenbanken“ anzeigen. Sie können auch verschiedene Objekte in den Knoten für Sicherheit, Serverobjekte, Replikation, Verwaltung, SQL Server-Agent und XEvent-Profilerstellung anzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Schnellstartanleitung, die zeigt, wie Sie eine Verbindung von einem virtuellen Azure-Computer herstellen, finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung](point-to-site-p2s-configure.md).
- Eine Übersicht über die Verbindungsoptionen für Anwendungen finden Sie unter [Verbinden Ihrer Anwendungen mit der verwalteten SQL-Instanz](connect-application-instance.md).
- Zum Wiederherstellen einer vorhandenen SQL Server-Datenbank von einer lokalen Instanz in eine verwaltete Instanz können Sie entweder [Azure Database Migration Service für die Migration](../../dms/tutorial-sql-server-to-managed-instance.md) oder den [T-SQL-Befehl „RESTORE“](restore-sample-database-quickstart.md) zum Wiederherstellen aus einer Datenbanksicherungsdatei verwenden.