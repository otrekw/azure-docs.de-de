---
title: Bereitstellen des ABAP-Funktionsmoduls für die Metadatenextraktion für die SAP R3-Brückenfamilie in Azure Purview
description: In diesem Artikel werden die Schritte zum Bereitstellen des ABAP-Funktionsmoduls in SAP Server beschrieben.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/13/2020
ms.openlocfilehash: 911238efafb948b304455cf75cc4ec2c3c605c76
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102042055"
---
# <a name="deploy-the-metadata-extraction-abap-function-module-for-the-sap-r3-family-of-bridges"></a>Bereitstellen des ABAP-Funktionsmoduls für die Metadatenextraktion für die SAP R3-Brückenfamilie 
In diesem Artikel werden die Schritte zum Bereitstellen des ABAP-Funktionsmoduls in SAP Server beschrieben.
## <a name="overview"></a>Übersicht 

Die SAP Business Suite 4 HANA (S/4HANA)-, ECC- und R/3 ERP-Brücke können zum Extrahieren von Metadaten vom SAP-Server verwendet werden. Hierzu wird das ABAP-Funktionsmodul auf dem SAP-Server platziert. Auf dieses Funktionsmodul kann die Brücke remote zugreifen, um die im SAP-Server enthaltenen Metadaten abzufragen und (als Textdatei) herunterzuladen.
Bei der Ausführung führt die Bücke eine der folgenden Aktionen durch:

1.  Metadaten werden aus einer vorhandenen Datei importiert, die bereits lokal aus einer vorherigen Brückenausführung heruntergeladen wurde.

2.  Die ABAP-Modul-API wird aufgerufen, es wird auf den Download gewartet, und dann werden Metadaten aus dieser Datei importiert.

In diesem Dokument werden die erforderlichen Schritte zum Bereitstellen dieses Moduls ausführlich beschrieben.

> [!Note] 
>Die folgenden Anweisungen wurden basierend auf der SAP GUI v. 7.2 zusammengestellt.

## <a name="deployment-of-the-module"></a>Bereitstellung des Moduls 

### <a name="create-a-package"></a>Erstellen eines Pakets 

Dieser Schritt ist optional, und ein vorhandenes Paket kann verwendet werden.

1.  Melden Sie sich beim SAP S/4HANA- oder SAP ECC-Server an, und öffnen Sie den \"Object Navigator\" (SE80-Transaktion).

2.  Wählen Sie \"Package\" (Paket) in der Liste aus, geben Sie einen Namen für das neue Paket ein (z. B. Z\_MITI), und klicken Sie dann auf die Schaltfläche „Display“ (Anzeige).

3.  Klicken Sie im Fenster „Create Package“ (Paket erstellen) auf „Yes“ (Ja). Daraufhin wird ein Fenster \"Package Builder: Create Package\" (Paketgenerator: Paket erstellen) geöffnet. Geben Sie einen Wert in das Feld „Short Description“ (Kurzbeschreibung) ein, und klicken Sie auf das Symbol \"Continue\" (Weiter).

4.  Klicken Sie im Fenster „Prompt for local Workbench request“ (Eingabeaufforderung für lokale Workbenchanforderung) auf „Own Requests“ (Eigene Anforderungen). Wählen Sie die Anforderung „Development“ (Entwicklung) aus.

### <a name="create-a-function-group"></a>Erstellen einer Funktionsgruppe 

Wählen Sie im Object Navigator in der Liste \"Function Group\" (Funktionsgruppe) aus, und geben Sie deren Namen unten in das Eingabefeld ein (z. B. Z\_MITI\_FGROUP). Klicken Sie auf das Ansichtssymbol.

1.  Klicken Sie im Fenster \"Create Object\" (Objekt erstellen) auf „Yes“ (Ja), um eine neue Funktionsgruppe zu erstellen.

2.  Geben Sie im Feld \"Short text\" (Kurztext) eine entsprechende Beschreibung ein, und klicken Sie auf die Schaltfläche \"Save\" (Speichern).

3.  Wählen Sie ein Paket aus, das im vorherigen Schritt \"Erstellen eines Pakets\" vorbereitet wurde, und klicken Sie auf das Symbol \"Save\" (Speichern).

4.  Bestätigen Sie eine Anforderung durch Klicken auf das Symbol \"Weiter\".

5.  Aktivieren Sie diese Funktionsgruppe.

### <a name="create-the-abap-function-module"></a>Erstellen des ABAP-Funktionsmoduls 

Nachdem Sie die Funktionsgruppe erstellt und ausgewählt haben, klicken Sie im Repositorybrowser mit der rechten Maustaste auf deren Namen, und wählen Sie \"Create=\> Function Module\" (Erstellen=>Funktionsmodul) aus.

Geben Sie \"Z\_MITI\_DOWNLOAD\" in das Feld \"Function Module\" (Funktionsmodul) ein, und geben Sie in \"Short text\" (Kurztext) eine entsprechende Beschreibung ein.

