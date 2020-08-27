---
title: Tutorial zum Exportieren von Daten aus Azure Data Box | Microsoft-Dokumentation
description: Hier erfahren Sie, welche Voraussetzungen für die Bereitstellung gelten und wie Sie Daten aus einer Azure Data Box exportieren.
services: databox
author: twooley
ms.service: databox
ms.subservice: pod
ms.topic: how-to
ms.date: 07/10/2020
ms.author: twooley
ms.openlocfilehash: 0ddadd8d2bddda0fdff6a126fe6c09d863139b44
ms.sourcegitcommit: 9c3cfbe2bee467d0e6966c2bfdeddbe039cad029
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88783619"
---
# <a name="tutorial-create-export-order-for-azure-data-box-preview"></a>Tutorial: Erstellen eines Exportauftrags für Azure Data Box (Vorschau)

Azure Data Box ist eine Hybridlösung, mit der Sie Daten aus Azure in ihren Standort verschieben können. In diesem Tutorial wird beschrieben, wie Sie einen Exportauftrag für Azure Data Box erstellen. Der Hauptgrund für die Erstellung eines Exportauftrags ist die Notfallwiederherstellung, falls der lokale Speicher kompromittiert wird und eine Sicherungskopie wiederhergestellt werden muss.

In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]
>
> * Voraussetzungen für den Export
> * Bestellen einer Data Box für den Export
> * Nachverfolgen des Exportauftrags
> * Stornieren des Exportauftrags

[!INCLUDE [Data Box feature is in preview](../../includes/data-box-feature-is-preview-info.md)]

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie das Gerät bestellen, müssen die folgenden Konfigurationsvoraussetzungen für den Data Box-Dienst und das Data Box-Gerät erfüllt sein.

### <a name="for-service"></a>Für den Dienst

[!INCLUDE [Data Box service prerequisites](../../includes/data-box-supported-subscriptions.md)]

* Stellen Sie sicher, dass Sie über eine Ressourcengruppe verfügen, die Sie mit Ihrer Azure Data Box verwenden können.

