---
title: Einrichten einer Azure Migrate-Appliance für VMware
description: In diesem Artikel erfahren Sie, wie Sie eine Azure Migrate-Appliance einrichten, um Server in VMware-Umgebungen zu bewerten und zu migrieren.
author: vikram1988
ms.author: vibansa
ms.manager: abhemraj
ms.topic: how-to
ms.date: 04/16/2020
ms.openlocfilehash: 64be28838abb5d5021f0a8cefc0eed2c2516498b
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104865229"
---
# <a name="set-up-an-appliance-for-servers-in-vmware-environment"></a>Einrichten einer Appliance für Server in einer VMware-Umgebung

In diesem Artikel erfahren Sie, wie Sie die Azure Migrate-Appliance für die Bewertung mit dem Tool [Azure Migrate: Ermittlung und Bewertung](migrate-services-overview.md#azure-migrate-discovery-and-assessment-tool) und für die Migration ohne Agents mit dem Tool für die [Azure Migrate-Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool) einrichten.

Die [Azure Migrate-Appliance](migrate-appliance.md) ist eine einfache Appliance, die von den Tools „Azure Migrate: Ermittlung und Bewertung“ und „Servermigration“ verwendet wird, um Server zu ermitteln, die auf vCenter Server ausgeführt werden, um die Serverkonfiguration und Leistungsmetadaten an Azure zu senden und Server mithilfe der Migration ohne Agent zu replizieren.

Sie können die Appliance mithilfe verschiedener Methoden bereitstellen:

- Erstellen Sie einen Server auf vCenter Server mithilfe einer heruntergeladenen OVA-Vorlage. Diese Methode wird in diesem Artikel beschrieben.
- Richten Sie die Appliance mithilfe eines PowerShell-Installationsskripts auf einem vorhandenen Server ein. [Diese Methode](deploy-appliance-script.md) sollte verwendet werden, wenn Sie keine OVA-Vorlage verwenden können oder wenn Sie in Azure Government arbeiten.

Überprüfen Sie nach der Erstellung der Appliance, ob diese eine Verbindung mit dem Tool „Azure Migrate: Ermittlung und Bewertung“ herstellen kann. Registrieren Sie dieses anschließend bei dem Projekt, und konfigurieren Sie die Appliance, um die Ermittlung zu initiieren.

## <a name="deploy-with-ova"></a>Bereitstellen mit OVA

Gehen Sie zum Einrichten der Appliance mithilfe einer OVA-Vorlage wie folgt vor:
1. Geben Sie einen Appliancenamen ein, und generieren Sie einen Projektschlüssel im Portal.
1. Herunterladen einer OVA-Vorlagendatei und Importieren der Datei in vCenter Server Vergewissern Sie sich, dass die OVA sicher ist.
1. Erstellen Sie die Appliance-VM anhand der OVA-Datei, und prüfen Sie, ob sie sich mit Azure Migrate verbinden kann.
1. Führen Sie die Erstkonfiguration der Appliance aus, und registrieren Sie sie unter Verwendung des Projektschlüssels im Projekt.

### <a name="1-generate-the-project-key"></a>1. Generieren des Projektschlüssels

1. Wählen Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Ermittlung und Bewertung** die Option **Ermitteln** aus.
2. Wählen Sie unter **Discover servers (Server ermitteln)**  > **Sind Ihre Server virtualisiert?** die Option **Ja, mit VMware vSphere-Hypervisor** aus.
3. Geben Sie in **1: Projektschlüssel generieren** einen Namen für die Azure Migrate-Appliance an, die Sie für die Ermittlung von Servern in Ihrer VMware-Umgebung einrichten möchten. Der Name muss alphanumerisch sein und darf höchstens 14 Zeichen enthalten.
1. Klicken Sie auf **Schlüssel generieren**, um mit der Erstellung der erforderlichen Azure-Ressourcen zu beginnen. Schließen Sie die Seite „Computer ermitteln“ nicht, während die Ressourcen erstellt werden.
1. Nach der erfolgreichen Erstellung der Azure-Ressourcen wird ein **Projektschlüssel** generiert.
1. Kopieren Sie den Schlüssel, da Sie ihn benötigen, um die Registrierung der Appliance während der Konfiguration abzuschließen.

### <a name="2-download-the-ova-template"></a>2. Herunterladen der OVA-Vorlage

Wählen Sie in **2: Azure Migrate-Appliance herunterladen** die OVA-Datei aus, und klicken Sie auf **Herunterladen**.

### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die OVA-Datei sicher ist:

1. Öffnen Sie auf dem Server, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die OVA-Datei zu generieren:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Beispielverwendung: ```C:\>CertUtil -HashFile C:\Users\Administrator\Desktop\MicrosoftAzureMigration.ova SHA256```

3. Überprüfen Sie die aktuellen Applianceversionen und Hashwerte:

    - Öffentliche Azure-Cloud:
    
        **Algorithmus** | **Download** | **SHA256**
        --- | --- | ---
        VMware (11,9 GB) | [Aktuelle Version](https://go.microsoft.com/fwlink/?linkid=2140333) | e9c9a1fe4f3ebae81008328e8f3a7933d78ff835ecd871d1b17f367621ce3c74


### <a name="3-create-the-appliance-server"></a>3. Erstellen des Servers auf der Appliance

Importieren Sie die heruntergeladene Datei, und erstellen Sie in der VMware-Umgebung einen Server.

1. Klicken Sie in der vSphere-Clientkonsole auf **Datei** > **Deploy OVF Template** (OVF-Vorlage bereitstellen).
2. Geben Sie im Assistenten zum Bereitstellen der OVF-Vorlage unter **Quelle** den Speicherort der OVA-Datei an.
3. Geben Sie unter **Name** und **Standort** einen Anzeigenamen für den Server an. Wählen Sie das Bestandsobjekt aus, in dem der Server gehostet wird.
5. Geben Sie unter **Host/Cluster** den Host oder Cluster an, auf bzw. in dem der Server ausgeführt wird.
6. Geben Sie unter **Speicher** das Speicherziel des Servers an.
7. Geben Sie unter **Datenträgerformat** den Typ und die Größe des Datenträgers an.
8. Geben Sie unter **Netzwerkzuordnung** das Netzwerk an, mit dem der Server eine Verbindung herstellt. Das Netzwerk benötigt Internetkonnektivität, um Metadaten an Azure Migrate zu senden.
9. Überprüfen Sie die Einstellungen, und klicken Sie dann auf **Fertig stellen**.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Stellen Sie sicher, dass der Server auf der Appliance eine Verbindung mit Azure-URLs für [öffentliche](migrate-appliance.md#public-cloud-urls) Clouds und [Government](migrate-appliance.md#government-cloud-urls)-Clouds herstellen kann.


### <a name="4-configure-the-appliance"></a>4. Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch.

> [!NOTE]
> Wenn Sie die Appliance mithilfe eines [**PowerShell-Skripts**](deploy-appliance-script.md) und nicht mithilfe der heruntergeladenen OVA-Datei einrichten, sind die ersten beiden Schritte in diesem Verfahren nicht relevant.

1. Klicken Sie in der vSphere-Clientkonsole mit der rechten Maustaste auf den Server, und wählen Sie **Konsole öffnen** aus.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie einen Browser auf einem beliebigen Server, der eine Verbindung mit dem Applianceserver herstellen kann, und rufen Sie die URL des Konfigurations-Managers der Appliance auf: `https://appliance name or IP address: 44368`.

   Alternativ können Sie den Konfigurations-Manager auch auf dem Desktop des Servers auf der Appliance öffnen, indem Sie die Verknüpfung für den Konfigurations-Manager auswählen.
1. Akzeptieren Sie die **Lizenzbedingungen**, und lesen Sie die Drittanbieterinformationen.
1. Navigieren Sie im Konfigurations-Manager zu **Erforderliche Komponenten einrichten**, und führen Sie die folgenden Schritte aus:
   - **Konnektivität**: Die Appliance überprüft, ob der Server über Internetzugriff verfügt. Wenn der Server einen Proxy verwendet:
     - Klicken Sie auf **Proxy einrichten**, um die Proxyadresse im Format `http://ProxyIPAddress` oder `http://ProxyFQDN` und den überwachenden Port anzugeben.
     - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
     - Es werden nur HTTP-Proxys unterstützt.
     - Wenn Sie Proxydetails hinzugefügt oder den Proxy und/oder die Authentifizierung deaktiviert haben, klicken Sie auf **Speichern**, um die Konnektivitätsprüfung erneut auszulösen.
   - **Uhrzeitsynchronisierung**: Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die Ermittlung ordnungsgemäß funktioniert.
   - **Updates installieren**: Die Appliance stellt sicher, dass die neuesten Updates installiert sind. Nachdem die Überprüfung abgeschlossen ist, können Sie auf **View appliance services** (Appliancedienste anzeigen) klicken, um den Status und die Versionen der auf dem Applianceserver ausgeführten Dienste anzuzeigen.
   - **Install VDDK** (VDDK installieren): Die Appliance überprüft, ob das VMware vSphere-VDDK (Virtual Disk Development Kit) installiert ist. Laden Sie bei Bedarf VDDK 6.7 von VMware herunter, und extrahieren Sie den Inhalt der heruntergeladenen ZIP-Datei am angegebenen Ort auf der Appliance, wie unter **Installationsanweisungen** beschrieben.

     Bei der Servermigration mit Azure Migrate wird das VDDK zur Replikation von Servern während der Migration zu Azure verwendet. 
1. Wenn Sie möchten, können Sie während der Konfiguration der Appliance jederzeit **Voraussetzungen erneut ausführen**, um zu prüfen, ob die Appliance alle Voraussetzungen erfüllt.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-prerequisites.png" alt-text="Panel 1 im Konfigurations-Manager der Appliance":::


## <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Fügen Sie den aus dem Portal kopierten **Projektschlüssel** ein. Wenn Sie nicht über den Schlüssel verfügen, navigieren Sie zu **Ermittlungs- und Bewertungstool > Ermitteln > Vorhandene Appliances verwalten**, wählen Sie den Appliancenamen aus, den Sie beim Generieren des Schlüssels angegeben haben, und kopieren Sie den entsprechenden Schlüssel.
1. Für die Authentifizierung bei Azure benötigen Sie einen Gerätecode. Wenn Sie auf **Anmelden** klicken, wird ein modales Dialogfeld mit dem Gerätecode angezeigt. Dies ist in der folgenden Abbildung dargestellt.

    :::image type="content" source="./media/tutorial-discover-vmware/device-code.png" alt-text="Modales Dialogfeld mit Gerätecode":::

1. Klicken Sie auf **Copy code & Login** (Code kopieren und anmelden), um den Gerätecode zu kopieren und eine Azure-Anmeldeaufforderung in einer neuen Browserregisterkarte zu öffnen. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
1. Fügen Sie auf der neuen Registerkarte den Gerätecode ein, und melden Sie sich mit Ihrem Azure-Benutzernamen und dem zugehörigen Kennwort an.
   
   Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Falls Sie die Registerkarte für die Anmeldung versehentlich schließen, ohne die Anmeldung durchzuführen, müssen Sie die Browserregisterkarte des Appliance-Konfigurations-Managers aktualisieren, um die Schaltfläche „Anmelden“ wieder zu aktivieren.
1. Wechseln Sie nach der erfolgreichen Anmeldung wieder zur vorherigen Registerkarte mit dem Appliance-Konfigurations-Manager.
1. Wenn das für die Protokollierung verwendete Azure-Benutzerkonto über die richtigen Berechtigungen für die während der Schlüsselgenerierung erstellten Azure-Ressourcen verfügt, wird die Registrierung der Appliance initiiert.
1. Nachdem die Appliance erfolgreich registriert wurde, können Sie die Registrierungsdetails anzeigen, indem Sie auf **Details anzeigen** klicken.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-registration.png" alt-text="Panel 2 im Appliance-Konfigurations-Manager":::

## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

### <a name="provide-vcenter-server-details"></a>Angeben von vCenter Server-Details

Die Appliance muss eine Verbindung mit der vCenter Server-Instanz herstellen, um die Konfigurations- und Leistungsdaten der Server zu ermitteln.

1. Klicken Sie in **Schritt 1: Geben Sie die vCenter Server-Anmeldeinformationen ein** auf **Anmeldeinformationen hinzufügen**, um einen Anzeigenamen für die Anmeldeinformationen anzugeben. Fügen Sie **Benutzername** und **Kennwort** für das vCenter Server-Konto hinzu, das die Appliance verwendet, um Server in der vCenter Server-Instanz zu ermitteln.
    - Sie sollten ein Konto mit den erforderlichen Berechtigungen festgelegt haben, wie weiter oben in diesem Artikel beschrieben.
    - Wenn Sie die Ermittlung auf bestimmte VMware-Objekte (vCenter Server-Rechenzentren, Cluster, einen Ordner mit Clustern, Hosts, einen Ordner mit Hosts oder einzelne Server) begrenzen möchten, hilft Ihnen die Anleitung in [diesem Artikel](set-discovery-scope.md) beim Einschränken des von Azure Migrate genutzten Kontos weiter.
1. Klicken Sie in **Schritt 2: Geben Sie vCenter Server-Details ein** auf **Ermittlungsquelle hinzufügen**, um den Anzeigenamen für die Anmeldeinformationen in der Dropdownliste auszuwählen, und geben Sie den Wert für **IP-Adresse/FQDN** der vCenter Server-Instanz an. Sie können für **Port** den Standardwert (443) beibehalten oder einen benutzerdefinierten Port angeben, den vCenter Server überwacht, und dann auf **Speichern** klicken.
1. Wenn Sie auf **Speichern** klicken, versucht die Appliance, die Verbindung mit der vCenter Server-Instanz mit den angegebenen Anmeldeinformationen zu überprüfen, und zeigt den **Überprüfungszustand** in der Tabelle für die vCenter Server-IP-Adresse/FQDN an.
1. Sie können die Verbindung zu vCenter Server jederzeit vor Beginn der Ermittlung **erneut überprüfen**.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-manage-sources.png" alt-text="Panel 3 im Konfigurations-Manager auf der Appliance mit vCenter Server-Details":::

### <a name="provide-server-credentials"></a>Angeben von Serveranmeldeinformation

In **Schritt 3: Geben Sie Serveranmeldeinformationen an, um eine Software-Inventur, Abhängigkeitsanalyse ohne Agent und Ermittlung von SQL Server-Instanzen und -Datenbanken durchzuführen** können Sie entweder die Anmeldeinformationen für mehrere Server angeben oder, wenn Sie diese Features nicht nutzen möchten, den Schritt überspringen und mit der vCenter Server-Ermittlung fortfahren. Sie können Ihre Absicht später jederzeit ändern.

:::image type="content" source="./media/tutorial-discover-vmware/appliance-server-credentials-mapping.png" alt-text="Panel 3 im Konfigurations-Manager auf der Appliance mit Serverdetails":::


Wenn Sie diese Features nutzen möchten, können Sie Serveranmeldeinformationen mit den folgenden Schritten bereitstellen. Die Appliance versucht, die Anmeldeinformationen automatisch den Servern zuzuordnen, um die Ermittlungsaufgaben durchzuführen.

- Sie können Anmeldeinformationen für den Server hinzufügen, indem Sie auf die Schaltfläche **Anmeldeinformationen hinzufügen** klicken. Dadurch wird ein modales Fenster geöffnet, in dem Sie den **Anmeldeinformationstyp** im Dropdownmenü auswählen können.
- Sie können Anmeldeinformationen für die Authentifizierung für eine Domäne, Windows (ohne Domäne), Linux (ohne Domäne) oder SQL Server angeben. [Erfahren Sie mehr](add-server-credentials.md) zur Bereitstellung von Anmeldeinformationen und zu deren Handhabung.
- Für jeden Typ von Anmeldeinformationen müssen Sie einen Anzeigenamen für Anmeldeinformationen angeben, **Benutzername** und **Kennwort** hinzufügen und auf **Speichern** klicken.
- Wenn Sie Domänenanmeldeinformationen wählen, müssen Sie auch den FQDN für die Domäne angeben. Der FQDN wird benötigt, um die Echtheit der Anmeldeinformationen mithilfe des Active Directory der jeweiligen Domäne zu bestätigen.
- Überprüfen Sie die [erforderlichen Berechtigungen](add-server-credentials.md#required-permissions) des Kontos für die Ermittlung installierter Anwendungen, Abhängigkeitsanalyse ohne Agent oder für Ermittlung von SQL Server-Instanzen und -Datenbanken.
- Wenn Sie mehrere Anmeldeinformationen gleichzeitig hinzufügen möchten, klicken Sie auf **Weitere hinzufügen**, um die Angeben zu speichern und weitere Anmeldeinformationen hinzuzufügen.
- Wenn Sie auf **Speichern** oder **Weitere hinzufügen** klicken, überprüft die Appliance die Domänenanmeldeinformationen mit dem Active Directory der Domäne auf Echtheit. Dies geschieht, um Kontosperrungen zu vermeiden, wenn die Appliance mehrere Iterationen durchführt, um Anmeldeinformationen den jeweiligen Servern zuzuordnen.
- Sie können den **Überprüfungsstatus** aller Domänenanmeldeinformationen in der Tabelle mit den Anmeldeinformationen einsehen. Nur Domänenanmeldeinformationen werden überprüft.
- Wenn die Überprüfung fehlschlägt, können Sie auf den Status **Fehlgeschlagen** klicken, um den aufgetretenen Fehler einzusehen, und nach Behebung des Problems auf **Anmeldeinformationen erneut überprüfen** klicken, um die fehlgeschlagenen Domänenanmeldeinformationen erneut zu überprüfen.


### <a name="start-discovery"></a>Ermittlung starten

1. Klicken Sie auf **Ermittlung starten**, um die vCenter Server-Ermittlung zu starten. Nachdem die Ermittlung erfolgreich gestartet wurde, können Sie den Ermittlungsstatus anhand von IP-Adresse/FQDN von vCenter Server in der Tabelle mit den Quellen überprüfen.
1. Wenn Sie Serveranmeldeinformationen angegeben haben, wird die Software-Inventur (Ermittlung installierter Anwendungen) automatisch eingeleitet, nachdem die vCenter Server-Ermittlung abgeschlossen ist. Die Software-Inventur erfolgt einmal alle 12 Stunden.
1. Bei der [Software-Inventur](how-to-discover-applications.md) werden die auf den Servern laufenden SQL Server-Instanzen ermittelt. Anhand der Informationen versucht die Appliance, sich mit den auf der Appliance bereitgestellten Anmeldeinformationen für die Windows- oder SQL Server-Authentifizierung mit den Instanzen zu verbinden und Daten zu SQL Server-Datenbanken und deren Eigenschaften zu sammeln. Die SQL Server-Ermittlung erfolgt einmal alle 24 Stunden.
1. Bei der Software-Inventur werden die hinzugefügten Serveranmeldeinformationen mit Servern abgeglichen und für die Abhängigkeitsanalyse ohne Agent überprüft. Sie können die Abhängigkeitsanalyse ohne Agent für Server im Portal aktivieren. Nur die Server mit erfolgreicher Überprüfung können ausgewählt werden, um die Abhängigkeitsanalyse ohne Agent zu aktivieren.

Die Ermittlung funktioniert wie folgt:
- Es dauert etwa 15 Minuten, bis der Bestand ermittelter Server im Portal sichtbar ist.
- Die Ermittlung installierter Anwendungen kann einige Zeit in Anspruch nehmen. Die Dauer hängt von der Anzahl der ermittelten Server ab. Bei 500 Servern dauert es ungefähr ein Stunde, bis der ermittelte Bestand im Azure Migrate-Portal angezeigt wird.
- Nach der Ermittlung von Servern können Sie im Portal die Abhängigkeitsanalyse ohne Agent auf den Servern aktivieren.
- SQL Server-Instanzen und -Datenbankdaten werden im Portal innerhalb von 24 Stunden nach Einleiten der Ermittlung angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Tutorials zur [VMware-Bewertung](./tutorial-assess-vmware-azure-vm.md) und zur [Migration ohne Agent](tutorial-migrate-vmware.md).