---
title: 'Verwalten von Firewallregeln – Azure-Portal – Azure Database for PostgreSQL: Flexible Server'
description: 'Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL: Flexible Server im Azure-Portal'
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 8c107b88f964d8e657d6833fc3a2e8425d9053ce
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90930301"
---
# <a name="create-and-manage-firewall-rules-for-azure-database-for-postgresql---flexible-server-using-the-azure-portal"></a>Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL: Flexible Server im Azure-Portal

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

Azure Database for PostgreSQL: Flexible Server unterstützt zwei Arten von sich gegenseitig ausschließenden Netzwerkverbindungsmethoden, mit denen eine Verbindung mit Ihrer Flexible Server-Instanz hergestellt werden kann. Die zwei Optionen sind:

* Öffentlicher Zugriff (zugelassene IP-Adressen)
* Privater Zugriff (VNET-Integration)

In diesem Artikel konzentrieren wir uns auf die Erstellung eines PostgreSQL-Servers mit **Öffentlicher Zugriff (zulässige IP-Adressen)** unter Verwendung des Azure-Portals und bieten eine Übersicht über die Verwaltung von Firewallregeln nach der Erstellung einer Flexible Server-Instanz. Mit *Öffentlicher Zugriff (zulässige IP-Adressen)* werden die Verbindungen ausschließlich auf zulässige IP-Adressen beschränkt. Die Client-IP-Adressen müssen in Firewallregeln zugelassen werden. Weitere Informationen finden Sie unter [Öffentlicher Zugriff (zugelassene IP-Adressen)](./concepts-networking.md#public-access-allowed-ip-addresses). Die Firewallregeln können zum Zeitpunkt der Servererstellung definiert werden (empfohlen), können aber auch später hinzugefügt werden. In diesem Artikel erhalten Sie eine Übersicht über das Erstellen und Verwalten von Firewallregeln mithilfe von „Öffentlicher Zugriff (zulässige IP-Adressen)“.

## <a name="create-a-firewall-rule-when-creating-a-server"></a>Erstellen einer Firewallregel beim Erstellen eines Servers

1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.
2. Wählen Sie **Datenbanken** > **Azure-Datenbank für PostgreSQL** aus. Sie können auch **PostgreSQL** in das Suchfeld eingeben, um nach dem Dienst zu suchen.
3. Wählen Sie **Flexible Server** als Bereitstellungsoption aus.
4. Füllen Sie das **Basisformular** aus.
5. Wechseln Sie zur Registerkarte **Netzwerk**, um zu konfigurieren, wie Sie die Verbindung mit dem Server hergestellt werden soll.
6. Wählen Sie unter **Verbindungsmethode** die Option *Öffentlicher Zugriff (zulässige IP-Adressen)* aus. Um die **Firewallregeln** zu erstellen, geben Sie den Namen der Firewallregel und eine einzelne IP-Adresse oder einen Adressbereich an. Wenn Sie die Regel auf eine einzelne IP-Adresse beschränken möchten, geben Sie dieselbe Adresse in das Feld für IP-Startadresse und IP-Endadresse ein. Mit dem Öffnen der Firewall wird es Administratoren, Benutzern und Anwendungen ermöglicht, auf dem PostgreSQL-Server auf alle Datenbanken zuzugreifen, für die sie über gültige Anmeldeinformationen verfügen.
   > [!Note]
   > Azure Database for PostgreSQL: Flexible Server erstellt eine Firewall auf Serverebene. Sie verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – es sei denn, Sie erstellen eine Regel, um die Firewall für bestimmte IP-Adressen zu öffnen.
7. Wählen Sie **Überprüfen + erstellen** aus, um Ihre Flexible Server-Konfiguration zu überprüfen.
8.  Wählen Sie **Erstellen** aus, um den Server bereitzustellen. Die Bereitstellung kann einige Minuten dauern.

## <a name="create-a-firewall-rule-after-server-is-created"></a>Erstellen einer Firewallregel nach der Erstellung des Servers

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Azure Database for PostgreSQL: Flexible Server-Instanz aus, zu der Sie Firewallregeln hinzufügen möchten.
2. Klicken Sie auf der Flexible Server-Seite unter der Überschrift **Einstellungen** auf **Netzwerk**, um die Seite „Netzwerk“ für die Flexible Server-Instanz zu öffnen.

   <!--![Azure portal - click Connection Security](./media/howto-manage-firewall-portal/1-connection-security.png)-->

3. Klicken Sie in den Firewallregeln auf **Aktuelle Client-IP-Adresse hinzufügen**. Dadurch wird automatisch eine Firewallregel mit der öffentlichen IP-Adresse Ihres Computers (gemäß Erkennung durch das Azure-System) erstellt.

   <!--![Azure portal - click Add My IP](./media/howto-manage-firewall-portal/2-add-my-ip.png)-->

4. Überprüfen Sie Ihre IP-Adresse, bevor Sie die Konfiguration speichern. In einigen Situationen weicht die vom Azure-Portal erkannte IP-Adresse von der IP-Adresse ab, die für den Zugriff auf das Internet und die Azure-Server verwendet wird. Aus diesem Grund müssen Sie die Start-IP-Adresse und die End-IP-Adresse ändern, damit die Regel wie erwartet funktioniert.

   Sie können eine Suchmaschine oder ein anderes Onlinetool verwenden, um Ihre eigene IP-Adresse zu überprüfen. Suchen Sie beispielsweise nach „Wie lautet meine IP?“.

   <!--![Bing search for What is my IP](./media/howto-manage-firewall-portal/3-what-is-my-ip.png)-->

5. Fügen Sie weitere Adressräume hinzu. In den Firewallregeln für Azure Database for PostgreSQL: Flexible Server können Sie eine einzelne IP-Adresse oder einen Adressbereich angeben. Wenn Sie die Regel auf eine einzelne IP-Adresse beschränken möchten, geben Sie dieselbe Adresse in das Feld für IP-Startadresse und IP-Endadresse ein. Mit dem Öffnen der Firewall wird es Administratoren, Benutzern und Anwendungen ermöglicht, auf dem PostgreSQL-Server auf alle Datenbanken zuzugreifen, für die sie über gültige Anmeldeinformationen verfügen.

   <!--![Azure portal - firewall rules](./media/howto-manage-firewall-portal/4-specify-addresses.png)-->

6. Klicken Sie auf der Symbolleiste auf **Speichern**, um diese Firewallregel zu speichern. Warten Sie auf die Bestätigung, dass das Update für die Firewallregeln erfolgreich war.

   <!--![Azure portal - click Save](./media/howto-manage-firewall-portal/5-save-firewall-rule.png)-->

## <a name="connecting-from-azure"></a>Herstellen einer Verbindung über Azure

Vielleicht möchten Sie Ressourcen oder Anwendungen, die in Azure bereitgestellt werden, das Verbinden mit Ihrer Flexible Server-Instanz ermöglichen. Dazu gehören in Azure App Service gehostete Webanwendungen, die auf einer Azure-VM, einem Datenmanagementgateway von Azure Data Factory und vielen anderen Komponenten ausgeführt werden. 

Wenn eine Anwendung innerhalb von Azure versucht, eine Verbindung mit Ihrem Server herzustellen, prüft die Firewall, ob Azure-Verbindungen zulässig sind. Sie können diese Einstellung aktivieren, indem Sie im Portal auf der Registerkarte **Netzwerk** die Option **Von Azure-Diensten und -Ressourcen in Azure aus öffentlichen Zugriff auf diesen Server gestatten** auswählen und auf **Speichern** klicken.

Die Ressourcen müssen sich nicht im gleichen virtuellen Netzwerk (VNET) oder in der gleichen Ressourcengruppe für die Firewallregel befinden, um diese Verbindungen zu ermöglichen. Ist der Verbindungsversuch nicht zulässig, erreicht die Anforderung die Azure Database for PostgreSQL: Flexible Server-Instanz nicht.

> [!IMPORTANT]
> Diese Option konfiguriert die Firewall so, dass sie alle von Azure ausgehenden Verbindungen zulässt, auch Verbindungen von den Abonnements anderer Kunden. Wenn Sie diese Option auswählen, stellen Sie sicher, dass die Anmelde- und die Benutzerberechtigungen den Zugriff nur auf autorisierte Benutzer beschränken.
>
> Für den sicheren Zugriff auf die Flexible Server-Instanz sollten Sie **Privater Zugriff (VNET-Integration)** auswählen.
>
## <a name="manage-existing-firewall-rules-through-the-azure-portal"></a>Verwalten vorhandener Firewallregeln im Azure-Portal

Wiederholen Sie die folgenden Schritte zum Verwalten der Firewallregeln.

- Klicken Sie zum Hinzufügen des aktuellen Computers auf **+ Aktuelle Client-IP-Adresse hinzufügen** in den Firewallregeln. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
- Um zusätzliche IP-Adressen hinzuzufügen, geben Sie den Regelnamen, die Start-IP-Adresse und die End-IP-Adresse ein. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
- Um eine vorhandene Regel zu ändern, klicken Sie auf eines der Felder in der Regel, und ändern Sie den betreffenden Wert. Klicken Sie zum Speichern der Änderungen auf **Speichern**.
- Um eine vorhandene Regel zu löschen, klicken Sie auf die Auslassungspunkte [...] und dann auf **Löschen**. Klicken Sie zum Speichern der Änderungen auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
- Hier erfahren Sie mehr über [Netzwerke in Azure Database for PostgreSQL – Flexible Server](./concepts-networking.md).
- Erfahren Sie mehr über [Firewallregeln für Azure Database for PostgreSQL: Flexible Server](./concepts-networking.md#public-access-allowed-ip-addresses).
- [Erstellen und Verwalten von Firewallregeln für Azure Database for PostgreSQL mithilfe der Azure CLI](./how-to-manage-firewall-cli.md).