---
title: Behandeln von Problemen bei der VM-Zertifizierung für Azure Marketplace
description: Dieser Artikel enthält Themen zur Behandlung häufiger Probleme, die beim Testen und Zertifizieren von VM-Images für Azure Marketplace auftreten.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 10/19/2020
ms.openlocfilehash: f065b1bc98eab86542ecff73e1471e4d90cd4182
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93339532"
---
# <a name="vm-certification-troubleshooting"></a>Behandeln von Problemen bei der VM-Zertifizierung

Beim Veröffentlichen Ihres VM-Images in Azure Marketplace überprüft das Azure-Team das VM-Image, um seine Startfähigkeit, Sicherheit und Kompatibilität mit Azure sicherzustellen. Wird einer der hochwertigen Tests nicht bestanden, tritt bei der Veröffentlichung ein Fehler auf, und Sie erhalten eine Fehlermeldung mit der Beschreibung des Problems.

In diesem Artikel werden Fehlermeldungen, die häufig bei der Veröffentlichung von VM-Images angezeigt werden, sowie die zugehörigen Lösungen erläutert.

> [!NOTE]
> Wenn Sie Fragen oder Verbesserungsvorschläge haben, wenden Sie sich bitte an den Partner Center-[Support](https://aka.ms/marketplacepublishersupport).

## <a name="approved-base-image"></a>Genehmigtes Basisimage

Wenn Sie eine Anforderung zur erneuten Veröffentlichung des Images mit Updates einreichen, schlägt der Testfall für die Überprüfung der Teilenummer möglicherweise fehl. In diesem Fall wird das Image nicht genehmigt.

Dieser Fehler tritt auf, wenn Sie ein Basisimage verwenden, das einem anderen Herausgeber gehört, und Sie das Image aktualisiert haben. In dieser Situation können Sie das Image nicht veröffentlichen.

Rufen Sie zum Beheben des Problems das Image aus Azure Marketplace ab, und nehmen Sie Änderungen daran vor. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Linux-Images](../virtual-machines/linux/endorsed-distros.md?toc=/azure/virtual-machines/linux/toc.json)
- [Windows-Images](azure-vm-create-using-approved-base.md)

> [!Note]
> Wenn Sie ein Linux-Basisimage verwenden, das nicht aus dem Azure Marketplace stammt, können Sie die erste Partition um 2048 KB versetzen. So kann der nicht formatierte Speicherplatz zum Hinzufügen neuer Abrechnungsinformationen verwendet werden, und Azure kann mit der Veröffentlichung Ihres virtuellen Computers im Azure Marketplace fortfahren.  

> [!Note]
> Wenn Sie ein Linux-Basisimage verwenden, das nicht aus dem Marketplace stammt, können Sie die erste Partition um 2.048 KB versetzen. So kann der nicht formatierte Speicherplatz zum Hinzufügen neuer Abrechnungsinformationen verwendet werden, und Azure kann mit der Veröffentlichung Ihres virtuellen Computers im Marketplace fortfahren.  

## <a name="vm-extension-failure"></a>Fehler bei einer VM-Erweiterung

Überprüfen Sie, ob Ihr Image VM-Erweiterungen unterstützt.

Gehen Sie zum Aktivieren von VM-Erweiterungen wie folgt vor:

1. Wählen Sie Ihren virtuellen Linux-Computer aus.
1. Navigieren Sie zu **Diagnoseeinstellungen**.
1. Aktivieren Sie Basismatrizen, indem Sie das **Speicherkonto** aktualisieren.
1. Wählen Sie **Speichern** aus.

   ![Überwachung auf Gastebene aktivieren](./media/create-vm/vm-certification-issues-solutions-1.png)

Überprüfen Sie wie folgt, ob die VM-Erweiterungen ordnungsgemäß aktiviert wurden:

1. Wählen Sie auf dem virtuellen Computer die Registerkarte **VM-Erweiterungen** aus, und überprüfen Sie den Status der **Linux-Diagnoseerweiterung**.
1. 
    * Ist der Status *Bereitstellung erfolgreich* , wurde der Testfall für Erweiterungen erfolgreich bestanden.  
    * Lautet der Status *Fehler beim Bereitstellen* , wurde der Testfall für Erweiterungen nicht bestanden, und Sie müssen das Festgeschrieben-Flag festlegen.

      ![Screenshot der erfolgreichen Bereitstellung](./media/create-vm/vm-certification-issues-solutions-2.png)

      Liegt ein Fehler bei der VM-Erweiterung vor, lesen Sie die Informationen unter [Verwenden der Linux-Diagnoseerweiterung zum Überwachen von Metriken und Protokollen](../virtual-machines/extensions/diagnostics-linux.md), um sie zu aktivieren. Wenn die VM-Erweiterung nicht aktiviert sein soll, wenden Sie sich an das Supportteam, und lassen Sie die Erweiterung deaktivieren.

## <a name="vm-provisioning-issue"></a>Problem bei der VM-Bereitstellung

Vergewissern Sie sich vor der Übermittlung Ihres Angebots, dass Sie den VM-Bereitstellungsprozess strikt befolgt haben. Unter dem Thema zum [Testen von VM-Images](azure-vm-image-test.md) finden Sie das JSON-Format zum Bereitstellen der VM.

Bereitstellungsprobleme können auf folgende Fehler zurückzuführen sein:

|Szenario|Fehler|`Reason`|Lösung|
|---|---|---|---|
|1|Ungültige virtuelle Festplatte (VHD)|Wenn der angegebene Cookie-Wert in der VHD-Fußzeile nicht korrekt ist, wird die VHD als ungültig eingestuft.|Erstellen Sie das Image neu, und senden Sie die Anforderung.|
|2|Ungültiger Blobtyp|Fehler bei der VM-Bereitstellung, weil der verwendete Block ein Blobtyp und kein Seitentyp ist.|Erstellen Sie das Image neu, und senden Sie die Anforderung.|
|3|Timeout bei der Bereitstellung oder nicht ordnungsgemäß generalisiert|Es liegt ein Problem mit der VM-Generalisierung vor.|Erstellen Sie das Image mit Generalisierung neu, und senden Sie die Anforderung.|
|

