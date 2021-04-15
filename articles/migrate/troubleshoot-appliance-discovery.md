---
title: Behandeln von Problemen bei der Bereitstellung und Ermittlung der Azure Migrate-Appliance
description: Erhalten Sie Hilfe bei der Bereitstellung einer Appliance und der Ermittlung von Servern.
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: troubleshooting
ms.date: 01/02/2020
ms.openlocfilehash: 995914fab0e7112327ebf6ab8e32fb67181f481e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105608917"
---
# <a name="troubleshoot-the-azure-migrate-appliance-and-discovery"></a>Behandeln von Problemen bei der Azure Migrate-Appliance und der Ermittlung

Dieser Artikel hilft Ihnen bei der Behebung von Problemen bei der Bereitstellung der [Azure Migrate](migrate-services-overview.md)-Appliance und bei der Verwendung der Appliance zur Erkennung von lokalen Servern.

## <a name="whats-supported"></a>Was wird unterstützt?

[Überprüfen](migrate-appliance.md) Sie die Anforderungen an die Applianceunterstützung.

## <a name="invalid-ovf-manifest-entry"></a>„Ungültiger OVF-Manifesteintrag“

Wenn Sie den Fehler „Die angegebene Manifestdatei ist ungültig: Ungültiger OVF-Manifesteintrag“ erhalten, gehen Sie wie folgt vor:

