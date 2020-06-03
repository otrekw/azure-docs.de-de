---
title: 'Verwenden von SQL-Datenbank-DAC-Paketen: Azure SQL Edge (Vorschau)'
description: Informationen zur Verwendung von DACPAC-Dateien in Azure SQL Edge (Vorschau)
keywords: SQL Edge, SqlPackage
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 0ddd1544c6a51ff1e2f98a28e40d9eb2ee0b47c7
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233283"
---
# <a name="sql-database-dac-packages-in-sql-edge"></a>SQL-Datenbank-DAC-Pakete in SQL Edge

Azure SQL Edge (Vorschau) ist eine optimierte relationale Datenbank-Engine für IoT- und Edge-Bereitstellungen. Sie basiert auf den aktuellen Versionen der Microsoft SQL Server-Datenbank-Engine, die branchenführende Funktionen für Leistung, Sicherheit und Abfrageverarbeitung bietet. Zusammen mit den branchenführenden Funktionen für die Verwaltung relationaler Datenbanken von SQL Server bietet Azure SQL Edge integrierte Streamingfunktionen für Echtzeitanalysen und komplexe Ereignisverarbeitung.

Azure SQL Edge bietet auch eine native Implementierung von „SqlPackage.exe“, mit der Sie während der Bereitstellung von SQL Edge ein [SQL-Datenbank-DAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications)-Paket bereitstellen können. SQL-Datenbank-DACPACs können in SQL Edge mithilfe des SqlPackage-Parameters bereitgestellt werden, der über die Option `module twin's desired properties` des SQL Edge-Moduls verfügbar gemacht wird:

```json
{
    "properties.desired":
    {
        "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
        "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
    }
}
```

|Feld | BESCHREIBUNG |
|------|-------------|
| SqlPackage | Azure Blob Storage-URI für die *.ZIP-Datei mit dem SQL-Datenbank-DAC-Paket.
| ASAJobInfo | Azure Blob Storage-URI für den ASA Edge-Auftrag.

## <a name="use-a-sql-database-dac-package-with-sql-edge"></a>Verwenden eines SQL-Datenbank-DAC-Pakets mit SQL Edge

Wenn Sie ein SQL-Datenbank-DAC-Paket (*.dacpac) mit SQL Edge verwenden möchten, führen Sie die folgenden Schritte aus:

1. Erstellen oder extrahieren Sie ein SQL-Datenbank-DAC-Paket. Informationen zum Generieren eines DAC-Pakets für eine vorhandene SQL Server-Datenbank finden Sie unter [Extrahieren einer DAC aus einer Datenbank](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/).

2. Komprimieren Sie die *.DACPAC-Datei, und laden Sie sie in ein Azure Blob Storage-Konto hoch. Weitere Informationen zum Hochladen von Dateien in Azure Blob Storage finden Sie unter [Hochladen, Herunterladen und Auflisten von Blobs über das Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

3. Generieren Sie eine Shared Access Signature (SAS) für die ZIP-Datei über das Azure-Portal. Weitere Informationen finden Sie unter [Delegieren des Zugriffs mit Shared Access Signatures (SAS)](../storage/common/storage-sas-overview.md).

4. Aktualisieren Sie die Konfiguration des SQL Edge-Moduls, um den SAS-URI für das DAC-Paket einzubeziehen. Führen Sie die folgenden Schritte zum Aktualisieren des SQL Edge-Moduls aus:

    1. Navigieren Sie im Azure-Portal zu Ihrer IoT Hub-Bereitstellung.

    2. Wählen Sie im linken Bereich die Option **IoT Edge** aus.

    3. Suchen Sie auf der Seite **IoT Edge** nach der IoT Edge-Instanz, in der das SQL Edge-Modul bereitgestellt wurde, und wählen Sie sie aus.

    4. Wählen Sie auf der Geräteseite **IoT Edge-Gerät** die Option **Modul festlegen** aus.

    5. Wählen Sie auf der Seite **Module festlegen** für das SQL Edge-Modul **Konfigurieren** aus.

    6. Wählen Sie im Bereich **Benutzerdefinierte IoT Edge-Module** die Option **Gewünschte Eigenschaften für Modulzwilling festlegen** aus. Aktualisieren Sie die gewünschten Eigenschaften, um den URI für die Option `SQLPackage` einzubeziehen, wie im folgenden Beispiel gezeigt wird.

        > [!NOTE]
        > Der SAS-URI im nachstehenden JSON-Code ist nur ein Beispiel. Ersetzen Sie diesen URI durch den tatsächlichen URI aus Ihrer Bereitstellung.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "<<<SAS URL for the *.zip file containing the dacpac",
                }
            }
        ```

    7. Wählen Sie **Speichern** aus.

    8. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** aus.

    9. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** und dann **Senden** aus.

5. Nach dem Modulupdate wird die DAC-Paketdatei heruntergeladen, entzippt und für die SQL Edge-Instanz bereitgestellt.

Bei jedem Neustart des Azure SQL Edge-Containers wird das Dateipaket vom Typ „*.dacpac“ heruntergeladen und auf Änderungen überprüft. Wenn eine neue Version der DACPAC-Datei gefunden wird, werden die Änderungen in der Datenbank in SQL Edge bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von SQL Edge über das Azure-Portal](deploy-portal.md)
- [Streamen von Daten](stream-data.md)
- [Machine Learning und KI mit ONNX in SQL Edge (Vorschau)](onnx-overview.md)
