---
title: Einrichten von IoT Edge-Modulen in Azure SQL Edge
description: Im zweiten Teil dieses dreiteiligen Azure SQL Edge-Tutorials zum Vorhersagen von Eisenerzverunreinigungen richten Sie IoT Edge-Module und -Verbindungen ein.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: a4087ef56712e098443009bd0457029394ea7b51
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235038"
---
# <a name="set-up-iot-edge-modules-and-connections"></a>Einrichten von IoT Edge-Modulen und -Verbindungen

Im zweiten Teil dieses dreiteiligen Tutorials zum Vorhersagen von Eisenerzverunreinigungen in Azure SQL Edge richten Sie die folgenden IoT Edge Module ein:

- Azure SQL Edge
- Datengenerator „IoT Edge-Modul“

## <a name="create-azure-stream-analytics-module"></a>Erstellen eines Azure Stream Analytics-Moduls

Erstellen Sie ein Azure Stream Analytics-Modul, das in diesem Tutorial verwendet wird. Weitere Informationen zur Verwendung von Streamingaufträgen mit SQL Edge finden Sie unter [Verwenden von Streamingaufträgen mit SQL Edge](stream-analytics.md).

Nachdem der Azure Stream Analytics-Auftrag mit der als Edge festgelegten Hostingumgebung erstellt wurde, richten Sie die Eingaben und Ausgaben für das Tutorial ein.

1. Klicken Sie zum Erstellen der **Eingabe** auf **+ Datenstromeingabe hinzufügen**. Füllen Sie den Abschnitt „Details“ mit den folgenden Informationen aus:

   Feld|Wert
   -----|-----
   Ereignisserialisierungsformat|JSON
   Codieren|UTF-8
   Typ der Ereigniskomprimierung|Keine

2. Klicken Sie zum Erstellen der **Ausgabe** auf **+ Hinzufügen**, und wählen Sie „SQL-Datenbank“ aus. Füllen Sie den Abschnitt „Details“ mit den nachstehenden Informationen aus.

   > [!NOTE]
   > Das in diesem Abschnitt angegebene Kennwort muss bei der Bereitstellung des SQL Edge-Moduls im Abschnitt **„Bereitstellen des Azure SQL Edge-Moduls“** für das SQL Server-Systemadministratorkennwort angegeben werden.

   Feld|Wert
   -----|-----
   Datenbank|IronOreSilicaPrediction
   Servername|tcp:.,1433
   Username|sa
   Kennwort|Geben Sie ein starkes Kennwort an.
   Tabelle|IronOreMeasurements1

3. Navigieren Sie zum Abschnitt **Abfrage**, und richten Sie die Abfrage folgendermaßen ein:

   `SELECT * INTO <name_of_your_output_stream> FROM <name_of_your_input_stream>`
   
4. Wählen Sie unter **Konfigurieren** die Option **Veröffentlichen** und dann die Schaltfläche **Veröffentlichen** aus. Speichern Sie den SAS-URI zur Verwendung mit dem SQL Database Edge-Modul.

## <a name="specify-container-registry-credentials"></a>Angeben von Anmeldeinformationen für die Containerregistrierung

Die Anmeldeinformationen für die Containerregistrierungen zum Hosten von Modulimages müssen angegeben werden. Sie finden diese Informationen in der Containerregistrierung, die in Ihrer Ressourcengruppe erstellt wurde. Navigieren Sie zum Abschnitt **Zugriffsschlüssel**. Notieren Sie sich die folgenden Felder:

- Registrierungsname
- Anmeldeserver
- Username
- Kennwort

Geben Sie jetzt die Containeranmeldeinformationen im IoT Edge-Modul an.

1. Navigieren Sie zu dem IoT-Hub, der in Ihrer Ressourcengruppe erstellt wurde.

2. Klicken Sie im Abschnitt **IoT Edge** unter **Automatische Geräteverwaltung** auf **Geräte-ID**. Für dieses Tutorial lautet die ID `IronOrePredictionDevice`.

3. Wählen Sie den Abschnitt **Module festlegen** aus.

4. Geben Sie unter **Anmeldeinformationen für Containerregistrierung** die folgenden Werte ein:

   _Feld_|_Wert_
   -------|-------
   Name|Registrierungsname
   Adresse|Anmeldeserver
   Benutzername|Username
   Kennwort|Kennwort
  
## <a name="deploy-the-data-generator-module"></a>Bereitstellen des Datengeneratormoduls

1. Klicken Sie im Abschnitt **IoT Edge-Module** auf **+ HINZUFÜGEN**, und wählen Sie **IoT Edge-Modul** aus.

2. Geben Sie einen IoT Edge-Modulnamen und den Image-URI an.
   Der Image-URI ist in der Containerregistrierung in der Ressourcengruppe zu finden. Wählen Sie unter **Services** den Abschnitt **Repositorys** aus. Wählen Sie für dieses Tutorial das Repository `silicaprediction` aus. Wählen Sie das geeignete Tag aus. Der Image-URI hat folgendes Format:

   *anmeldeserver der containerregistrierung*/*repositoryname*:*tagname*

   Beispiel:

   ```
   ASEdemocontregistry.azurecr.io/silicaprediction:amd64
   ```

3. Klicken Sie auf **Hinzufügen**.

## <a name="deploy-the-azure-sql-edge-module"></a>Bereitstellen des Azure SQL Edge-Moduls

1. Stellen Sie das Azure SQL Edge-Modul bereit, indem Sie die Schritte unter [Bereitstellen von Azure SQL Edge (Vorschau)](https://docs.microsoft.com/azure/azure-sql-edge/deploy-portal) ausführen.

2. Geben Sie im Abschnitt **Route angeben** der Seite **Module festlegen** die Routen für das Modul zur IoT Edge Hub-Kommunikation wie folgt an. 

   ```
   FROM /messages/modules/<your_data_generator_module>/outputs/<your_output_stream_name> INTO
   BrokeredEndpoint("/modules/<your_azure_sql_edge_module>/inputs/<your_input_stream_name>")
   ```

   Beispiel:

   ```
   FROM /messages/modules/ASEDataGenerator/outputs/IronOreMeasures INTO BrokeredEndpoint("/modules/AzureSQLEdge/inputs/Input1")
   ```

3. Stellen Sie in den Einstellungen **Modulzwilling** sicher, dass „SQLPackage“ und „ASAJonInfo“ mit den relevanten SAS-URLs aktualisiert werden, die Sie zuvor im Tutorial gespeichert haben.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen eines ML-Modells in Azure SQL Edge mithilfe von ONNX](tutorial-run-ml-model-on-sql-edge.md)