> [!NOTE]
> Weitere Informationen zur VM-Generalisierung finden Sie in den folgenden Ressourcen:
> - [Generalisieren des Images](azure-vm-create-using-approved-base.md#generalize-the-image)
> - [Generalisieren der Windows-VM mithilfe von Sysprep](../virtual-machines/windows/capture-image-resource.md#generalize-the-windows-vm-using-sysprep)


## <a name="vhd-specifications"></a>VHD-Spezifikationen

### <a name="conectix-cookie-and-other-vhd-specifications"></a>Conectix-Cookie und andere VHD-Spezifikationen
Die conectix-Zeichenfolge ist Teil der VHD-Spezifikation und wird in der unten angegebenen VHD-Fußzeile als 8-Byte-Cookie definiert, das den Ersteller der Datei identifiziert. Alle von Microsoft erstellten VHD-Dateien verfügen über dieses Cookie. 

Ein im VHD-Format formatiertes Blob sollte eine 512-Byte-Fußzeile aufweisen. Hier das Format der VHD-Fußzeile:

|Felder in Festplatten-Fußzeilen|Größe (Byte)|
|---|---|
Cookie|8
Features|4
Dateiformatversion|4
Datenoffset|8
Zeitstempel|4
Anwendung des Erstellers|4
Version des Erstellers|4
Hostbetriebssystem des Erstellers|4
Ursprüngliche Größe|8
Aktuelle Größe|8
Datenträgergeometrie|4
Datenträgertyp|4
Checksum|4
Eindeutige ID|16
Gespeicherter Zustand|1
Reserviert|427


### <a name="vhd-specifications"></a>VHD-Spezifikationen
Um eine reibungslose Veröffentlichung zu gewährleisten, stellen Sie sicher, dass die **VHD die folgenden Kriterien erfüllt** :
* Das Cookie muss die Zeichenfolge „conectix“ enthalten.
* Der Datenträgertyp muss „Fixed“ lauten.
* Die virtuelle Größe der VHD beträgt mindestens 20 MB.
* Die VHD ist ausgerichtet. (Das heißt, dass die virtuelle Größe ein Vielfaches von 1 MB sein muss.)
* VHD-Bloblänge = virtuelle Größe + VHD-Fußzeilenlänge (512)

Sie können die VHD-Spezifikation hier [herunterladen](https://www.microsoft.com/download/details.aspx?id=23850).


## <a name="software-compliance-for-windows"></a>Softwarekompatibilität für Windows

Wenn die Windows-Imageanforderung aufgrund eines Problems mit der Softwarekompatibilität abgelehnt wird, haben Sie möglicherweise ein Windows-Image mit installierter SQL Server-Instanz erstellt, anstatt das Basisimage mit der relevanten SQL-Version aus Azure Marketplace zu verwenden.

Erstellen Sie kein eigenes Windows-Image, in dem SQL Server installiert ist. Verwenden Sie stattdessen die genehmigten SQL-Basisimages (Enterprise/Standard/Web) aus Azure Marketplace.

Wenn Sie Visual Studio oder ein lizenziertes Office-Produkt installieren möchten, holen Sie vorab die Genehmigung des Supportteams ein.

Weitere Informationen zum Auswählen einer genehmigten Basis finden Sie im Thema zum [Erstellen eines virtuellen Computers aus einer genehmigten Basis](azure-vm-create-using-approved-base.md).

## <a name="tool-kit-test-case-execution-failed"></a>Fehler beim Ausführen des Toolkit-Testfalls

Mit dem Microsoft Certification-Toolkit können Sie Testfälle ausführen und überprüfen, ob Ihre VHD oder Ihr Image mit der Azure-Umgebung kompatibel ist.

Laden Sie das [Microsoft Certification-Toolkit](azure-vm-image-test.md) herunter.

## <a name="linux-test-cases"></a>Linux-Testfälle

In der folgenden Liste sind die Linux-Testfälle aufgelistet, die vom Toolkit ausgeführt werden. Die Testvalidierung wird in der Beschreibung angegeben.

|Szenario|Testfall|BESCHREIBUNG|
|---|---|---|
|1|Bashverlauf|Bashverlaufsdateien sollten vor dem Erstellen des VM-Images gelöscht werden.|
|2|Linux-Agent-Version|Azure Linux Agent 2.2.41 oder höher muss installiert sein.|
|3|Erforderliche Kernelparameter|Überprüft, ob die folgenden Kernelparameter festgelegt sind: <br>console=ttyS0<br>earlyprintk=ttyS0<br>rootdelay=300|
|4|Swap-Partition auf dem Betriebssystemdatenträger|Stellt sicher, dass Swap-Partitionen nicht auf dem Betriebssystemdatenträger erstellt werden.|
|5|Stammpartition auf dem Betriebssystemdatenträger|Erstellen Sie eine einzelne Stammpartition für den Betriebssystemdatenträger.|
|6|OpenSSL-Version|Sie müssen mindestens Version v0.9.8 von OpenSSL verwenden.|
|7|Python-Version|Dringend empfohlen wird Python-Version 2.6 oder höher.|
|8|Client-Alive-Intervall|Legen Sie das ClientAliveInterval auf 180 fest. Je nach Anforderung der Anwendung kann es auf einen Wert zwischen 30 und 235 festgelegt werden. Wenn Sie SSH für Ihre Endbenutzer aktivieren, muss dieser Wert gemäß der Beschreibung festgelegt werden.|
|9|Betriebssystemarchitektur|Es werden nur 64-Bit-Betriebssysteme unterstützt.|
|10|Automatische Aktualisierung|Gibt an, ob die automatische Aktualisierung des Linux-Agents aktiviert ist.|

### <a name="common-errors-found-while-executing-previous-test-cases"></a>Allgemeine Fehler beim Ausführen der obigen Testfälle

Die folgende Tabelle enthält allgemeine Fehler, die beim Ausführen der obigen Testfälle auftreten:
 
|Szenario|Testfall|Fehler|Lösung|
|---|---|---|---|
|1|Testfall zur Überprüfung der Linux-Agent-Version|Die mindestens erforderliche Linux-Agent-Version ist 2.2.41 oder höher. Diese Anforderung gilt seit dem 1. Mai 2020.|Aktualisieren Sie die Version des Linux-Agents auf 2.241 oder höher. Weitere Informationen finden Sie auf der [Seite mit Updates für die Linux-Agent-Version](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).|
|2|Bashverlauf-Testfall|Es wird ein Fehler angezeigt, wenn die Größe des Bashverlaufs in Ihrem gesendeten Image 1 Kilobyte (KB) überschreitet. Die Größe ist auf 1 KB beschränkt, um sicherzustellen, dass keine potenziell sensiblen Informationen in der Bashverlaufsdatei erfasst werden.|Um dieses Problem zu beheben, binden Sie die VHD auf einer beliebigen anderen funktionierenden VM ein, und nehmen Sie Änderungen vor (löschen Sie z. B. die Verlaufsdateien vom Typ *.bash* ), um die Größe auf 1 KB oder weniger zu reduzieren.|
|3|Testfall für erforderliche Kernelparameter|Dieser Fehler wird angezeigt, wenn der Wert für **console** nicht auf **ttyS0** festgelegt ist. Überprüfen Sie die Änderung, indem Sie den folgenden Befehl ausführen:<br>`cat /proc/cmdline`|Legen Sie den Wert für **console** auf **ttyS0** fest, und senden Sie die Anforderung noch einmal.|
|4|Testfall für das Client-Alive-Intervall|Wird im Toolkit-Ergebnis für diesen Testfall ein Fehler zurückgegeben, ist für **ClientAliveInterval** ein unzulässiger Wert vorhanden.|Legen Sie den Wert für **ClientAliveInterval** auf maximal 235 fest, und senden Sie die Anforderung noch einmal.|

### <a name="windows-test-cases"></a>Windows-Testfälle

Die folgende Tabelle enthält die Windows-Testfälle, die vom Toolkit ausgeführt werden, sowie eine Beschreibung der Testüberprüfung:

|Szenario |Testfälle|BESCHREIBUNG|
|---|---|---|---|
|1|Betriebssystemarchitektur|Azure unterstützt nur 64-Bit-Betriebssysteme.|
|2|Abhängigkeit von Benutzerkonten|Die Ausführung der Anwendung sollte nicht vom Administratorkonto abhängen.|
|3|Failovercluster|Das Feature „Windows Server-Failoverclustering“ wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|4|IPV6|IPv6 wird in der Azure-Umgebung noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|5|DHCP|Die Serverrolle „Dynamic Host Configuration-Protokoll“ wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|6|Hyper-V|Die Serverrolle „Hyper-V“ wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|7|Remotezugriff|Die Serverrolle „Remotezugriff (Direktzugriff)“ wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|8|Rights Management Services|Rights Management Services. Die Serverrolle wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|9|Windows-Bereitstellungsdienste|Windows-Bereitstellungsdienste. Die Serverrolle wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|10|BitLocker-Laufwerkverschlüsselung|Die BitLocker-Laufwerkverschlüsselung wird auf der Betriebssystem-Festplatte nicht unterstützt, aber möglicherweise auf Datenträgern verwendet.|
|11|Internet Storage Name Server|Das Feature „Internet Storage Name Server“ wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|12|Multipfad-E/A|Multipfad-E/A. Diese Serverfunktion wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|13|Netzwerklastenausgleich|Netzwerklastenausgleich. Diese Serverfunktion wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|14|Peer Name Resolution-Protokoll|Peer Name Resolution-Protokoll. Diese Serverfunktion wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|15|SNMP-Dienste|Das Feature „SNMP-Dienste“ (Simple Network Management-Protokoll) wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|16|WINS (Windows Internet Name Service)|WINS (Windows Internet Name Service). Diese Serverfunktion wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|17|WLAN-Dienst|WLAN-Dienst. Diese Serverfunktion wird noch nicht unterstützt. Die Anwendung sollte nicht von diesem Feature abhängen.|
|

Wenn im Zusammenhang mit den obigen Testfällen Fehler auftreten, finden Sie in der Tabelle in der Spalte **Beschreibung** Hinweise zur Fehlerbehebung. Wenn Sie weitere Informationen benötigen, wenden Sie sich an das Supportteam. 

## <a name="data-disk-size-verification"></a>Überprüfung der Datenträgergröße

Wenn die Größe einer mit dem Datenträger gesendeten Anforderung 1023 Gigabyte (GB) überschreitet, wird die Anforderung nicht genehmigt. Diese Regel gilt sowohl für Linux als auch für Windows.

Senden Sie die Anforderung mit einer Größe von maximal 1.023 GB noch einmal.

## <a name="os-disk-size-validation"></a>Validierung der Betriebssystem-Datenträgergröße

In den folgenden Regeln finden Sie Einschränkungen hinsichtlich der Betriebssystem-Datenträgergröße. Überprüfen Sie beim Senden von Anforderungen, ob die Betriebssystem-Datenträgergröße innerhalb der Grenzen für Linux bzw. Windows liegt.

|OS|Empfohlene VHD-Größe|
|---|---|
|Linux|30 GB bis 1023 GB|
|Windows|30 GB bis 250 GB|
|

Da VMs den Zugriff auf das zugrunde liegende Betriebssystem zulassen, vergewissern Sie sich, dass die Größe der VHD ausreichend ist. Da Datenträger nicht ohne Downtime erweitert werden können, verwenden Sie eine Datenträgergröße zwischen 30 und 50 GB.

|VHD-Größe|Tatsächlich belegte Größe|Lösung|
|---|---|---|
|Mehr als 500 Tebibyte (TiB)|–|Wenden Sie sich wegen einer Ausnahmegenehmigung an das Supportteam.|
|250-500 TiB|Differenz von mehr als 200 Gibibyte (GiB) zur Blobgröße|Wenden Sie sich wegen einer Ausnahmegenehmigung an das Supportteam.|
|

> [!NOTE]
> Größere Datenträgergrößen verursachen höhere Kosten und bewirken während der Einrichtung und Replikation eine Verzögerung. Aufgrund dieser Verzögerung und Kosten kann das Supportteam eine begründete Ausnahmegenehmigung erteilen.

## <a name="wannacry-patch-verification-test-for-windows"></a>Test zur Überprüfung auf WannaCry-Patch für Windows

Um einen potenziellen Angriff im Zusammenhang mit dem WannaCry-Virus zu verhindern, stellen Sie sicher, dass alle Windows-Image-Anforderungen mit dem neuesten Patch aktualisiert sind.

Angaben zum Betriebssystem und zur unterstützten Mindestversion der gepatchten Version von Windows Server finden Sie in der folgenden Tabelle: 

Die Imagedateiversion kann über `C:\windows\system32\drivers\srv.sys` oder `srv2.sys`überprüft werden.

> [!NOTE]
> Für WindowsServer2019 gelten keine obligatorischen Versionsanforderungen.

|OS|Version|
|---|---|
|Windows Server 2008 R2|6.1.7601.23689|
|Windows Server 2012|6.2.9200.22099|
|Windows Server 2012 R2|6.3.9600.18604|
|Windows Server 2016|10.0.14393.953|
|Windows Server 2019|Nicht verfügbar|
|

## <a name="sack-vulnerability-patch-verification"></a>Überprüfung auf Patch für SACK-Sicherheitsrisiken

Wenn Sie ein Linux-Image senden, wird Ihre Anforderung möglicherweise aufgrund von Problemen mit der Kernelversion abgelehnt.

Aktualisieren Sie den Kernel mit einer genehmigten Version, und senden Sie die Anforderung noch einmal. Die genehmigte Kernelversion finden Sie in der folgenden Tabelle. Die Versionsnummer muss mindestens der hier aufgeführten entsprechen.

Wenn das Image nicht mit einer der folgenden Kernelversionen installiert ist, aktualisieren Sie es mit den richtigen Patches. Fordern Sie die nötige Genehmigung vom Supportteam an, nachdem das Image mit diesen erforderlichen Patches aktualisiert wurde:

- CVE-2019-11477 
- CVE-2019-11478 
- CVE-2019-11479

|Betriebssystemfamilie|Version|Kernel|
|---|---|---|
|Ubuntu|14.04 LTS|4.4.0-151| 
||14.04 LTS|4.15.0-1049-*-azure|
||16.04 LTS|4.15.0-1049|
||18.04 LTS|4.18.0-1023|
||18.04 LTS|5.0.0-1025|
||18.10|4.18.0-1023|
||19.04|5.0.0-1010|
||19.04|5.3.0-1004|
|RHEL und CentOS|6.10|2.6.32-754.15.3|
||7.2|3.10.0-327.79.2|
||7.3|3.10.0-514.66.2|
||7.4|3.10.0-693.50.3|
||7,5|3.10.0-862.34.2|
||7.6|3.10.0-957.21.3|
||7,7|3.10.0-1062.1.1|
||8.0|4.18.0-80.4.2|
||8.1|4.18.0-147|
||„7-RAW“ (7.6)||
||„7-LVM“ (7.6)|3.10.0-957.21.3|
||RHEL-SAP 7.4|TBD|
||RHEL-SAP 7.5|TBD|
|SLES|SLES11SP4 (einschließlich SAP)|3.0.101-108.95.2|
||SLES12SP1 für SAP|3.12.74-60.64.115.1|
||SLES12SP2 für SAP|4.4.121-92.114.1|
||SLES12SP3|4.4180-4.31.1 (Kernel-Azure)|
||SLES12SP3 für SAP|4.4.180-94.97.1|
||SLES12SP4|4.12.14-6.15.2 (Kernel-Azure)|
||SLES12SP4 für SAP|4.12.14-95.19.1|
||SLES15|4.12.14-5.30.1 (Kernel-Azure)|
||SLES15 für SAP|4.12.14-5.30.1 (Kernel-Azure)|
||SLES15SP1|4.12.14-5.30.1 (Kernel-Azure)|
|Oracle|6.10|UEK2 2.6.39-400.312.2<br>UEK3 3.8.13-118.35.2<br>RHCK 2.6.32-754.15.3 
||7.0-7.5|UEK3 3.8.13-118.35.2<br>UEK4 4.1.12-124.28.3<br>RHCK folgt auf obiges RHEL|
||7.6|RHCK 3.10.0-957.21.3<br>UEK5 4.14.35-1902.2.0|
|CoreOS Stable 2079.6.0|4.19.43*|
||Beta 2135.3.1|4.19.50*|
||Alpha 2163.2.1|4.19.50*|
|Debian|Jessie (Sicherheit)|3.16.68-2|
||Jessie-Backports|4.9.168-1+deb9u3|
||Stretch (Sicherheit)|4.9.168-1+deb9u3|
||Debian GNU/Linux 10 (Buster)|Debian 6.3.0-18+deb9u1|
||Buster, SID (Stretch-Backports)|4.19.37-5|
|

## <a name="image-size-should-be-in-multiples-of-megabytes"></a>Die Imagegröße muss ein Megabyte-Vielfaches sein.

Alle VHDs in Azure müssen eine virtuelle Größe aufweisen, die sich an einem Vielfachen von 1 Megabyte (MB) orientiert. Wenn Ihre VHD nicht empfohlene virtuelle Größe aufweist, wird Ihre Anforderung möglicherweise zurückgewiesen.

Befolgen Sie beim Konvertieren von einem RAW-Datenträger zu einer VHD die Richtlinien. Sie müssen sicherstellen, dass die Größe des RAW-Datenträgers ein Vielfaches von 1 MB beträgt. Weitere Informationen finden Sie unter [Informationen zu nicht unterstützten Verteilungen](../virtual-machines/linux/create-upload-generic.md).

## <a name="vm-access-denied"></a>VM-Zugriff verweigert

Treten Probleme im Zusammenhang mit verweigertem Zugriff beim Ausführen der Testfälle für die VM auf, ist dies möglicherweise auf unzureichende Berechtigungen zum Ausführen der Testfälle zurückzuführen.

Überprüfen Sie, ob für das Konto, unter dem die Testfälle ausgeführt werden, der entsprechende Zugriff aktiviert ist. Wenn der Zugriff nicht aktiviert ist, aktivieren Sie ihn, um die Testfälle auszuführen. Wenn Sie den Zugriff nicht aktivieren möchten, können Sie die Ergebnisse des Selbsttestfalls mit dem Supportteam teilen.

Wenn Sie Ihre Anforderung mit einem Image mit deaktiviertem SSH für den Zertifizierungsprozess übermitteln möchten, führen Sie die folgenden Schritte aus:

1. Führen Sie das Azure-Toolkit für Ihr Image aus. (Laden Sie das [neueste Toolkit](https://aka.ms/AzureCertificationTestTool) herunter.)

2. Erstellen Sie ein [Supportticket](https://aka.ms/marketplacepublishersupport), fügen Sie den Toolkitbericht an, und geben Sie Angebotsdetails an: Angebotsname, Herausgebername, Plan-ID/-SKU und Version.

3. Übermitteln Sie Ihre Zertifizierungsanforderung erneut.


## <a name="download-failure"></a>Fehler beim Herunterladen
    
In der folgenden Tabelle finden Sie alle Probleme, die auftreten können, wenn Sie das VM-Image mithilfe einer SAS-URL (Shared Access Signature) herunterladen:

|Szenario|Fehler|`Reason`|Lösung|
|---|---|---|---|
|1|Nicht gefundenes Blob|Die VHD wurde entweder gelöscht oder vom angegebenen Speicherort verschoben.|| 
|2|Blob wird verwendet|Die VHD wird von einem anderen internen Prozess verwendet.|Die virtuelle Festplatte muss sich beim Herunterladen über eine SAS-URL im Status „Verwendet“ befinden.|
|3|Ungültige SAS-URL|Die zugehörige SAS-URL für die VHD ist falsch.|Rufen Sie die richtige SAS-URL ab.|
|4|Ungültige Signatur|Die zugehörige SAS-URL für die VHD ist falsch.|Rufen Sie die richtige SAS-URL ab.|
|6|Bedingter HTTP-Header|Die SAS-URL ist ungültig.|Rufen Sie die richtige SAS-URL ab.|
|7|Ungültiger VHD-Name|Überprüfen Sie, ob Sonderzeichen wie das Prozentzeichen (%) oder Anführungszeichen (") im VHD-Namen enthalten sind.|Benennen Sie die VHD-Datei um, und entfernen Sie die Sonderzeichen.|
|

## <a name="first-mb-2048-kb-partition-only-for-linux"></a>Erste MB-Partition (2.048 KB) (nur für Linux)

Stellen Sie beim Senden der VHD sicher, dass die ersten 2.048 KB der VHD leer sind. Andernfalls tritt bei der Anforderung ein Fehler auf.

>[!NOTE]
>\* Für bestimmte besondere Images wie die auf Azure Windows-Basisimages aus Azure Marketplace erstellten wird nach einem Abrechnungstag gesucht, und die MB-Partition wird ignoriert, wenn das Abrechnungstag vorhanden ist und unseren internen verfügbaren Werten entspricht.


## <a name="steps-for-creating-first-mb-2048-kb-partition-only-for-linux-on-an-empty-vhd"></a>Schritte zum Erstellen der ersten MB-Partition (2.048 KB) (nur für Linux) auf einer leeren VHD

Schritt 1: Erstellen Sie einen beliebigen Typ von VM (z. B.: Ubuntu, CentOS usw.). Füllen Sie die erforderlichen Felder aus, und klicken Sie auf „Weiter: Datenträger >“.
![Nächste Schritte: Datenträger >](./media/create-vm/vm-certification-issues-solutions-15.png)

Schritt 2: Erstellen Sie einen nicht verwalteten Datenträger für die obige VM.
![Erstellen eines nicht verwalteten Datenträgers](./media/create-vm/vm-certification-issues-solutions-16.png)

Beachten Sie, dass Sie die Standardwerte verwenden oder beliebige Werte für die Felder wie NIC, NSG und öffentliche IP-Adresse angeben können.

Schritt 3: Klicken Sie nach dem Erstellen des virtuellen Computers auf „Datenträger“. Sie finden diese Option auf der linken Seite, wie unten dargestellt. ![Klicken auf „Datenträger“](./media/create-vm/vm-certification-issues-solutions-17.png)

Schritt 4:  Fügen Sie die VHD als Datenträger an die obige VM an, um die Partitionstabelle wie unten dargestellt zu erstellen.
![Anfügen der VHD](./media/create-vm/vm-certification-issues-solutions-18.png)

Klicken Sie auf „Datenträger hinzufügen“ und „Vorhandenes Blob“, navigieren Sie zu Ihrem VHD-Speicherkonto, und klicken Sie auf „Container“. Wählen Sie dann Ihre VHD aus, und klicken Sie auf „OK“, wie unten gezeigt.
![VHD auswählen](./media/create-vm/vm-certification-issues-solutions-19.png)

Die VHD wird als Datenträger „LUN 0“ hinzugefügt. Starten Sie den virtuellen Computer nach dem Hinzufügen des Datenträgers neu.

Schritt 5:  Melden Sie sich nach dem Neustart des virtuellen Computers mithilfe von PuTTY (oder einem anderen Client) auf der VM an, und führen Sie den Befehl „sudo -i“ aus, um root-Zugriff zu erhalten.

![Anmelden auf der VM](./media/create-vm/vm-certification-issues-solutions-20.png)

Schritt 6: Führen Sie die folgenden Schritte aus, um auf der VHD eine Partition zu erstellen.

a) Geben Sie den Befehl „fdisk /dev/sdb“ ein.

b) Geben Sie zum Anzeigen der bestehenden Partitionsliste auf der VHD „p“ ein.

c) Geben Sie „d“ ein, um alle auf der VHD vorhandenen Partitionen zu löschen. (Sie können diesen Schritt überspringen, wenn er nicht erforderlich ist.) ![Löschen aller vorhandenen Partitionen](./media/create-vm/vm-certification-issues-solutions-21.png)

d) Geben Sie „n“ ein, um eine neue Partition zu erstellen, und wählen Sie „p“ aus, um eine primäre Partition zu erstellen.

e) Geben Sie als Wert für den ersten Sektor „2048“ ein. Den Wert für den letzten Sektor können Sie unverändert lassen, da der Standardwert verwendet wird. Beachten Sie, dass alle Daten bis 2.048 KB gelöscht werden.
           
>[!NOTE]
>\* Beachten Sie, dass durch das Erstellen der Partition nach der Beschreibung oben alle zuvor vorhandenen Daten bis 2.048 KB gelöscht werden. Es wird daher empfohlen, vor dem Ausführen des obigen Befehls eine Sicherung der virtuellen Festplatte durchzuführen.

Sehen Sie sich den folgenden Screenshot als Referenz an.
![Gelöschte Daten](./media/create-vm/vm-certification-issues-solutions-22.png)

f) Geben Sie „w“ ein, um die Erstellung der Partition zu bestätigen. 

![Erstellung der Partition](./media/create-vm/vm-certification-issues-solutions-23.png)

g) Sie können die Partitionstabelle überprüfen, indem Sie den Befehl „fdisk /dev/sdb“ und dann „p“ eingeben. Sie sehen dann, dass die Partition wie unten mit einem Offset von 2.048 KB erstellt wurde. 

 ![2\.048-KB-Offset](./media/create-vm/vm-certification-issues-solutions-24.png)

