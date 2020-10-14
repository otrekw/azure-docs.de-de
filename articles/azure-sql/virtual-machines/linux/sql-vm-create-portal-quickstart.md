---
title: 'Schnellstart: Erstellen einer Linux-basierten SQL Server-VM in Azure'
description: In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal einen virtuellen Linux-Computer mit SQL Server 2017 erstellen.
services: virtual-machines-linux
author: MashaMSFT
ms.date: 10/22/2019
tags: azure-service-management
ms.topic: quickstart
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: e1a9d2722987464b1bb3c8b1489a2d1258a41d15
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91273082"
---
# <a name="provision-a-linux-virtual-machine-running-sql-server-in-the-azure-portal"></a>Bereitstellen eines virtuellen Linux-Computers, auf dem SQL Server ausgeführt wird, im Azure-Portal
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Linux](sql-vm-create-portal-quickstart.md)
> * [Windows](../windows/sql-vm-create-portal-quickstart.md)

In diesem Schnellstarttutorial erstellen Sie über das Azure-Portal eine Linux-VM mit SQL Server 2017. Hier lernen Sie Folgendes: 


* [Erstellen eines virtuellen Linux-Computers, auf dem SQL Server ausgeführt wird, über den Katalog](#create)
* [Herstellen einer Verbindung mit dem neuen virtuellen Computer per SSH](#connect)
* [Ändern des Systemadministratorkennworts](#password)
* [Konfigurieren für Remoteverbindungen](#remote)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a name="create-a-linux-vm-with-sql-server-installed"></a><a id="create"></a> Erstellen eines virtuellen Linux-Computers mit SQL Server

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie im linken Bereich **Ressource erstellen**.

1. Wählen Sie im Bereich **Ressource erstellen** die Option **Compute** aus.

1. Klicken Sie neben der Überschrift **Ausgewählte** auf **Alle anzeigen**.

   ![Alle VM-Images anzeigen](./media/sql-vm-create-portal-quickstart/azure-compute-blade.png)

1. Geben Sie **SQL Server 2019** in das Suchfeld ein, und starten Sie die Suche durch Drücken der **EINGABETASTE**.

1. Schränken Sie die Suchergebnisse ein, indem Sie **Betriebssystem** > **RedHat** auswählen.

    ![Suchfilter für VM-Images mit SQL Server 2019](./media/sql-vm-create-portal-quickstart/searchfilter.png)

1. Wählen Sie in den Suchergebnissen ein Linux-Image mit SQL Server 2019 aus. In diesem Tutorial wird **SQL Server 2019 auf RHEL74** verwendet.

   > [!TIP]
   > Mit der Developer Edition können Sie für Entwicklungs- und Testaufgaben die Features der Enterprise Edition nutzen, ohne das SQL Server-Lizenzkosten anfallen. Sie zahlen nur für die Ausführung des virtuellen Linux-Computers.

1. Klicken Sie auf **Erstellen**. 


### <a name="set-up-your-linux-vm"></a>Einrichten Ihrer Linux-VM

1. Wählen Sie auf der Registerkarte **Grundlagen** Ihr **Abonnement** und Ihre **Ressourcengruppe** aus. 

    ![Fenster „Grundlagen“](./media/sql-vm-create-portal-quickstart/basics.png)

1. Geben Sie im Feld **Name des virtuellen Computers** einen Namen für die neue Linux-VM ein.
1. Geben Sie dann die folgenden Werte ein, oder wählen Sie sie aus:
   * **Region**: Wählen Sie die für Sie geeignete Azure-Region aus.
   * **Verfügbarkeitsoptionen**: Wählen Sie die Verfügbarkeits- und Redundanzoption aus, die für Ihre Apps und Daten am besten geeignet ist.
   * **Größe ändern**: Wählen Sie diese Option aus, um eine VM-Größe auszuwählen, und klicken Sie anschließend auf **Auswählen**. Weitere Informationen zu VM-Größen finden Sie unter [VM-Größen](../../../virtual-machines/sizes.md).

     ![Auswählen einer VM-Größe](./media/sql-vm-create-portal-quickstart/vmsizes.png)

   > [!TIP]
   > Verwenden Sie für Entwicklungsaufgaben und Funktionstests mindestens die VM-Größe **DS2**. Für Leistungstests sollten Sie mindestens **DS13** verwenden.

   * **Authentifizierungstyp:** Wählen Sie **Öffentlicher SSH-Schlüssel** aus.

     > [!Note]
     > Für die Authentifizierung können Sie einen öffentlichen SSH-Schlüssel oder ein Kennwort verwenden. SSH ist sicherer. Wie Sie einen SSH-Schlüssel generieren, erfahren Sie unter [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](../../../virtual-machines/linux/mac-create-ssh-keys.md).

   * **Benutzername**: Geben Sie den Administratornamen für die VM ein.
   * **Öffentlicher SSH-Schlüssel**: Geben Sie Ihren öffentlichen RSA-Schlüssel ein.
   * **Öffentliche Eingangsports**: Wählen Sie **Ausgewählte Ports zulassen** aus, und wählen Sie dann in der Liste **Öffentliche Eingangsports hinzufügen** den Port **SSH (22)** aus. Dieser Schritt ist in diesem Schnellstart notwendig, um eine Verbindung herzustellen und die SQL Server-Konfiguration durchzuführen. Wenn Sie eine Remoteverbindung mit SQL Server herstellen möchten, müssen Sie den Datenverkehr an dem von Microsoft SQL Server verwendeten Standardport (1433) für Verbindungen über das Internet nach der Erstellung des virtuellen Computers manuell zulassen.

     ![Eingehende Ports](./media/sql-vm-create-portal-quickstart/port-settings.png)

1. Nehmen Sie auf den folgenden Registerkarten die gewünschten Änderungen an den Einstellungen vor, oder übernehmen Sie die Standardeinstellungen.
    * **Datenträger**
    * **Netzwerk**
    * **Verwaltung**
    * **Gastkonfiguration**
    * **Tags**

1. Klicken Sie auf **Überprüfen + erstellen**.
1. Klicken Sie im Bereich **Bewerten + erstellen** auf **Erstellen**.

## <a name="connect-to-the-linux-vm"></a><a id="connect"></a> Herstellen einer Verbindung mit dem virtuellen Linux-Computer

Wenn Sie bereits über eine BASH-Shell verfügen, verwenden Sie den Befehl **ssh**, um eine Verbindung mit dem virtuellen Azure-Computer herzustellen. Ersetzen Sie im folgenden Befehl den VM-Benutzernamen und die IP-Adresse, um eine Verbindung mit Ihrem virtuellen Linux-Computer herzustellen.

```bash
ssh azureadmin@40.55.55.555
```

Die IP-Adresse Ihres virtuellen Computers finden Sie im Azure-Portal.

![IP-Adresse im Azure-Portal](./media/sql-vm-create-portal-quickstart/vmproperties.png)

Wenn Sie mit Windows arbeiten und nicht über eine BASH-Shell verfügen, installieren Sie einen SSH-Client (beispielsweise PuTTY).

1. [Laden Sie PuTTY herunter, und installieren Sie es.](https://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

1. Führen Sie PuTTY aus.

1. Geben Sie im Konfigurationsbildschirm von PuTTY die öffentliche IP-Adresse Ihres virtuellen Computers ein.

1. Wählen Sie **Öffnen** aus, und geben Sie Ihren Benutzernamen und Ihr Kennwort ein.

Weitere Informationen zum Herstellen einer Verbindung mit virtuellen Linux-Computern finden Sie unter [Erstellen einer Linux-VM mit dem Azure-Portal](../../../virtual-machines/linux/quick-create-portal.md).

> [!NOTE]
> Falls eine PuTTY-Sicherheitswarnung angezeigt wird, dass der Hostschlüssel des Servers nicht in der Registrierung zwischengespeichert wird, wählen Sie eine der folgenden Optionen aus. Wenn Sie diesem Host vertrauen, wählen Sie **Ja** aus, um den Schlüssel dem Cache von PuTTy hinzuzufügen und die Verbindungsherstellung fortzusetzen. Wenn Sie die Verbindung nur einmal herstellen möchten, ohne den Schlüssel dem Cache hinzuzufügen, wählen Sie **Nein** aus. Wenn Sie diesem Host nicht vertrauen, wählen Sie **Abbrechen** aus, um die Verbindungsherstellung abzubrechen.

## <a name="change-the-sa-password"></a><a id="password"></a> Ändern des Systemadministratorkennworts

Der neue virtuelle Computer installiert SQL Server mit einem zufälligen Systemadministratorkennwort. Setzen Sie dieses Kennwort zurück, bevor Sie mit der Systemadministratoranmeldung eine Verbindung mit SQL Server herstellen.

1. Öffnen Sie nach dem Herstellen einer Verbindung mit Ihrem virtuellen Linux-Computer ein neues Befehlsterminal.

1. Ändern Sie das Systemadministratorkennwort mithilfe der folgenden Befehle:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```

   Geben Sie ein neues Systemadministratorkennwort und eine Kennwortbestätigung ein, wenn Sie dazu aufgefordert werden.

1. Starten Sie den SQL Server-Dienst neu.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="add-the-tools-to-your-path-optional"></a>Hinzufügen der Tools zu Ihrem Pfad (optional)

Einige [SQL Server-Pakete](sql-server-on-linux-vm-what-is-iaas-overview.md#packages) werden standardmäßig installiert. Hierzu zählt auch das Paket mit den SQL Server-Befehlszeilentools. Das Toolpaket enthält die Tools **sqlcmd** und **bcp**. Der Einfachheit halber können Sie optional den Toolpfad (`/opt/mssql-tools/bin/`) Ihrer **PATH**-Umgebungsvariablen hinzufügen.

1. Führen Sie die folgenden Befehle aus, um **PATH** sowohl für Anmeldesitzungen als auch für interaktive Sitzungen oder Sitzungen ohne Anmeldung zu ändern:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="configure-for-remote-connections"></a><a id="remote"></a> Konfigurieren für Remoteverbindungen

Wenn Sie mit SQL Server auf dem virtuellen Azure-Computer eine Remoteverbindung herstellen möchten, müssen Sie in der Netzwerksicherheitsgruppe eine Eingangsregel konfigurieren. Die Regel lässt Datenverkehr an dem Port zu, an dem SQL Server lauscht (standardmäßig 1433). In den folgenden Schritten wird die Vorgehensweise mit dem Azure-Portal gezeigt:

> [!TIP]
> Wenn Sie bei der Bereitstellung den eingehenden Port **MS SQL (1433)** in den Einstellungen ausgewählt haben, sind diese Änderungen für Sie relevant. Sie können mit dem nächsten Abschnitt zum Konfigurieren der Firewall fortfahren.

1. Wählen Sie im Portal **Virtuelle Computer**, und wählen Sie dann die SQL Server-VM aus.
1. Wählen Sie im linken Navigationsbereich unter **Einstellungen** die Option **Netzwerk** aus.
1. Wählen Sie im Fenster „Netzwerk“ unter **Regeln für eingehende Ports** die Option **Eingehenden Port hinzufügen** aus.

   ![Regeln für eingehende Ports](./media/sql-vm-create-portal-quickstart/networking.png)

1. Wählen Sie in der Liste **Dienst** die Option **MS SQL** aus.

    ![MS SQL-Sicherheitsgruppenregel](./media/sql-vm-create-portal-quickstart/sqlnsgrule.png)

1. Klicken Sie auf **OK** , um die Regel für den virtuellen Computer zu speichern.

### <a name="open-the-firewall-on-rhel"></a>Öffnen der Firewall in RHEL

In diesem Tutorial wurde die Erstellung eines virtuellen RHEL-Computers (Red Hat Enterprise Linux) erläutert. Wenn Sie mit virtuellen RHEL-Computern eine Remoteverbindung herstellen möchten, müssen Sie in der Linux-Firewall den Port 1433 öffnen.

1. [Stellen Sie eine Verbindung mit Ihrem virtuellen RHEL-Computer her.](#connect)

1. Führen Sie in der BASH-Shell die folgenden Befehle aus:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über einen virtuellen Azure-Computer mit SQL Server 2017 verfügen, können Sie mit **sqlcmd** eine lokale Verbindung herstellen und Transact-SQL-Abfragen ausführen.

Wenn Sie die Azure-VM für SQL Server-Remoteverbindungen konfiguriert haben, sollte das Herstellen einer Remoteverbindung möglich sein. Ein Beispiel dafür, wie Sie unter Windows eine Remoteverbindung mit SQL Server unter Linux herstellen, finden Sie unter [Use SSMS on Windows to connect to SQL Server on Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-ssms) (Verwenden von SSMS unter Windows, um eine Verbindung mit SQL Server unter Linux herzustellen). Informationen zum Herstellen einer Verbindung mit Visual Studio Code finden Sie unter [Use Visual Studio Code to create and run Transact-SQL scripts for SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode) (Erstellen und Ausführen von Transact-SQL-Skripts für SQL Server mithilfe von Visual Studio Code).

Allgemeine Informationen zu SQL Server unter Linux finden Sie in der [Übersicht über SQL Server 2017 unter Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview). Ausführlichere Informationen zur Verwendung virtueller Linux-Computer mit SQL Server 2017 finden Sie in der [Übersicht über virtuelle SQL Server 2017-Computer in Azure](sql-server-on-linux-vm-what-is-iaas-overview.md).