* Stellen Sie sicher, dass Ihr Azure Storage-Konto, aus dem Sie Daten exportieren möchten, zu einem der in [Unterstützte Speicherkonten](data-box-system-requirements.md#supported-storage-accounts) beschriebenen Speicherkontotypen gehört.

### <a name="for-device"></a>Für das Gerät

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie verfügen über einen Hostcomputer, der mit dem Netzwerk des Datencenters verbunden ist. Sie kopieren die Daten aus der Azure Data Box auf diesen Computer. Auf dem Hostcomputer muss ein unterstütztes Betriebssystem ausgeführt werden (siehe [Azure Data Box – Systemanforderungen](data-box-system-requirements.md)).

* Ihr Datencenter verfügt über ein Hochgeschwindigkeitsnetzwerk. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, kann auch eine 1-GbE-Datenverbindung verwendet werden, aber hierdurch wird die Geschwindigkeit der Kopiervorgänge beeinträchtigt.

## <a name="order-data-box-for-export"></a>Bestellen einer Data Box für den Export

Führen Sie die folgenden Schritte im Azure-Portal aus, um ein Gerät zu bestellen.

1. Melden Sie sich mit Ihren Microsoft Azure-Anmeldeinformationen unter folgender URL an: [https://portal.azure.com](https://portal.azure.com).

2. Wählen Sie **+ Ressource erstellen** aus, und suchen Sie nach *Azure Data Box*. Wählen Sie **Azure Data Box** aus.

   ![Erstellen von Ressourcen](media/data-box-deploy-export-ordered/azure-data-box-export-00b.png)

3. Klicken Sie auf **Erstellen**.

   ![Erstellen einer Azure Data Box](media/data-box-deploy-export-ordered/azure-data-box-export-00c.png)

4. Überprüfen Sie, ob der Azure Data Box-Dienst in Ihrer Region verfügbar ist. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie anschließend **Übernehmen** aus.

    |Einstellung  |Wert  |
    |---------|---------|
    |Übertragungstyp     | Wählen Sie **Nach Azure exportieren**.        |
    |Subscription     | Wählen Sie ein EA-, CSP- oder Azure Sponsorship-Abonnement für den Data Box-Dienst aus. <br> Das Abonnement ist mit Ihrem Abrechnungskonto verknüpft.       |
    |Resource group     |    Wählen Sie eine vorhandene Ressourcengruppe aus. <br> Eine Ressourcengruppe ist ein logischer Container für die Ressourcen, die zusammen verwaltet oder bereitgestellt werden können.         |
    |Azure-Quellregion    |    Wählen Sie die Azure-Region aus, in der sich Ihre Daten zurzeit befinden.         |
    |Zielland     |     Wählen Sie das Land aus, wohin Sie das Gerät versenden möchten.        |

   ![Auswählen Ihrer Data Box-Einstellungen](media/data-box-deploy-export-ordered/azure-data-box-export-01.png)

5. Wählen Sie **Data Box** aus. Die maximal nutzbare Kapazität für eine einzelne Bestellung beträgt 80 TB. Sie können mehrere Bestellungen für größere Datenmengen erstellen.

   ![Auswählen der Data Box-Kapazität](media/data-box-deploy-export-ordered/azure-data-box-export-02b.png)

6. Geben Sie in **Bestellung** die **Grundeinstellungen** für die Bestellung an. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie anschließend **Weiter** aus.

    |Einstellung  |Wert  |
    |---------|---------|
    |Subscription     | Das Abonnement wird auf der Grundlage Ihrer zuvor getroffenen Auswahl automatisch eingetragen.|
    |Resource group | Die Ressourcengruppe, die Sie zuvor ausgewählt haben. |
    |Name des Exportauftrags     |  Geben Sie einen Anzeigenamen an, um die Bestellung nachzuverfolgen. <br> Der Name kann zwischen 3 und 24 Zeichen lang sein und darf nur Buchstaben, Zahlen und Bindestriche enthalten. <br> Der Name muss mit einem Buchstaben oder einer Zahl beginnen und enden.      |

    ![Grundeinstellungen für den Exportauftrag](media/data-box-deploy-export-ordered/azure-data-box-export-03.png)

    Klicken Sie auf **Weiter: Datenauswahl**, um fortzufahren.

7. Wählen Sie in **Datenauswahl** die Option **Speicherkonto und Exporttyp hinzufügen** aus.

    ![Hinzufügen von Speicherkonto und Exporttyp](media/data-box-deploy-export-ordered/azure-data-box-export-03b.png)

8. Geben Sie in **Exportoption auswählen** die Exportoptiondetails an. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie anschließend **Hinzufügen** aus.

    |Einstellung  |Wert  |
    |---------|---------|
    |Speicherkonto     | Das Azure Storage-Konto, aus dem Sie Daten exportieren möchten. |
    |Exporttyp     | Gibt den Typ der aus **Alle Objekte** zu exportierenden Daten und **XML-Datei verwenden** an.<ul><li> **Alle Objekte**: Gibt an, dass der Auftrag alle Daten abhängig von Ihrer Auswahl für **Übertragungsoptionen** exportiert.</li><li> **XML-Datei verwenden**: Gibt eine XML-Datei an, die eine Reihe von Pfaden und Präfixen für Blobs und/oder Dateien enthält, die aus dem Speicherkonto exportiert werden sollen. Die XML-Datei muss sich im Container des ausgewählten Speicherkontos befinden, und die Auswahl aus Dateifreigaben wird derzeit nicht unterstützt. Die Datei muss eine nicht leere XML-Datei sein.</li></ul>        |
    |Übertragungsoptionen     |  Gibt die Datenübertragungsoptionen **Alle auswählen**, **Alle Blobs** und **Alle Dateien** an. <ul><li> **Alle auswählen**: Gibt an, dass alle Blobdateien und Azure-Dateien exportiert werden. Wenn Sie ein Speicherkonto verwenden, das nur Blobs (Blobspeicherkonten) unterstützt, ist die Option **Alle Dateien** nicht auswählbar.</li><li> **Alle Blobs**: Gibt an, dass nur Block- und Seitenblobs exportiert werden.</li><li> **Alle Dateien**: Gibt an, dass alle Dateien außer Blobs exportiert werden. Der Typ Ihres Speicherkontos (GPv1 und GPv2, Storage Premium oder Blobspeicher) bestimmt, welche Datentypen Sie exportieren können. Weitere Informationen finden Sie unter [Unterstützte Speichertypen](../storage/common/storage-import-export-requirements.md#supported-storage-types).</li></ul>         |
    |Ausführliches Protokoll einschließen     | Gibt an, ob Sie eine ausführliche Protokolldatei mit einer Liste aller Dateien wünschen, die erfolgreich exportiert wurden.        |

    > [!NOTE]
    >
    > Wenn Sie **XML-Datei verwenden** für die Einstellung **Exporttyp** auswählen, müssen Sie sicherstellen, dass die XML-Datei gültige Pfade und/oder Präfixe enthält. Sie müssen die XML-Datei erstellen und bereitstellen.  Wenn die Datei ungültig ist oder keine Daten mit den angegebenen Pfaden übereinstimmen, wird der Auftrag mit partiellen Daten oder ohne exportierte Daten beendet.

    Informationen zum Hinzufügen einer XML-Datei zu einem Container finden Sie unter [Exportauftrag mit XML-Datei](data-box-deploy-export-ordered.md#export-order-using-xml-file).

   ![Auswählen der Exportoption](media/data-box-deploy-export-ordered/azure-data-box-export-04b.png)

   Ein Beispiel für die XML-Eingabe finden Sie unter [Beispiel für eine XML-Datei](data-box-deploy-export-ordered.md#sample-xml-file).

9. Überprüfen Sie Ihre Einstellungen in **Datenauswahl**, und wählen Sie **Weiter: Kontaktdetails** aus.

   ![Kontaktdetails](media/data-box-deploy-export-ordered/azure-data-box-export-05.png)

10. Wählen Sie in **Kontakt Details** die Option **+ Lieferadresse hinzufügen** aus, um Ihre Versandinformationen einzugeben.

    ![Lieferadresse hinzufügen](media/data-box-deploy-export-ordered/azure-data-box-export-06.png)

11. Geben Sie unter **Lieferadresse hinzufügen** Ihren Vor- und Nachnamen, den Namen und die Postanschrift des Unternehmens sowie eine gültige Telefonnummer an. Wählen Sie **Überprüfen** aus. Der Dienst überprüft anhand der Lieferadresse die Verfügbarkeit des Diensts. Wenn der Dienst für die angegebene Lieferadresse verfügbar ist, erhalten Sie eine entsprechende Benachrichtigung.

    ![Überprüfen der Lieferadresse](media/data-box-deploy-export-ordered/azure-data-box-export-07.png)

    Wenn Sie in einer Region bestellen, wo der selbstverwaltete Versand verfügbar ist, können Sie diese Option auswählen. Weitere Informationen zum selbstverwalteten Versand finden Sie unter [Verwenden des selbstverwalteten Versands](data-box-portal-customer-managed-shipping.md).

12. Wählen Sie **Weiter** aus, wenn die Versanddetails erfolgreich überprüft wurden.

13. Überprüfen Sie in **Kontaktdetails** Ihre Lieferadresse und Ihre E-Mail-Adresse. Der Dienst sendet E-Mail-Benachrichtigungen in Bezug auf Aktualisierungen des Auftragsstatus an die angegebenen E-Mail-Adressen.

    Es wird empfohlen, eine E-Mail-Gruppenadresse zu verwenden, damit Sie weiterhin Benachrichtigungen erhalten, wenn ein Administrator die Gruppe verlässt.

    ![Bestelldetails](media/data-box-deploy-export-ordered/azure-data-box-export-09.png)

14. Klicken Sie auf **Weiter: Überprüfen + bestellen**. Sie müssen die Geschäftsbedingungen akzeptieren, um mit der Auftragserstellung fortzufahren.

15. Wählen Sie **Bestellen** aus. Die Erstellung des Auftrags dauert einige Minuten.

    ![Committen des Auftrags](media/data-box-deploy-export-ordered/azure-data-box-export-10.png)

## <a name="export-order-using-xml-file"></a>Exportauftrag mit XML-Datei

Wenn Sie **XML-Datei verwenden** auswählen, können Sie bestimmte Container und Blobs (Seite und Block) angeben, die Sie exportieren möchten. Sie müssen die Spezifikationen in [Beispiel für eine XML-Dateitabelle](#sample-xml-file) zum Formatieren Ihres XML-Codes befolgen. In den folgenden Schritten wird gezeigt, wie Sie eine XML-Datei zum Exportieren von Daten verwenden:

1. Wählen Sie für **Exporttyp** die Option **XML-Datei verwenden** aus. Dies ist Ihre XML-Datei, die bestimmte Blobdateien und Azure-Dateien angibt, die Sie exportieren möchten. Um die XML-Datei hinzuzufügen, wählen Sie **Klicken Sie hier, um eine XML-Datei auszuwählen**.
     ![XML-Datei](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-01.png)

2. Wählen Sie **+ Container**, um einen Container zu erstellen.
    ![XML-Datei](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-02.png)

3. Fügen Sie auf der Registerkarte **Neuer Container**, die auf der rechten Seite des Azure-Portals angezeigt wird, einen Namen für den Container hinzu. Der Name darf nur Kleinbuchstaben enthalten, und Sie können Zahlen und Bindestriche „-“ einschließen. Wählen Sie dann im Dropdownlistenfeld die **Öffentliche Zugriffsebene** aus. Sie sollten **Privat (nicht anonymer Zugriff)** auszuwählen, um andere Personen am Zugriff auf Ihre Daten zu hindern. Weitere Informationen zu Zugriffsebenen für Container finden Sie unter [Konfigurieren des anonymen öffentlichen Lesezugriffs für Container und Blobs](../storage/blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs).

   ![XML-Datei:](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-04.png)

4. Klicken Sie auf **Erstellen**.

   ![XML-Datei:](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-07.png)

   Wenn Ihr Container erfolgreich erstellt wurde, wird die folgende Meldung angezeigt:

   ![XML-Datei:](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-09.png)

5. Wählen Sie den Container aus, den Sie erstellt haben, und doppelklicken Sie darauf.

   ![XML-Datei:](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-08.png)

6. Wenn Sie auf den Container doppelklicken, werden die Containereigenschaften angezeigt. Nun können Sie Ihre XML-Datei anfügen (oder durchsuchen), die die Liste der zu exportierenden Blobs und/oder Azure-Dateien enthält. Wählen Sie die Option **Hochladen**.

   ![XML-Datei:](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-10c.png)

7. Die XML-Datei wurde dem Container erfolgreich hinzugefügt. Nur Blobs und Azure-Dateien, die Sie in dieser XML-Datei angegeben haben, werden exportiert.

   ![XML-Datei:](media/data-box-deploy-export-ordered/azure-data-box-export-sms-use-xml-file-12.png)

## <a name="track-the-order"></a>Nachverfolgen der Bestellung

Nachdem Sie die Bestellung aufgegeben haben, können Sie ihren Status im Azure-Portal nachverfolgen. Navigieren Sie zu Ihrer Data Box-Bestellung, und navigieren Sie dann zu **Übersicht**, um den Status anzuzeigen. Die Bestellung wird im Portal mit dem Status **Bestellt** angezeigt.

Nachdem die Gerätevorbereitung abgeschlossen ist, beginnt das Kopieren der Daten aus den ausgewählten Speicherkonten. Im Portal wird der Auftrag mit dem Status **Daten werden kopiert** angezeigt.

![Data Box-Exportauftrag wird verarbeitet](media/data-box-deploy-export-ordered/azure-data-box-export-15b.png)

Data Box kopiert Daten aus den Quellspeicherkonten. Nachdem das Kopieren der Daten abgeschlossen ist, wird Data Box gesperrt, und im Portal wird der Auftrag mit dem Status **Kopiervorgang abgeschlossen** angezeigt.

![Datenkopiervorgang für Data Box-Export ist abgeschlossen](media/data-box-deploy-export-ordered/azure-data-box-export-15c.png)

Sollte das Gerät nicht verfügbar sein, erhalten Sie eine entsprechende Benachrichtigung. Wenn das Gerät verfügbar ist, identifiziert Microsoft das Gerät für den Versand und bereitet den Versand vor. Während der Vorbereitung des Geräts werden folgende Aktionen ausgeführt:

* Für jedes mit dem Gerät verknüpfte Speicherkonto werden SMB-Freigaben erstellt.
* Für jede Freigabe werden Anmeldeinformationen wie Benutzername und Kennwort generiert.
* Das Gerät ist gesperrt, und für den Zugriff ist das Kennwort zum Entsperren des Geräts erforderlich. Zum Abrufen des Kennworts müssen Sie sich bei Ihrem Azure-Portalkonto anmelden und **Gerätedetails** auswählen.

Microsoft bereitet dann Ihr Gerät vor und versendet es über einen regionalen Kurierdienst. Sobald das Gerät versandt wurde, erhalten Sie eine Nachverfolgungsnummer. Das Portal zeigt die Bestellung im Zustand **Versandt** an.

![Data Box-Exportauftrag wurde versandt](media/data-box-deploy-export-ordered/azure-data-box-export-16.png)

Wenn Sie den selbstverwalteten Versand ausgewählt haben, erhalten Sie eine E-Mail-Benachrichtigung mit den nächsten Schritten, wenn das Gerät aus dem Rechenzentrum abgeholt werden kann. Weitere Informationen zum selbstverwalteten Versand finden Sie unter [Verwenden des selbstverwalteten Versands für Azure Data Box im Azure-Portal](data-box-portal-customer-managed-shipping.md).

![Selbstverwalteter Versand: bereit für Abholung](media/data-box-deploy-export-ordered/azure-data-box-export-17.png)

## <a name="cancel-the-order"></a>Stornieren der Bestellung

Um die Bestellung zu stornieren, navigieren Sie im Azure-Portal zu **Übersicht** und wählen dann auf der Befehlsleiste die Option **Stornieren** aus.

Solange ein Auftrag noch nicht verarbeitet wird, können Sie ihn jederzeit stornieren.

Um eine stornierte Bestellung zu löschen, navigieren Sie zu **Übersicht** und wählen dann auf der Befehlsleiste die Option **Löschen** aus.

## <a name="sample-xml-file"></a>Beispiel für eine XML-Datei

Der folgende XML-Code ist ein Beispiel für Blobnamen, Blobpräfixe und Azure-Dateien im XML-Format, das im Exportauftrag verwendet wird, wenn Sie die Option **XML-Datei verwenden** auswählen:

```xml
<?xml version="1.0" encoding="utf-8"?>
   <!-- BlobList/prefix/Container list for Blob storage for export  -->
   <BlobList>
      <BlobPath>/8tbpageblob/8tbpageblob/8tbpageblob</BlobPath>
      <BlobPathPrefix>/blockblob4dot75tbdata/</BlobPathPrefix>
      <BlobPathPrefix>/1tbfilepageblob</BlobPathPrefix>
      <BlobPathPrefix>/1tbfile/</BlobPathPrefix>
      <BlobPathPrefix>/8mbfiles/</BlobPathPrefix>
      <BlobPathPrefix>/64mbfiles/</BlobPathPrefix>
   </BlobList>
   <!-- FileList/prefix/Share list for Azure File storage for export  -->
   <AzureFileList>
      <FilePathPrefix>/64mbfiles/</FilePathPrefix>
      <FilePathPrefix>/4mbfiles/prefix2/subprefix</FilePathPrefix>
      <FilePathPrefix>/1tbfile/prefix</FilePathPrefix>
   </AzureFileList>
```

Einige wichtige Punkte in Bezug auf XML-Dateien:

* Bei XML-Tags muss die Groß-/Kleinschreibung beachtet werden, und sie müssen genau dem obigen Beispiel entsprechen.
* Öffnende und schließende Tags müssen einander entsprechen.
* Falsche XML-Tags oder Formatierungen führen möglicherweise zu Datenexportfehlern.
* Wenn Blob- und/oder Dateipräfixe ungültig sind, werden keine Daten exportiert.

### <a name="examples-of-valid-blob-paths"></a>Beispiele für gültige Blobpfade

Die folgende Tabelle enthält Beispiele für gültige Blob-Pfade:

   | Auswahl | Blob-Pfad | BESCHREIBUNG |
   | --- | --- | --- |
   | Beginnt mit |/ |Exportiert alle Blobs im Speicherkonto |
   | Beginnt mit |/$root/ |Exportiert alle Blobs im Stammcontainer |
   | Beginnt mit |/containers |Exportiert alle Blobs in allen Containern, die mit dem Präfix **containers** beginnen |
   | Beginnt mit |/container-name/ |Exportiert alle Blobs im Container **container-name** |
   | Beginnt mit |/container-name/prefix |Exportiert alle Blobs im Container **container-name**, die mit dem Präfix **prefix** beginnen |
   | Gleich |$root/logo.bmp |Exportiert das Blob **logo.bmp** im Stammcontainer |
   | Gleich |8tbpageblob/mydata.txt |Exportiert Blob **mydata.txt** in Container **8tbpageblob** |

## <a name="sample-log-files"></a>Beispiel für Protokolldateien

Dieser Abschnitt enthält Beispiele für Protokolldateien, die während des Exports generiert werden. Die Fehlerprotokolle werden automatisch generiert. Zum Generieren der ausführlichen Protokolldatei müssen Sie beim Konfigurieren des Exportauftrags **Ausführliches Protokoll einschließen** im Azure-Portal einschließen.
Weitere Informationen zum Kopieren und zu ausführlichen Protokollen finden Sie unter [Protokolle kopieren](data-box-deploy-export-copy-data.md#copy-data-from-data-box).

<!-- ### Verbose log

The following log files show examples of verbose logging when you select **Include verbose log**:

```xml
<File CloudFormat="BlockBlob" Path="validblobdata/test1.2.3.4" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/helloEndWithDot..txt" Size="11" crc64="7320094093915972193"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test..txt" Size="12" crc64="17906086011702236012"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test1" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/test1.2.3" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/.......txt" Size="11" crc64="7320094093915972193"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/copylogb08fa3095564421bb550d775fff143ed====..txt" Size="53638" crc64="1147139997367113454"></File>
<File CloudFormat="BlockBlob" Path="validblobdata/testmaxChars-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-123456790-12345679" Size="1024" crc64="7573843669953104266"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file0" Size="0" crc64="0"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file1" Size="0" crc64="0"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file4096_000001" Size="4096" crc64="16969371397892565512"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file4096_000000" Size="4096" crc64="16969371397892565512"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/64KB-Seed10.dat" Size="65536" crc64="10746682179555216785"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/LiveSiteReport_Oct.xlsx" Size="7028" crc64="6103506546789189963"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/NE_Oct_GeoReport.xlsx" Size="103197" crc64="13305485882546035852"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/64KB-Seed1.dat" Size="65536" crc64="3140622834011462581"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/1mbfiles-0-0" Size="1048576" crc64="16086591317856295272"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file524288_000001" Size="524288" crc64="8908547729214703832"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/4mbfiles-0-0" Size="4194304" crc64="1339017920798612765"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/file524288_000000" Size="524288" crc64="8908547729214703832"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/8mbfiles-0-1" Size="8388608" crc64="3963298606737216548"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/1mbfiles-0-1" Size="1048576" crc64="11061759121415905887"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/XLS-10MB.xls" Size="1199104" crc64="2218419493992437463"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/8mbfiles-0-0" Size="8388608" crc64="1072783424245035917"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/4mbfiles-0-1" Size="4194304" crc64="9991307204216370812"></File>
<File CloudFormat="BlockBlob" Path="export-ut-container/VL_Piracy_Negtive10_TPNameAndGCS.xlsx" Size="12398699" crc64="13526033021067702820"></File>
```

### Copy logs

For more information regarding copy logs, see [Copy logs](data-box-deploy-export-copy-data.md#copy-data-from-data-box). -->

<!-- The following xml shows an example of the copy log when the export is successful:

```xml
<CopyLog Summary="Summary">
  <Status>Succeeded</Status>
    <TotalFiles_Blobs>27</TotalFiles_Blobs>
    <FilesErrored>0</FilesErrored>
</CopyLog>
```

For more information regarding copy logs, see [Copy logs](data-box-deploy-export-copy-data.md#copy-data-from-data-box).

The following xml shows an example of the copy log when the export has errors:

```xml
<ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/wastorage.v140.3.0.2.nupkg">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/xunit.console.Primary_2020-05-07_03-54-42-PM_27444.hcsml">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><ErroredEntity CloudFormat="AppendBlob" Path="export-ut-appendblob/xunit.console.Primary_2020-05-07_03-54-42-PM_27444 (1).hcsml">
  <Category>UploadErrorCloudHttp</Category>
  <ErrorCode>400</ErrorCode>
  <ErrorMessage>UnsupportBlobType</ErrorMessage>
  <Type>File</Type>
</ErroredEntity><CopyLog Summary="Summary">
  <Status>Failed</Status>
  <TotalFiles_Blobs>4</TotalFiles_Blobs>
  <FilesErrored>3</FilesErrored>
</CopyLog>
``` -->

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
>
> * Voraussetzungen für den Export
> * Bestellen einer Data Box für den Export
> * Nachverfolgen des Exportauftrags
> * Stornieren des Exportauftrags

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Data Box einrichten.

> [!div class="nextstepaction"]
> [Einrichten der Azure Data Box](./data-box-deploy-set-up.md)