Schritt 7: Trennen Sie die VHD vom virtuellen Computer, und löschen Sie den virtuellen Computer.

         
## <a name="steps-for-creating-first-mb-2048-kb-partition-only-for-linux-by-moving-the-existing-data-on-vhd"></a>Schritte zum Erstellen der ersten MB-Partition (2048 KB) (nur für Linux) durch Verschieben vorhandener Daten auf eine VHD

Schritt 1: Erstellen Sie einen beliebigen Typ von VM (z. B.: Ubuntu, CentOS usw.). Füllen Sie die erforderlichen Felder aus, und klicken Sie auf „Weiter: Datenträger >“.
![Klicken auf „Weiter: Datenträger >“](./media/create-vm/vm-certification-issues-solutions-15.png)

Schritt 2: Erstellen Sie einen nicht verwalteten Datenträger für die obige VM.
![Erstellen eines nicht verwalteten Datenträgers](./media/create-vm/vm-certification-issues-solutions-16.png)

Beachten Sie, dass Sie die Standardwerte verwenden oder beliebige Werte für die Felder wie NIC, NSG und öffentliche IP-Adresse angeben können.

Schritt 3: Klicken Sie nach dem Erstellen des virtuellen Computers auf „Datenträger“. Sie finden diese Option auf der linken Seite, wie unten dargestellt. ![Klicken auf „Datenträger“](./media/create-vm/vm-certification-issues-solutions-17.png)