Wenn das Modul erstellt wurde, geben Sie die folgenden Informationen an:

1.  Navigieren Sie zur Registerkarte \"Attributes\" (Attribute).

2.  Wählen Sie „Processing Type = Remote-Enabled Function Module“ (Verarbeitungstyp = Remotefähiges Funktionsmodul) aus.

    :::image type="content" source="media/abap-functions-deployment-guide/processing-type.png" alt-text="Optionen für die Quellenregistrierung" border="true":::

3.  Navigieren Sie zur Registerkarte \"Source code\" (Quellcode). Es gibt zwei Möglichkeiten, Code für die Funktion bereitzustellen:

    a.  Laden Sie im Hauptmenü die Textdatei [Z\_MITI\_DOWNLOAD](https://github.com/Azure/Purview-Samples/tree/master/connectors/sap) hoch, indem Sie „Utilities=\>More Utilities=\>Upload/Download=\>Upload“ (Hilfsprogramme=>Weitere Hilfsprogramme=>Upload/Download=>Upload) auswählen.

    b.  Öffnen Sie alternativ die Datei, kopieren Sie den Inhalt, und fügen Sie ihn in den Bereich \"Source code\" (Quellcode) ein.

4.  Navigieren Sie zur Registerkarte \"Import\" (Importieren), und erstellen Sie die folgenden Parameter:

    a.  P\_AREA TYPE DD02L-TABNAME (Optional = True)

    b.  *P\_LOCAL\_PATH TYPE STRING* (Optional = True)

    c.  *P\_LANGUAGE TYPE L001TAB-DATA DEFAULT \'E\'*

    d.  *ROWSKIPS TYPE SO\_INT DEFAULT 0*

    e.  *ROWCOUNT TYPE SO\_INT DEFAULT 0*

    > [!Note]
    > Wählen Sie für alle \"Pass Value\" (Wert übergeben) aus.

    :::image type="content" source="media/abap-functions-deployment-guide/import.png" alt-text="Optionen für die Quellenregistrierung" border="true":::

5.  Navigieren Sie zur Registerkarte „Tables“ (Tabellen), und definieren Sie Folgendes:

    *EXPORT\_TABLE LIKE TAB512*

    :::image type="content" source="media/abap-functions-deployment-guide/export-table.png" alt-text="Optionen für die Quellenregistrierung" border="true":::

6.  Navigieren Sie zur Registerkarte \"Exceptions\" (Ausnahmen), und definieren Sie die folgende Ausnahme:

    *E\_EXP\_GUI\_DOWNLOADFAILED*

    :::image type="content" source="media/abap-functions-deployment-guide/exceptions.png" alt-text="Optionen für die Quellenregistrierung" border="true":::

7.  Speichern Sie die Funktion (drücken Sie STRG+S, oder wählen Sie im Hauptmenü „Function Module=\>Save“ (Funktionsmodul=>Speichern) aus).

8.  Klicken Sie in der Symbolleiste auf das Symbol \"Activate\" (Aktivieren) (STRG+F3) und im Dialogfeld auf die Schaltfläche \"Continue\" (Weiter). Wenn Sie dazu aufgefordert werden, sollten Sie auswählen, dass die generierten einzuschließenden Elemente zusammen mit dem Hauptfunktionsmodul aktiviert werden sollen.

### <a name="testing-the-function"></a>Testen der Funktion 

Wenn alle vorherigen Schritte abgeschlossen sind, führen Sie die folgenden Schritte aus, um die Funktion zu testen:

1.  Öffnen Sie das Z\_MITI\_DOWNLOAD-Funktionsmodul.

2.  Wählen Sie im Hauptmenü \"Function Module=\>Test=\>Test Function Module\" (Funktionsmodul=>Test=>Funktionsmodul testen) aus, oder drücken Sie F8.

3.  Geben Sie einen Pfad zum Ordner im lokalen Dateisystem in Parameter P\_LOCAL\_PATH ein, und klicken Sie auf der Symbolleiste auf das Symbol \"Execute\" (Ausführen), oder drücken Sie F8.

4.  Geben Sie den Namen des relevanten Bereichs in das Feld P\_AREA ein, wenn eine Datei mit Metadaten heruntergeladen oder aktualisiert werden muss. Nach Ausführung der Funktion muss der im Parameter P\_LOCAL\_PATH angegebene Ordner mehrere Dateien mit Metadaten enthalten. Die Namen der Dateien imitieren Bereiche, die im Feld P\_AREA angegeben werden können.

Die Ausführung der Funktion und das Herunterladen der Metadaten erfolgen wesentlich schneller, wenn die Funktion auf dem Computer gestartet wird, der über eine schnelle Netzwerkverbindung mit dem SAP S/4HANA- oder ECC-Server verfügt.

## <a name="next-steps"></a>Nächste Schritte

- [Registrieren und Überprüfen einer SAP ECC-Quelle](register-scan-sapecc-source.md)
- [Registrieren und Überprüfen einer SAP S/4HANA-Quelle](register-scan-saps4hana-source.md)