1. Überprüfen Sie, ob die OVA-Datei für die Azure Migrate-Appliance ordnungsgemäß heruntergeladen wird, indem Sie deren Hashwert überprüfen. [Weitere Informationen](./tutorial-discover-vmware.md) Wenn der Hashwert nicht übereinstimmt, laden Sie die OVA-Datei erneut herunter, und wiederholen Sie die Bereitstellung.
2. Wenn immer noch Fehler bei der Bereitstellung auftreten und Sie den VMware vSphere-Client zum Bereitstellen der OVF-Datei verwenden, versuchen Sie, die Bereitstellung über den vSphere-Webclient vorzunehmen. Wenn die Bereitstellung weiterhin fehlschlägt, versuchen Sie es mit einem anderen Webbrowser.
3. Wenn Sie den vSphere-Webclient verwenden und die Bereitstellung auf vCenter Server 6.5 oder 6.7 ausführen möchten, versuchen Sie, die OVA-Datei direkt auf dem ESXi-Host bereitzustellen:
   - Stellen Sie mit dem Webclient (https://<*Host-IP-Adresse*>/ui) eine direkte Verbindung mit dem ESXi-Host her (anstelle von vCenter Server).
   - Wählen Sie unter **Startseite** > **Bestand** die Optionen **Datei** > **OVF-Vorlage bereitstellen** aus. Navigieren Sie zur OVA-Datei, und schließen Sie die Bereitstellung ab.
4. Wenn weiterhin ein Fehler bei der Bereitstellung auftritt, wenden Sie sich an den Azure Migrate-Support.

## <a name="cant-connect-to-the-internet"></a>Verbindung mit dem Internet kann nicht hergestellt werden

Dies kann der Fall sein, wenn sich der Applianceserver hinter einem Proxy befindet.

- Stellen Sie sicher, dass Sie die Anmeldeinformationen für die Autorisierung angeben, wenn der Proxy diese benötigt.
- Wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Verbindungen verwenden, fügen Sie [diese URLs](migrate-appliance.md#url-access) einer Positivliste hinzu.
- Wenn Sie für die Internetverbindung einen abfangenden Proxy verwenden, importieren Sie das Proxyzertifikat [mit diesen Schritten](./migrate-appliance.md) in die Appliance.

## <a name="cant-sign-into-azure-from-the-appliance-web-app"></a>Anmeldung bei Azure über die Appliance-Web-App ist nicht möglich

Wenn Sie das falsche Konto zur Anmeldung bei Azure verwenden, wird der Fehler „Leider können wir Sie nicht anmelden“ angezeigt. Dieser Fehler hat folgende Gründe:

- Sie melden sich bei der Webanwendung der Appliance für die öffentliche Cloud mit den Anmeldeinformationen des Benutzerkontos für das Portal der Government-Cloud an.
- Sie melden sich bei der Webanwendung der Appliance für die Government-Cloud mit den Anmeldeinformationen des Benutzerkontos für das Portal der öffentlichen Cloud an.

Stellen Sie sicher, dass Sie die richtigen Anmeldeinformationen verwenden.

## <a name="datetime-synchronization-error"></a>Datums-/Zeitsynchronisierungsfehler

Ein Fehler bei der Datums- und Zeitsynchronisierung (Fehler 802) weist darauf hin, dass die Serveruhr möglicherweise um mehr als fünf Minuten von der aktuellen Uhrzeit abweicht. Passen Sie die Uhrzeit auf dem Collector-Server an die aktuelle Uhrzeit an:

1. Öffnen Sie auf dem Server eine Administratoreingabeaufforderung.
2. Führen Sie **w32tm /tz** aus, um die Zeitzone zu überprüfen.
3. Führen Sie **w32tm /resync** aus, um die Zeit zu synchronisieren.

## <a name="unabletoconnecttoserver"></a>„UnableToConnectToServer“

Wenn dieser Verbindungsfehler angezeigt wird, können Sie möglicherweise keine Verbindung mit vCenter Server „*Servername*.com:9443“ herstellen. Die Fehlerdetails weisen darauf hin, dass unter `https://\*servername*.com:9443/sdk` kein Endpunkt lauscht, der die Nachricht akzeptieren kann.

- Überprüfen Sie, ob Sie die neueste Version der Appliance ausführen. Ist dies nicht der Fall, führen Sie ein Upgrade der Appliance auf die [neueste Version](./migrate-appliance.md) durch.
- Wenn das Problem bei der neuesten Version weiterhin auftritt, ist die Appliance möglicherweise nicht in der Lage, den angegebenen vCenter Server-Namen aufzulösen, oder der angegebene Port ist eventuell falsch. Wenn der Port nicht angegeben ist, versucht Collector standardmäßig, eine Verbindung mit Port 443 herzustellen.

    1. Versuchen Sie, „*Servername*.com“ von der Appliance aus per Ping zu erreichen.
    2. Wenn Schritt 1 nicht erfolgreich ist, können Sie versuchen, eine Verbindung mit vCenter Server über die IP-Adresse herzustellen.
    3. Ermitteln Sie die richtige Portnummer für die Verbindung mit vCenter Server.
    4. Überprüfen Sie, ob vCenter Server ausgeführt wird und betriebsbereit ist.

## <a name="error-6005260039-appliance-might-not-be-registered"></a>Fehler 60052/60039: Die Appliance ist möglicherweise nicht registriert

- Der Fehler 60052 „Möglicherweise wurde die Appliance nicht erfolgreich beim Projekt registriert“ tritt auf, wenn das zum Registrieren der Appliance verwendete Azure-Konto nicht über ausreichende Berechtigungen verfügt.
    - Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt.
    - Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](./migrate-appliance.md#appliance---vmware).
- Der Fehler 60039 „Möglicherweise wurde die Appliance nicht erfolgreich beim Projekt registriert" kann auftreten, wenn die Registrierung fehlschlägt, da das zum Registrieren der Appliance verwendete Projekt nicht gefunden werden kann.
    - Überprüfen Sie im Azure-Portal, ob das Projekt in der Ressourcengruppe vorhanden ist.
    - Sollte das Projekt nicht vorhanden sein, erstellen Sie in Ihrer Ressourcengruppe ein neues Projekt, und registrieren Sie die Appliance erneut. Informationen zum Erstellen eines neuen Projekts finden Sie [hier](./create-manage-projects.md#create-a-project-for-the-first-time).

## <a name="error-6003060031-key-vault-management-operation-failed"></a>Fehler 60030/60031: Ein Key Vault-Verwaltungsvorgang war nicht erfolgreich

Wenn der Fehler 60030 oder 60031 „Ein Azure Key Vault-Verwaltungsvorgang war nicht erfolgreich“ auftritt, gehen Sie wie folgt vor:

- Stellen Sie sicher, dass das für die Registrierung der Appliance verwendete Azure-Benutzerkonto mindestens über „Mitwirkender“-Berechtigungen für das Abonnement verfügt.
- Vergewissern Sie sich, dass das Konto Zugriff auf den in der Fehlermeldung angegebenen Key Vault hat, und wiederholen Sie dann den Vorgang.
- Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.
- Weitere Informationen zu den erforderlichen Azure-Rollen und -Berechtigungen finden Sie [hier](./migrate-appliance.md#appliance---vmware).

## <a name="error-60028-discovery-couldnt-be-initiated"></a>Fehler 60028: Die Ermittlung konnte nicht initiiert werden

Fehler 60028: „Die Ermittlung konnte aufgrund eines Fehlers nicht initiiert werden. Der Vorgang war für die angegebene Liste von Hosts oder Clustern nicht erfolgreich“. Dieser Fehler weist darauf hin, dass die Ermittlung aufgrund eines Problems beim Zugreifen auf oder Abrufen von Serverinformationen auf den in der Fehlermeldung aufgelisteten Hosts nicht gestartet werden konnte. Die übrigen Hosts wurden erfolgreich hinzugefügt.

- Fügen Sie die in der Fehlermeldung angegebenen Hosts mithilfe der Option **Host hinzufügen** erneut hinzu.
- Gehen Sie im Falle eines Überprüfungsfehlers gemäß der Korrekturanleitung vor, um den Fehler zu beheben, und verwenden Sie dann erneut die Option **Speichern und Ermittlung starten**.

## <a name="error-60025-azure-ad-operation-failed"></a>Fehler 60025: Fehler bei einem Azure AD-Vorgang

Fehler 60025: „Fehler bei einem Azure AD-Vorgang. Der Fehler trat beim Erstellen oder Aktualisieren der Azure AD-Anwendung auf“. Dieser Fehler tritt auf, wenn zum Initiieren der Ermittlung ein anderes Azure-Benutzerkonto als für die Registrierung der Appliance verwendet wird. Führen Sie eines der folgenden Verfahren aus:

- Stellen Sie sicher, dass zum Initiieren der Ermittlung dasselbe Benutzerkonto verwendet wird wie zum Registrieren der Appliance.
- Weisen Sie dem Benutzerkonto, bei dem der Ermittlungsvorgang fehlgeschlagen ist, Zugriffsberechtigungen für die Azure Active Directory-Anwendung zu.
- Löschen Sie die Ressourcengruppe, die zuvor für das Projekt erstellt wurde. Erstellen Sie eine andere Ressourcengruppe, um neu zu beginnen.
- Weitere Informationen zu den Azure Active Directory-Anwendungsberechtigungen finden Sie [hier](./migrate-appliance.md#appliance---vmware).

## <a name="error-50004-cant-connect-to-host-or-cluster"></a>Fehler 50004: Verbindung mit Host oder Cluster nicht möglich

Fehler 50004: „Es ist keine Verbindung mit einem Host oder Cluster möglich, da der Servername nicht aufgelöst werden kann. WinRM-Fehlercode: 0x803381B9“. Dieser Fehler tritt auf, wenn der Azure DNS-Dienst für die Appliance den von Ihnen angegebenen Cluster- oder Hostnamen nicht auflösen kann.

- Wenn Sie diesen Fehler auf dem Cluster sehen, versuchen Sie, den vollqualifizierten Domänennamen (FQDN) des Clusters anzugeben.
- Sie sehen diesen Fehler eventuell auch für Hosts in einem Cluster. In diesem Fall kann die Appliance zwar eine Verbindung mit dem Cluster herstellen, doch gibt der Cluster Hostnamen zurück, bei denen es sich nicht um FQDNs handelt. Um diesen Fehler zu beheben, aktualisieren Sie die „hosts“-Datei auf der Appliance und fügen Sie ein Mapping von IP-Adresse und Hostnamen hinzu:
    1. Öffnen Sie Notepad als Administrator.
    2. Öffnen Sie die Datei „C:\Windows\System32\Drivers\etc\hosts“.
    3. Fügen Sie die IP-Adresse und den Hostnamen in einer Zeile hinzu. Wiederholen Sie dies für jeden Host oder Cluster, bei dem dieser Fehler angezeigt wird.
    4. Speichern und schließen Sie die Datei „hosts“.
    5. Überprüfen Sie mithilfe der Applianceverwaltungs-App, ob die Appliance eine Verbindung mit den Hosts herstellen kann. Nach 30 Minuten sollten die neuesten Informationen für diese Hosts im Azure-Portal angezeigt werden.

## <a name="error-60001-unable-to-connect-to-server"></a>Fehler 60001: Verbindung mit dem Server konnte nicht hergestellt werden.

- Stellen Sie sicher, dass eine Verbindung zwischen der Appliance und dem Server besteht.
- Wenn es sich um einen Linux-Server handelt, stellen Sie sicher, dass die kennwortbasierte Authentifizierung mithilfe der folgenden Schritte aktiviert ist:
    1. Melden Sie sich beim Linux-Server an, und öffnen Sie die SSH-Konfigurationsdatei mit dem Befehl „vi /etc/ssh/sshd_config“.
    2. Setzen Sie die Option „PasswordAuthentication“ auf „Ja“. Speichern Sie die Datei .
    3. Starten Sie den SSH-Dienst durch Ausführen von „service sshd restart“ neu.
- Wenn es sich um einen Windows-Server handelt, vergewissern Sie sich, dass Port 5985 geöffnet ist, um WMI-Remoteaufrufe zuzulassen.
- Wenn Sie einen GCP-Linux-Server ermitteln und einen Root-Benutzer verwenden, verwenden Sie die folgenden Befehle, um die Standardeinstellung für die Root-Anmeldung zu ändern.
    1. Melden Sie sich beim Linux-Server an, und öffnen Sie die SSH-Konfigurationsdatei mit dem Befehl „vi /etc/ssh/sshd_config“.
    2. Setzen Sie die Option „PermitRootLogin“ auf „Ja“.
    3. Starten Sie den SSH-Dienst durch Ausführen von „service sshd restart“ neu.

## <a name="error-no-suitable-authentication-method-found"></a>Error: Es wurde keine passende Authentifizierungsmethode gefunden.

Stellen Sie mithilfe der folgenden Schritte sicher, dass die kennwortbasierte Authentifizierung auf dem Linux-Server aktiviert ist:
    1. Melden Sie sich beim Linux-Server an, und öffnen Sie die SSH-Konfigurationsdatei mit dem Befehl „vi /etc/ssh/sshd_config“.
    2. Setzen Sie die Option „PasswordAuthentication“ auf „Ja“. Speichern Sie die Datei .
    3. Starten Sie den SSH-Dienst durch Ausführen von „service sshd restart“ neu.

## <a name="discovered-servers-not-in-portal"></a>Ermittelte Server nicht im Portal

Wenn der Ermittlungszustand „Ermittlung wird ausgeführt“ lautet, die Server im Portal aber noch nicht angezeigt werden, warten Sie einige Minuten:

- Es dauert etwa 15 Minuten für einen VMware-Server.
- Es dauert etwa zwei Minuten für jeden hinzugefügten Host für die Hyper-V-Ermittlung.

Wenn Sie warten und der Zustand sich nicht ändert, wählen Sie **Aktualisieren** auf der Registerkarte **Server** aus. Dadurch sollte in „Azure Migrate: Ermittlung und Bewertung“ und „Azure Migrate: Servermigration“ die Anzahl der ermittelten Server angezeigt werden.

Wenn dies nicht funktioniert und Sie VMware-Server erkennen:

- Stellen Sie sicher, dass das von Ihnen angegebene vCenter-Konto über ordnungsgemäß festgelegte Berechtigungen mit Zugriff auf mindestens einen Server verfügt.
- Azure Migrate kann keine VMware-Server ermitteln, wenn dem vCenter-Konto auf Ebene des vCenter-VM-Ordners Zugriff gewährt wird. [Erfahren Sie mehr](set-discovery-scope.md) über das Beschränken der Ermittlung.

## <a name="server-data-not-in-portal"></a>Serverdaten nicht im Portal

Wenn ermittelte Server nicht im Portal angezeigt werden oder die Serverdaten veraltet sind, warten Sie einige Minuten. Es dauert bis zu 30 Minuten, bis Änderungen an den ermittelten Serverkonfigurationsdaten im Portal angezeigt werden. Es kann einige Stunden dauern, bis Änderungen an Softwareinventardaten angezeigt werden. Sollten nach dieser Zeit immer noch keine Daten vorhanden sein, versuchen Sie, die Daten wie folgt zu aktualisieren:

1. Wählen Sie unter **Windows, Linux und SQL Server** > **Azure Migrate: Ermittlung und Bewertung** die Option **Übersicht** aus.
2. Wählen Sie unter **Verwalten** die Option **Agent-Integritätsdiagnose** aus.
3. Wählen Sie **Agent aktualisieren** aus.
4. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen sehen.

## <a name="deleted-servers-appear-in-portal"></a>Gelöschte Server werden im Portal angezeigt

Wenn Sie Server löschen und sie weiterhin im Portal angezeigt werden, warten Sie 30 Minuten. Wenn sie weiterhin angezeigt werden, aktualisieren Sie sie wie oben beschrieben.

## <a name="discovered-software-inventory-and-sql-server-instances-and-databases-not-in-portal"></a>Ermitteltes Softwareinventar sowie ermittelte SQL Server-Instanzen und -Datenbanken nicht im Portal

Nach dem Initiieren der Ermittlung auf der Appliance kann es bis zu 24 Stunden dauern, bis die Inventurdaten im Portal angezeigt werden.

Wenn Sie keine Anmeldeinformationen für die Windows- oder SQL Server-Authentifizierung für den Appliance-Konfigurations-Manager angegeben haben, fügen Sie die Anmeldeinformationen hinzu, damit sie von der Appliance zum Herstellen einer Verbindung mit den entsprechenden SQL Server-Instanzen verwendet werden können.

Sobald die Verbindung hergestellt wurde, sammelt die Appliance Konfigurations- und Leistungsdaten von SQL Server-Instanzen und -Datenbanken. Die SQL Server-Konfigurationsdaten werden alle 24 Stunden aktualisiert, und die Leistungsdaten werden alle 30 Sekunden aufgezeichnet. Daher kann es bis zu 24 Stunden dauern, bis Änderungen an den Eigenschaften der SQL Server-Instanz und -Datenbanken im Portal angezeigt werden, zum Beispiel der Datenbankstatus, der Kompatibilitätsgrad und weitere Eigenschaften.

## <a name="sql-server-instance-is-showing-up-in-not-connected-state-on-portal"></a>SQL Server-Instanz mit Zustand „Nicht verbunden“ im Portal

Klicken Sie zum Anzeigen der Probleme, die bei der Ermittlung von SQL Server-Instanzen und -Datenbanken aufgetreten sind, auf der Seite „Ermittelte Server“ Ihres Projekts in der Spalte „Verbindungsstatus“ auf den Status „Nicht verbunden“.

Die Erstellung einer Bewertung auf Servern mit SQL-Instanzen, die nicht vollständig erkannt wurden oder nicht verbunden sind, kann dazu führen, dass für die Bereitschaft „Unbekannt“ angegeben wird.

## <a name="i-do-not-see-performance-data-for-some-network-adapters-on-my-physical-servers"></a>Ich sehe keine Leistungsdaten für einige Netzwerkadapter auf meinen physischen Servern.

Dies kann vorkommen, wenn die Hyper-V-Virtualisierung auf dem physischen Server aktiviert ist. Aufgrund einer Produktlücke wird der Netzwerkdurchsatz auf den erkannten virtuellen Netzwerkadaptern aufgezeichnet.

## <a name="error-the-file-uploaded-is-not-in-the-expected-format"></a>Error: Die hochgeladene Datei weist nicht das erwartete Format auf

In einigen Tools sind regionale Einstellungen festgelegt, durch die die CSV-Datei mit Semikolon als Trennzeichen erstellt wird. Ändern Sie die Einstellungen so, dass das Trennzeichen ein Komma ist.

## <a name="i-imported-a-csv-but-i-see-discovery-is-in-progress"></a>Ich habe eine CSV-Datei importiert, sehe aber, dass die Ermittlung ausgeführt wird

Dieser Status wird angezeigt, wenn das Hochladen Ihrer CSV-Datei aufgrund eines Überprüfungsfehlers fehlgeschlagen ist. Versuchen Sie, die CSV-Datei erneut zu importieren. Sie können den Fehlerbericht des vorherigen Hochladevorgangs herunterladen und der Anleitung zur Fehlerbehebung in der Datei folgen, um die Fehler zu beheben. Der Fehlerbericht kann im Abschnitt „Importdetails“ auf der Seite „Server ermitteln“ heruntergeladen werden.

## <a name="do-not-see-software-inventory-details-even-after-updating-guest-credentials"></a>Es werden auch nach dem Aktualisieren der Gastanmeldeinformationen keine Softwareinventardetails angezeigt

Die Softwareinventarermittlung wird einmal alle 24 Stunden ausgeführt. Wenn die Details sofort angezeigt werden sollen, führen Sie die folgenden Schritte zur Aktualisierung aus. Je nach Anzahl der ermittelten VMs von ermittelten Servern.

1. Wählen Sie unter **Windows, Linux und SQL Server** > **Azure Migrate: Ermittlung und Bewertung** die Option **Übersicht** aus.
2. Wählen Sie unter **Verwalten** die Option **Agent-Integritätsdiagnose** aus.
3. Wählen Sie **Agent aktualisieren** aus.
4. Warten Sie, bis der Aktualisierungsvorgang abgeschlossen wurde. Sie sollten nun aktuelle Informationen sehen.

## <a name="unable-to-export-software-inventory"></a>Export des Softwareinventars nicht möglich

Stellen Sie sicher, dass der Benutzer, der den Bestand aus dem Portal herunterlädt, über die Berechtigung „Mitwirkender“ für das Abonnement verfügt.

## <a name="no-suitable-authentication-method-found-to-complete-authentication-publickey"></a>Keine geeignete Authentifizierungsmethode gefunden, um die Authentifizierung abzuschließen (publickey)

Die schlüsselbasierte Authentifizierung wird nicht funktionieren, verwenden Sie die Kennwortauthentifizierung.

## <a name="common-app-discovery-errors"></a>Häufige Fehler bei der App-Ermittlung

Azure Migrate unterstützt die Ermittlung des Softwareinventars mit der Azure Migrate-Ermittlung und -Bewertung. Die App-Ermittlung wird derzeit nur für VMware unterstützt. [Erfahren Sie mehr](how-to-discover-applications.md) über die Anforderungen und Schritte bei der Einrichtung der App-Ermittlung.

Typische Fehler bei der App-Ermittlung sind in der Tabelle zusammengefasst.

| **Fehler** | **Ursache** | **Aktion** |
|--|--|--|
| 9000: Der Status der VMware-Tools kann nicht ermittelt werden. | Die VMware-Tools sind möglicherweise nicht installiert oder beschädigt. | Stellen Sie sicher, dass die VMware-Tools auf dem Server installiert sind und ausgeführt werden. |
| 9001: Die VMware-Tools sind nicht installiert. | Die VMware-Tools sind möglicherweise nicht installiert oder beschädigt. | Stellen Sie sicher, dass die VMware-Tools auf dem Server installiert sind und ausgeführt werden. |
| 9002: Die VMware-Tools werden nicht ausgeführt. | Die VMware-Tools sind möglicherweise nicht installiert oder beschädigt. | Stellen Sie sicher, dass die VMware-Tools auf dem Server installiert sind und ausgeführt werden. |
| 9003: Der Betriebssystemtyp wird für die Ermittlung von Gastservern nicht unterstützt. | Das auf dem Server ausgeführte Betriebssystem ist weder Windows noch Linux. | Als Betriebssystemtypen werden nur Windows und Linux unterstützt. Wenn der Server tatsächlich Windows oder Linux verwendet, überprüfen Sie den in vCenter Server angegebenen Betriebssystemtyp. |
| 9004: Der Server wird nicht ausgeführt. | Der Server wurde ausgeschaltet. | Stellen Sie sicher, dass der Server eingeschaltet ist. |
| 9005: Der Betriebssystemtyp wird für die Ermittlung von Gastservern nicht unterstützt. | Der Betriebssystemtyp wird für die Ermittlung von Gastservern nicht unterstützt. | Als Betriebssystemtypen werden nur Windows und Linux unterstützt. |
| 9006: Die URL zum Herunterladen der Metadatendatei vom Gast ist leer. | Dies kann vorkommen, wenn der Ermittlungs-Agent nicht erwartungsgemäß funktioniert. | Das Problem sollte sich innerhalb von 24 Stunden automatisch lösen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| 9007: Der Prozess zur Ausführung der Ermittlungsaufgabe auf dem Gastserver wurde nicht gefunden. | Dies kann vorkommen, wenn der Ermittlungs-Agent nicht ordnungsgemäß funktioniert. | Das Problem sollte sich innerhalb von 24 Stunden automatisch lösen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| 9008: Der Prozessstatus des Gastservers kann nicht abgerufen werden. | Das Problem kann aufgrund eines internen Fehlers auftreten. | Das Problem sollte sich innerhalb von 24 Stunden automatisch lösen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| 9009: Die Ausführung der Ermittlungsaufgabe auf dem Server wurde durch die Windows-Benutzerkontensteuerung (UAC) verhindert. | Die Einstellungen für die Windows-Benutzerkontensteuerung (UAC) auf dem Server sind sehr restriktiv und verhindern die Ermittlung des installierten Softwareinventars. | Konfigurieren Sie die Einstellungen für die Benutzerkontensteuerung auf dem Server mit einer der beiden niedrigeren Ebenen. |
| 9010: Der Server wurde ausgeschaltet. | Der Server wurde ausgeschaltet. | Stellen Sie sicher, dass der Server eingeschaltet ist. |
| 9011: Die Datei mit ermittelten Metadaten wurde im Gastserver-Dateisystem nicht gefunden. | Das Problem kann aufgrund eines internen Fehlers auftreten. | Das Problem sollte sich innerhalb von 24 Stunden automatisch lösen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| 9012: Die ermittelte Metadatendatei ist leer. | Das Problem kann aufgrund eines internen Fehlers auftreten. | Das Problem sollte sich innerhalb von 24 Stunden automatisch lösen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| 9013: Für jede Anmeldung wird ein neues temporäres Profil erstellt. | Für jede Anmeldung beim VMware-Server wird ein neues temporäres Profil erstellt. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 9014: Die Metadaten können nicht aus dem Gastserver-Dateisystem abgerufen werden. | Keine Verbindung mit dem ESXi-Host. | Stellen Sie sicher, dass die Appliance eine Verbindung mit Port 443 auf dem ESXi-Host herstellen kann, auf dem der Server ausgeführt wird. |
| 9015: Die Rolle „Gastvorgänge“ ist für das vCenter-Benutzerkonto nicht aktiviert. | Die Rolle „Gastvorgänge“ ist für das vCenter-Benutzerkonto nicht aktiviert. | Stellen Sie sicher, dass die Rolle „Gastvorgänge“ für das vCenter-Benutzerkonto aktiviert ist. |
| 9016: Es kann keine Ermittlung durchgeführt werden, weil der Agent für Gastvorgänge veraltet ist. | Die VMware-Tools sind nicht ordnungsgemäß installiert oder nicht auf dem neuesten Stand. | Stellen Sie sicher, dass die VMware-Tools ordnungsgemäß installiert und auf dem neuesten Stand sind. |
| 9017: Die Datei mit den ermittelten Metadaten wurde auf dem Server nicht gefunden. | Das Problem kann aufgrund eines internen Fehlers auftreten. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 9018: PowerShell ist auf den Gastservern nicht installiert. | PowerShell ist auf dem Gastserver nicht verfügbar. | Installieren Sie PowerShell auf dem Gastserver. |
| 9019: Ermittlung aufgrund von Fehlern beim Gastservervorgang nicht möglich. | Es ist ein Fehler beim VMware-Gastvorgang auf dem Server aufgetreten. | Stellen Sie sicher, dass die Serveranmeldeinformationen gültig sind und der Benutzername in den Anmeldeinformationen für den Gastserver das UPN-Format hat. |
| 9020: Die Berechtigung zum Erstellen von Dateien wurde verweigert. | Die dem Benutzer oder der Gruppenrichtlinie zugeordnete Rolle hindert den Benutzer daran, die Datei im Ordner zu erstellen. | Überprüfen Sie, ob der angegebene Gastbenutzer über die Berechtigung „Erstellen“ für die Datei im Ordner verfügt. Den Namen des Ordners finden Sie unter **Benachrichtigungen** in „Azure Migrate: Ermittlung und Bewertung“. |
| 9021: Es kann keine Datei im temporären Systempfad erstellt werden. | Das VMware-Tool meldet den temporären Pfad des Systems anstelle des temporären Pfads des Benutzers. | Aktualisieren Sie Ihre VMware-Tools auf eine höhere Version als 10287 (NGC/VI-Clientformat). |
| 9022: Der Zugriff auf das WMI-Objekt wird verweigert. | Die dem Benutzer zugeordnete Rolle oder die Gruppenrichtlinie schränkt den Zugriff des Benutzers auf das WMI-Objekt ein. | Kontaktieren Sie den Microsoft-Support. |
| 9023: PowerShell kann nicht ausgeführt werden, weil der Wert der SystemRoot-Umgebungsvariablen leer ist. | Der Wert der SystemRoot-Umgebungsvariablen für den Gastserver ist leer. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 9024: Es kann keine Ermittlung durchgeführt werden, weil der Wert der TEMP-Umgebungsvariablen leer ist. | Der Wert der TEMP-Umgebungsvariablen für den Gastserver ist leer. | Kontaktieren Sie den Microsoft-Support. |
| 9025: PowerShell ist auf den Gastservern beschädigt. | PowerShell ist auf dem Gastserver beschädigt. | Installieren Sie PowerShell auf dem Gastserver neu, und überprüfen Sie, ob PowerShell auf dem Gastserver ausgeführt werden kann. |
| 9026: Auf dem Server können keine Gastvorgänge ausgeführt werden. | Der Serverzustand lässt nicht zu, dass Gastvorgänge auf dem Server ausgeführt werden. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 9027: Der Agent für Gastvorgänge wird nicht auf dem Server ausgeführt. | Fehler beim Kontaktieren des Agents für Gastvorgänge, der auf dem virtuellen Server ausgeführt wird. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 9028: Die Datei kann aufgrund von unzureichendem Speicherplatz auf dem Server nicht erstellt werden. | Nicht genügend Speicherplatz auf dem Datenträger. | Stellen Sie sicher, dass genügend Speicherplatz im Datenträgerspeicher des Servers zur Verfügung steht. |
| 9029: Kein Zugriff auf PowerShell mit den angegebenen Anmeldeinformationen für den Gastserver. | Der Benutzer kann nicht auf PowerShell zugreifen. | Stellen Sie sicher, dass der in der Appliance hinzugefügte Benutzer auf PowerShell auf dem Gastserver zugreifen kann. |
| 9030: Die ermittelten Metadaten konnten nicht abgerufen werden, weil der ESXi-Host getrennt ist. | Der ESXi-Host befindet sich in einem nicht verbundenen Zustand. | Stellen Sie sicher, dass der ESXi-Host, der den Server ausführt, verbunden ist. |
| 9031: Die ermittelten Metadaten konnten nicht abgerufen werden, weil der ESXi-Host nicht antwortet. | Der Remotehost befindet sich in einem ungültigen Zustand. | Stellen Sie sicher, dass der ESXi-Host, der den Server ausführt, ausgeführt wird und verbunden ist. |
| 9032: Aufgrund eines internen Fehlers kann keine Ermittlung durchgeführt werden. | Das Problem kann aufgrund eines internen Fehlers auftreten. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 9033: Es kann keine Ermittlung durchgeführt werden, weil der Serverbenutzername ungültige Zeichen enthält. | Im Benutzernamen wurden ungültige Zeichen gefunden. | Geben Sie die Serveranmeldeinformationen erneut an, und stellen Sie hierbei sicher, dass keine ungültigen Zeichen vorhanden sind. |
| 9034: Der angegebene Benutzername weist nicht das UPN-Format auf. | Der Benutzername weist nicht das UPN-Format auf. | Stellen Sie sicher, dass der Benutzername im UPN-Format (User Pincipal Name) vorliegt. |
| 9035: Es kann keine Ermittlung durchgeführt werden, weil der PowerShell-Sprachmodus nicht auf „FullLanguage“ festgelegt ist. | Der Sprachmodus für PowerShell auf dem Gastserver ist nicht auf „FullLanguage“ festgelegt. | Stellen Sie sicher, dass der PowerShell-Sprachmodus auf „FullLanguage“ festgelegt ist. |
| 9037: Die Datensammlung wurde vorübergehend angehalten, weil die Serverantwortzeit zu hoch ist. | Der ermittelte Server braucht zu lange, um zu antworten. | Keine weiteren Maßnahmen erforderlich. Ein Wiederholungsversuch wird in 24 Stunden für die Softwareinventarermittlung und in 3 Stunden für die Abhängigkeitsanalyse (ohne Agent) ausgeführt. |
| 10000: Der Betriebssystemtyp wird nicht unterstützt. | Das auf dem Server ausgeführte Betriebssystem ist weder Windows noch Linux. | Als Betriebssystemtypen werden nur Windows und Linux unterstützt. |
| 10001: Das Skript für die Serverermittlung wurde nicht in der Appliance gefunden. | Die Ermittlung funktioniert nicht wie erwartet. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 10002: Die Ermittlungsaufgabe wurde nicht rechtzeitig abgeschlossen. | Der Ermittlungs-Agent funktioniert nicht wie erwartet. | Das Problem sollte sich innerhalb von 24 Stunden automatisch lösen. Wenden Sie sich an den Microsoft Support, wenn das Problem weiterhin besteht. |
| 10003: Der Prozess zur Ausführung der Ermittlungsaufgabe wurde mit einem Fehler beendet. | Der Prozess zur Ausführung der Ermittlungsaufgabe wurde mit einem Fehler beendet. | Das Problem sollte sich innerhalb von 24 Stunden automatisch lösen. Wenden Sie sich an den Microsoft-Support, wenn das Problem weiterhin besteht. |
| 10004: Für den Gastbetriebssystemtyp wurden keine Anmeldeinformationen angegeben. | In der Azure Migrate-Appliance wurden keine Anmeldeinformationen für den Zugriff auf Server mit diesem Betriebssystemtyp angegeben. | Fügen Sie in der Appliance Anmeldeinformationen für Server hinzu. |
| 10005: Die angegebenen Anmeldeinformationen sind nicht zulässig. | Die in der Appliance für den Serverzugriff angegebenen Anmeldeinformationen sind falsch. | Aktualisieren Sie die in der Appliance angegebenen Anmeldeinformationen, und stellen Sie sicher, dass der Zugriff auf den Server mit den Anmeldeinformationen möglich ist. |
| 10006: Der Gastbetriebssystemtyp wird vom Anmeldeinformationsspeicher nicht unterstützt. | Das auf dem Server ausgeführte Betriebssystem ist weder Windows noch Linux. | Als Betriebssystemtypen werden nur Windows und Linux unterstützt. |
| 10007: Die ermittelten Metadaten können nicht verarbeitet werden. | Fehler beim Versuch, die JSON zu deserialisieren. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 10008: Es kann keine Datei auf dem Server erstellt werden. | Das Problem kann aufgrund eines internen Fehlers auftreten. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |
| 10009: Die ermittelten Metadaten können nicht in eine Datei auf dem Server geschrieben werden. | Das Problem kann aufgrund eines internen Fehlers auftreten. | Wenden Sie sich an den Microsoft-Support, um eine Lösung zu erhalten. |

## <a name="common-sql-server-instances-and-database-discovery-errors"></a>Häufige Fehler bei der Ermittlung von SQL Server-Instanzen und -Datenbanken

Azure Migrate unterstützt die Ermittlung von SQL Server-Instanzen und -Datenbanken, die auf lokalen Computern ausgeführt werden, mit Azure Migrate: Ermittlung und Bewertung. Die SQL-Ermittlung wird derzeit nur für VMware unterstützt. Weitere Informationen zum Einstieg finden Sie im [Tutorial zur Ermittlung](tutorial-discover-vmware.md).

Typische Fehler bei der SQL-Ermittlung sind in der Tabelle zusammengefasst.

| **Fehler** | **Ursache** | **Aktion** |
|--|--|--|
|30000: Die dieser SQL Server-Instanz zugeordneten Anmeldeinformationen haben nicht funktioniert.|Entweder sind manuell zugeordnete Anmeldeinformationen ungültig, oder automatisch zugeordnete Anmeldeinformationen können nicht mehr auf die SQL Server-Instanz zugreifen.|Fügen Sie Anmeldeinformationen für SQL Server auf der Appliance hinzu, und warten Sie bis zum nächsten SQL-Ermittlungszyklus, oder erzwingen Sie eine Aktualisierung.|
|30001: Von der Appliance aus kann keine Verbindung mit der SQL Server-Instanz hergestellt werden.|1. Die Appliance hat keine Netzwerk-Sichtverbindung zum SQL Server.<br/>2. Die Firewall blockiert die Verbindung zwischen SQL Server und der Appliance.|1. Machen Sie SQL Server für die Appliance erreichbar.<br/>2. Lassen Sie eingehende Verbindungen von der Appliance zum SQL Server zu.|
|30003: Das Zertifikat ist nicht vertrauenswürdig.|Auf dem Computer, auf dem SQL Server ausgeführt wird, ist kein vertrauenswürdiges Zertifikat installiert.|Richten Sie ein vertrauenswürdiges Zertifikat auf dem Server ein. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153616)|
|30004: Unzureichende Berechtigungen.|Dieser Fehler kann dadurch verursacht werden, dass keine ausreichenden Berechtigungen zum Überprüfen von SQL Server-Instanzen vorhanden sind. |Weisen Sie den für die Appliance angegebenen Anmeldeinformationen bzw. dem angegebenen Konto die Rolle „sysadmin“ zu, um SQL Server-Instanzen und -Datenbanken zu ermitteln. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153511)|
|30005: SQL Server-Anmeldung konnte aufgrund eines Problems mit der Standard-Masterdatenbank keine Verbindung herstellen.|Entweder ist die Datenbank ungültig, oder der Anmeldename besitzt keine CONNECT-Berechtigung für die Datenbank.|Verwenden Sie ALTER LOGIN, um die Standarddatenbank auf die Masterdatenbank festzulegen.<br/>Weisen Sie den für die Appliance angegebenen Anmeldeinformationen bzw. dem angegebenen Konto die Rolle „sysadmin“ zu, um SQL Server-Instanzen und -Datenbanken zu ermitteln. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153615)|
|30006: SQL Server-Anmeldung kann nicht mit der Windows-Authentifizierung verwendet werden.|1. Die Anmeldung ist möglicherweise eine SQL Server-Anmeldung, aber der Server akzeptiert nur Windows-Authentifizierung.<br/>2. Sie versuchen, eine Verbindung mit SQL Server-Authentifizierung herzustellen, aber die Anmeldeinformationen sind in SQL Server nicht vorhanden.<br/>3. Die Anmeldung verwendet möglicherweise die Windows-Authentifizierung, aber die Anmeldung ist ein nicht erkannter Windows-Prinzipal. Ein nicht erkannter Windows-Prinzipal bedeutet, dass die Anmeldung nicht von Windows überprüft werden kann. Das konnte daran liegen, dass die Windows-Anmeldung von einer nicht vertrauenswürdigen Domäne stammt.|Wenn Sie versuchen, eine Verbindung mithilfe der SQL Server-Authentifizierung herzustellen, vergewissern Sie sich, dass SQL Server im gemischten Authentifizierungsmodus konfiguriert ist und die SQL Server-Anmeldung vorhanden ist.<br/>Wenn Sie versuchen, eine Verbindung mit Windows-Authentifizierung herzustellen, überprüfen Sie, dass Sie ordnungsgemäß bei der richtigen Domäne angemeldet sind. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153421)|
|30007: Kennwort abgelaufen.|Das Kennwort für das Konto ist abgelaufen.|Das Kennwort für die SQL Server-Anmeldung ist möglicherweise abgelaufen. Setzen Sie das Kennwort zurück, oder verlängern Sie das Datum für das Kennwort. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153419)|
|30008: Das Kennwort muss geändert werden.|Das Kennwort für das Konto muss geändert werden.|Ändern Sie das Kennwort der für die SQL Server-Ermittlung angegebenen Anmeldeinformation. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153318)|
|30009: Ein interner Fehler ist aufgetreten.|Interner Fehler beim Ermitteln von SQL Server-Instanzen und -Datenbanken. |Falls das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.|
|30010: Es wurden keine Datenbanken gefunden.|Auf der ausgewählten Serverinstanz wurden keine Datenbanken gefunden.|Weisen Sie den für die Appliance angegebenen Anmeldeinformationen bzw. dem angegebenen Konto die Rolle „sysadmin“ zu, um SQL-Datenbanken zu ermitteln.|
|30011: Interner Fehler beim Bewerten einer SQL-Instanz oder -Datenbank.|Interner Fehler beim Ausführen der Bewertung.|Falls das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.|
|30012: SQL-Verbindungsfehler.|1. Die Firewall auf dem Server hat die Verbindung abgelehnt.<br/>2. Der SQL Server-Browserdienst (sqlbrowser) wurde nicht gestartet.<br/>3. Keine Antwort von SQL Server auf die Clientanforderung, da der Server wahrscheinlich nicht gestartet wurde.<br/>4. Der SQL Server-Client kann keine Verbindung mit dem Server herstellen. Dieser Fehler ist möglicherweise darauf zurückzuführen, dass der Server nicht für die Annahme von Remoteverbindungen konfiguriert ist.<br/>5. Der SQL Server-Client kann keine Verbindung mit dem Server herstellen. Der Fehler ist möglicherweise darauf zurückzuführen, dass entweder der Name des Servers durch den Client nicht aufgelöst werden kann oder der Name des Servers falsch ist.<br/>6. Die TCP- oder Named Pipe-Protokolle sind nicht aktiviert.<br/>7. Der angegebene Name der SQL Server-Instanz ist ungültig.|Verwenden Sie [diesen](https://go.microsoft.com/fwlink/?linkid=2153317) interaktiven Benutzerleitfaden für die Behandlung des Verbindungsproblems. Nachdem Sie die Schritte im Leitfaden befolgt haben, warten Sie 24 Stunden, bis die Daten im Dienst aktualisiert wurden. Wenden Sie sich an den Microsoft-Support, wenn das Problem weiterhin besteht.|
|30013: Fehler beim Herstellen einer Verbindung mit der SQL Server-Instanz.|1. Der SQL Server-Name kann von der Appliance nicht aufgelöst werden.<br/>2. SQL Server lässt keine Remoteverbindungen zu.|Wenn Sie SQL Server von der Appliance aus anpingen können, warten Sie 24 Stunden, und prüfen Sie dann, ob das Problem automatisch gelöst ist. Wenn nicht, wenden Sie sich an den Microsoft-Support. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153316)|
|30014: Benutzername oder Kennwort ist falsch.| Dieser Fehler kann auf einen Authentifizierungsfehler aufgrund eines ungültigen Kennworts oder Benutzernamens zurückzuführen sein.|Geben Sie Anmeldeinformationen mit gültigem Benutzernamen und Kennwort an. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153315)|
|30015: Interner Fehler beim Ermitteln der SQL-Instanz.|Beim Ermitteln der SQL-Instanz ist ein interner Fehler aufgetreten.|Falls das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.|
|30016: Aufgrund eines Timeouts konnte keine Verbindung mit der Instanz „%instance;“ hergestellt werden.| Dieses Problem kann auftreten, wenn die Firewall auf dem Server die Verbindung ablehnt.|Überprüfen Sie, ob die Firewall der SQL Server-Instanz für die Annahme von Verbindungen konfiguriert ist. Wenden Sie sich an den Microsoft-Support, wenn der Fehler weiterhin besteht. [Weitere Informationen](https://go.microsoft.com/fwlink/?linkid=2153611)|
|30017: Interner Fehler.|Ausnahmefehler.|Falls das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support.|
|30018: Interner Fehler.|Interner Fehler beim Erfassen von Daten wie z. B. der Größe der temporären Datenbank, der Dateigröße der SQL-Instanz usw.|Warten Sie 24 Stunden, und wenden Sie sich an den Microsoft-Support, falls das Problem weiterhin besteht.|
|30019: Ein interner Fehler ist aufgetreten.|Interner Fehler beim Erfassen von Leistungsmetriken wie z. B. der Arbeitsspeicherauslastung einer Datenbank oder Instanz.|Warten Sie 24 Stunden, und wenden Sie sich an den Microsoft-Support, falls das Problem weiterhin besteht.|

## <a name="next-steps"></a>Nächste Schritte

Richten Sie eine Appliance für [VMware](how-to-set-up-appliance-vmware.md), [Hyper-V](how-to-set-up-appliance-hyper-v.md) oder [physische Server](how-to-set-up-appliance-physical.md) ein.