Schritt 4:  Fügen Sie die VHD als Datenträger an die obige VM an, um die Partitionstabelle wie unten dargestellt zu erstellen.
![Partitionstabelle](./media/create-vm/vm-certification-issues-solutions-18.png)

Klicken Sie auf „Datenträger hinzufügen“ und „Vorhandenes Blob“, navigieren Sie zu Ihrem VHD-Speicherkonto, und klicken Sie auf „Container“. Wählen Sie dann Ihre VHD aus, und klicken Sie auf „OK“, wie unten gezeigt.
![VHD auswählen](./media/create-vm/vm-certification-issues-solutions-19.png)

Die VHD wird als Datenträger „LUN 0“ hinzugefügt. Starten Sie den virtuellen Computer nach dem Hinzufügen des Datenträgers neu.

Schritt 5:  Melden Sie sich nach dem Neustart des virtuellen Computers mithilfe von PuTTY auf der VM an, und führen Sie den Befehl „sudo -i“ aus, um root-Zugriff zu erhalten. \
![Anmelden nach dem Neustart](./media/create-vm/vm-certification-issues-solutions-20.png)

Schritt 6: Führen Sie den Befehl „echo '+1M,' | sfdisk --move-data /dev/sdc -N 1“ aus. ![Ausführen des Befehls](./media/create-vm/vm-certification-issues-solutions-25.png)

>[!NOTE]
>\* Beachten Sie, dass der obige Befehl möglicherweise mehr Zeit in Anspruch nimmt, da er von der Größe des Datenträgers abhängig ist.

Schritt 7: Trennen Sie die VHD vom virtuellen Computer, und löschen Sie den virtuellen Computer.


## <a name="default-credentials"></a>Standardanmeldeinformationen

Stellen Sie stets sicher, dass beim Senden der VHD nicht die Standardanmeldeinformationen übermittelt werden. Das Hinzufügen von Standardanmeldeinformationen macht die VHD anfälliger gegenüber Sicherheitsbedrohungen. Erstellen Sie stattdessen beim Senden der VHD eigene Anmeldeinformationen.
  
## <a name="datadisk-mapped-incorrectly"></a>DataDisk nicht ordnungsgemäß zugeordnet

Wenn eine Anforderung mit mehreren Datenträgern gesendet wird, diese jedoch nicht in aufeinanderfolgender Reihenfolge übermittelt werden, wird dies als Zuordnungsproblem angesehen. Wenn z. B. drei Datenträger vorhanden sind, muss die Nummerierreihenfolge *0, 1, 2* sein. Alle anderen Reihenfolgen werden als Zuordnungsproblem behandelt.

Senden Sie die Anforderung mit ordnungsgemäßer Sequenzierung der Datenträger noch einmal.

## <a name="incorrect-os-mapping"></a>Falsche Betriebssystemzuordnung

Beim Erstellen eines Images kann dieses der falschen Betriebssystembezeichnung zugeordnet oder zugewiesen werden. Wenn Sie beim Erstellen des Images beispielsweise **Windows** als Teil des Betriebssystemnamens auswählen, darf der Betriebssystem-Datenträger nur mit Windows installiert werden. Die gleiche Anforderung gilt für Linux.

## <a name="vm-not-generalized"></a>VM ist nicht generalisiert

Wenn alle Images aus Azure Marketplace wiederverwendet werden sollen, muss die Betriebssystem-VHD generalisiert werden.

* Unter **Linux** wird mit dem folgenden Prozess eine Linux-VM generalisiert und als separate VM erneut bereitgestellt.

  Geben Sie im SSH-Fenster den folgenden Befehl ein: `sudo waagent -deprovision+user`

* Unter **Windows** generalisieren Sie Windows-Images mithilfe von `sysreptool`.

Weitere Informationen zu diesem Tool finden Sie unter [Sysprep (Systemvorbereitung) – Übersicht]( https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).

## <a name="datadisk-errors"></a>DataDisk-Fehler

In der folgenden Tabelle sind Fehlerbehebungsoptionen für Fehler in Verbindung mit dem Datenträger aufgeführt:

|Fehler|`Reason`|Lösung|
|---|---|---|
|`DataDisk- InvalidUrl:`|Dieser Fehler kann darauf zurückzuführen sein, dass für die logische Gerätenummer (Logical Unit Number, LUN) beim Übermitteln des Angebots eine ungültige Anzahl angegeben wurde.|Vergewissern Sie sich, dass sich die Sequenz der LUN-Nummern für den Datenträger in Partner Center befindet.|
|`DataDisk- NotFound:`|Dieser Fehler kann darauf zurückzuführen sein, dass ein Datenträger unter einer angegebenen SAS-URL nicht gefunden wurde.|Überprüfen Sie, ob sich der Datenträger unter der in der Anforderung angegebenen SAS-URL befindet.|
|

## <a name="remote-access-issue"></a>Problem mit dem Remotezugriff

Wenn für das Windows-Image nicht die RDP-Option (Remote Desktop Protocol, Remotedesktopprotokoll) aktiviert ist, wird dieser Fehler angezeigt. 

Aktivieren Sie vor dem Senden den RDP-Zugriff für Windows-Images.

## <a name="bash-history-failed"></a>Fehler beim Bashverlauf

Dieser Fehler tritt auf, wenn die Größe des Bashverlaufs in Ihrem gesendeten Image 1 Kilobyte (KB) überschreitet. Die Größe ist auf 1 KB beschränkt, um sicherzustellen, dass keine potenziell sensiblen Informationen in der Bashverlaufsdatei erfasst werden.

Im Folgenden finden Sie die Schritte zum Löschen des Bashverlaufs:

Schritt 1: Stellen Sie den virtuellen Computer bereit, und klicken Sie im Azure-Portal auf die Option „Befehl ausführen“.
![„Befehl ausführen“ im Azure-Portal](./media/create-vm/vm-certification-issues-solutions-3.png)

Schritt 2: Wählen Sie die erste Option „RunShellScript“ aus, und führen Sie den folgenden Befehl aus:

Befehl: „cat /dev/null > ~/.bash_history && history -c“ ![Befehl für den Bashverlauf im Azure-Portal](./media/create-vm/vm-certification-issues-solutions-4.png)

Schritt 3: Nachdem der Befehl erfolgreich ausgeführt wurde, starten Sie den virtuellen Computer neu.

Schritt 4. Generalisieren Sie den virtuellen Computer, extrahieren Sie die Image-VHD, und beenden Sie den virtuellen Computer.

Schritt 5: Übermitteln Sie das generalisierte Image erneut.

## <a name="requesting-exceptions-custom-templates-on-vm-images-for-selective-tests"></a>Anfordern von Ausnahmen (benutzerdefinierte Vorlagen) für VM-Images für selektive Tests

Herausgeber können für einige Tests, die während der VM-Zertifizierung durchgeführt werden, Ausnahmen anfordern. Ausnahmen werden in äußerst seltenen Fällen gemacht, wenn der Herausgeber die Anforderung fundiert begründet. Das Zertifizierungsteam behält sich das Recht vor, Ausnahmen jederzeit abzulehnen oder zu genehmigen.

In den folgenden Abschnitten werden die wichtigsten Szenarien erläutert, in denen Ausnahmen angefordert werden, und es wird beschrieben, wie Sie eine Ausnahme anfordern.

### <a name="scenarios-for-exception"></a>Szenarien für eine Ausnahme

Es gibt grundsätzlich drei Szenarien/Fälle, in denen Herausgeber Ausnahmen anfordern.

- **Ausnahme für mindestens einen Testfall** : Herausgeber kontaktieren den Partner Center- [Support](https://aka.ms/marketplacepublishersupport), um Ausnahmen für Testfälle anzufordern.

- **Gesperrte VMs/kein Root-Zugriff** : Einige Herausgeber verfügen über Szenarien, in denen VMs gesperrt werden müssen, da Software wie Firewalls auf der VM installiert ist. In diesem Fall können Herausgeber das [Certified Test Tool](https://aka.ms/AzureCertificationTestTool) herunterladen und den Bericht beim Partner Center-[Support](https://aka.ms/marketplacepublishersupport) einreichen.

- **Benutzerdefinierte Vorlagen** : Einige Herausgeber veröffentlichen VM-Images, für die eine benutzerdefinierte ARM-Vorlage zum Bereitstellen der VMs erforderlich ist. In diesem Fall sollten die Herausgeber die benutzerdefinierten Vorlagen beim Partner Center-[Support](https://aka.ms/marketplacepublishersupport) einreichen, damit diese vom Zertifizierungsteam für die Überprüfung verwendet werden können.

### <a name="information-to-provide-for-exception-scenarios"></a>Zu übermittelnde Informationen für Ausnahmeszenarien

Herausgeber sollten sich unter Angabe der folgenden Informationen an den Partner Center-[Support](https://aka.ms/marketplacepublishersupport) wenden, um Ausnahmen für das oben beschriebene Szenario anzufordern:

   1. Herausgeber-ID: die Herausgeber-ID im Partner Center-Portal
   2. Angebots-ID/-Name: ID/Name des Angebots, für das die Ausnahme angefordert wird
   3. SKU/Plan-ID: die Plan-ID/SKU des VM-Angebots, für das eine Ausnahme angefordert wird
   4. Version: die Version des VM-Angebots, für das eine Ausnahme angefordert wird
   5. Ausnahmetyp: Tests, gesperrte VM, benutzerdefinierte Vorlagen
   6. Grund für die Anforderung: der Grund für diese Ausnahme und Informationen zu den auszuschließenden Tests
   7. Zeitachse: das Datum, bis zu dem die Ausnahme angefordert wurde
   8. Anlage: Fügen Sie alle relevanten Belege an. Fügen Sie bei gesperrten VMs den Testbericht und bei benutzerdefinierten Vorlagen die benutzerdefinierte ARM-Vorlage als Anlage an. Wenn Sie den Testbericht für gesperrte VMs und die benutzerdefinierte ARM-Vorlage für benutzerdefinierte Vorlagen nicht anfügen, wird die Anforderung abgelehnt.

## <a name="address-a-vulnerability-or-exploit-in-a-vm-offer"></a>Beheben eines Sicherheitsrisikos oder Exploits in einem VM-Angebot

In diesem Abschnitt wird beschrieben, wie Sie ein neues VM-Image bereitstellen, wenn ein Sicherheitsrisiko oder Exploit bei einem Ihrer VM-Images erkannt wird. Dies betrifft nur Azure-VM-Angebote, die im Azure Marketplace veröffentlicht werden.

> [!NOTE]
> Sie können weder das letzte VM-Image aus einem Plan entfernen noch den Verkauf des letzten Plans für ein Angebot einstellen.

Führen Sie eines der folgenden Verfahren aus:

- Wenn Sie über ein neues VM-Image als Ersatz für das gefährdete VM-Image verfügen, informieren Sie sich unter [Bereitstellen eines korrigierten VM-Images](#provide-a-fixed-vm-image) unten.
- Wenn Sie kein neues VM-Image als Ersatz für das einzige VM-Image in einem Plan besitzen und den Plan beenden möchten, können Sie [den Verkauf des Plans einstellen](partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan).
- Wenn das einzige VM-Image im Angebot nicht ersetzt werden soll, empfiehlt es sich, [den Verkauf des Angebots einzustellen](partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan).

### <a name="provide-a-fixed-vm-image"></a>Bereitstellen eines korrigierten VM-Images

Führen Sie die folgenden Schritte aus, um ein korrigiertes VM-Image als Ersatz für ein VM-Image mit Sicherheitsrisiko oder Exploit bereitzustellen:

1. Stellen Sie ein neues VM-Image bereit, um das Sicherheitsrisiko oder den Exploit zu beheben.
2. Entfernen Sie das VM-Image mit dem Sicherheitsrisiko oder Exploit.
3. Veröffentlichen Sie das Angebot erneut.

#### <a name="provide-a-new-vm-image-to-address-the-security-vulnerability-or-exploit"></a>Bereitstellen eines neuen VM-Images zur Behebung des Sicherheitsrisikos oder Exploits

Um diese Schritte ausführen zu können, müssen Sie die technischen Ressourcen für das hinzuzufügende VM-Image vorbereiten. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers mit einer genehmigten Basis](azure-vm-create-using-approved-base.md) oder [Erstellen eines virtuellen Computers mit einem eigenen Image](azure-vm-create-using-own-image.md) und [Generieren eines SAS-URIs für Ihr VM-Image](azure-vm-get-sas-uri.md).

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie in der Spalte **Angebotsalias** das Angebot aus.
4. Wählen Sie auf der Registerkarte **Planübersicht** in der Spalte **Name** den Plan aus, dem Sie die VM hinzufügen möchten.
5. Wählen Sie auf der Registerkarte **Technische Konfiguration** unter **VM-Images** die Option **+ VM-Image hinzufügen** aus.

> [!NOTE]
> Sie können einem Plan jeweils nur ein VM-Image hinzufügen. Wenn Sie mehrere VM-Images hinzufügen möchten, veröffentlichen Sie das erste live, bevor Sie das nächste VM-Image hinzufügen.

6. Geben Sie in den angezeigten Feldern eine neue Datenträgerversion und das VM-Image an.
7. Wähen Sie **Entwurf speichern** aus.

Fahren Sie mit dem nächsten Abschnitt unten fort, um das VM-Image mit dem Sicherheitsrisiko zu entfernen.

#### <a name="remove-the-vm-image-with-the-security-vulnerability-or-exploit"></a>Entfernen des VM-Images mit dem Sicherheitsrisiko oder Exploit

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie in der Spalte **Angebotsalias** das Angebot aus.
4. Wählen Sie auf der Registerkarte **Planübersicht** in der Spalte **Name** den Plan mit der zu entfernenden VM aus.
5. Wählen Sie auf der Registerkarte **Technische Konfiguration** unter **VM-Images** neben dem zu entfernenden VM-Image **VM-Image entfernen** aus.
6. Wählen Sie im angezeigten Dialogfeld **Weiter** aus.
7. Wähen Sie **Entwurf speichern** aus.

Fahren Sie mit dem nächsten Abschnitt unten fort, um das Angebot erneut zu veröffentlichen.

#### <a name="republish-the-offer"></a>Erneutes Veröffentlichen des Angebots

1. Wählen Sie **Überprüfen und veröffentlichen** aus.
2. Wenn Sie Informationen für das Zertifizierungsteam bereitstellen müssen, fügen Sie diese dem Feld **Hinweise zur Zertifizierung** hinzu.
3. Wählen Sie **Veröffentlichen**.

Unter [Überprüfen und Veröffentlichen von Angeboten](review-publish-offer.md) erfahren Sie, wie Sie die Veröffentlichung abschließen.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von VM-Angebotseigenschaften](azure-vm-create-properties.md)
- [Aktive Marketplace-Prämien](partner-center-portal/marketplace-rewards.md)
- Wenn Sie Fragen oder Verbesserungsvorschläge haben, wenden Sie sich an den Partner Center-[Support](https://aka.ms/marketplacepublishersupport).
