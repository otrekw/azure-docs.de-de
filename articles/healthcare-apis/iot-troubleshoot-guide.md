---
title: 'Azure IoT-Konnektor für FHIR (Vorschauversion): Leitfaden zur Problembehandlung und Anleitungen'
description: Informationen zur Problembehandlung bei häufigen Fehlermeldungen zu Azure IoT-Konnektor für FHIR (Vorschauversion), zu den damit verbundenen Bedingungen und zum Kopieren von Zuordnungsdateien
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: troubleshooting
ms.date: 09/16/2020
ms.author: jasteppe
ms.openlocfilehash: 64056ef2f63331686553c52040af9e10ee0ac468
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90982992"
---
# <a name="azure-iot-connector-for-fhir-preview-troubleshooting-guide"></a>Azure IoT-Konnektor für FHIR (Vorschauversion): Leitfaden zur Problembehandlung

Dieser Artikel enthält Schritte zur Problembehandlung bei häufigen Fehlermeldungen und Bedingungen zu Azure IoT-Konnektor für FHIR*.  

Außerdem erfahren Sie, wie Sie Kopien der JSON-Datei für Konvertierungszuordnung zu Azure IoT-Konnektor für FHIR erstellen (z. B.: „Gerät“ und „FHIR“).  

Sie können die Kopien der JSON-Datei für Konvertierungszuordnung zur Bearbeitung und Archivierung außerhalb des Azure-Portals verwenden.  

> [!TIP]
> Wenn Sie ein [Azure Technical Support](https://azure.microsoft.com/support/create-ticket/)-Ticket zu Azure IoT-Konnektor für FHIR öffnen, müssen Sie unbedingt Kopien Ihrer JSON-Datei für Konvertierungszuordnung als Hilfe bei der Problembehandlung einbeziehen.

## <a name="device-and-fhir-conversion-mapping-json-template-validations-for-azure-iot-connector-for-fhir-preview"></a>JSON-Vorlagenüberprüfungen für die Konvertierungszuordnung vom Typ „Gerät“ und „FHIR“ für Azure IoT-Konnektor für FHIR (Vorschau)
In diesem Abschnitt erfahren Sie mehr über den Überprüfungsprozess, den Azure IoT-Konnektor für FHIR durchführt, um die JSON-Vorlagen für die Konvertierungszuordnung vom Typ „Gerät“ und „FHIR“ zu überprüfen, bevor sie zur Verwendung gespeichert werden können.  Diese Elemente werden im JSON-Code für die Konvertierungszuordnung vom Typ „Gerät“ und „FHIR“ benötigt.

**Gerätezuordnung**

|Element|Erforderlich|
|:-------|:------|
|TypName|True|
|TypeMatchExpression|True|
|DeviceIdExpression|True|
|TimestampExpression|True|
|Values[].ValueName|True|
|Values[].ValueExpression|True|

> [!NOTE]
> Values[].ValueName und Values[].ValueExpression
>
> Diese Elemente sind nur erforderlich, wenn sich ein Werteintrag im Array befindet – es ist zulässig, dass keine Werte zugeordnet sind. Dies wird verwendet, wenn die gesendete Telemetrie ein Ereignis ist. Beispiel: Wenn ein tragbares IoMT-Gerät angelegt oder entfernt wird. Die Elemente haben keine Werte außer einem Namen, der von Azure IoT-Konnektor für FHIR verglichen und ausgegeben wird. Bei der FHIR-Konvertierung wird es von Azure IoT-Konnektor für FHIR einem codierbaren Konzept zugeordnet, das auf dem semantischen Typ basiert – es werden keine tatsächlichen Werte aufgefüllt.

**FHIR-Zuordnung**

|Element|Erforderlich|
|:------|:-------|
|TypName|True|

> [!NOTE]
> Dies ist das einzige erforderliche FHIR-Zuordnungselement, das zu diesem Zeitpunkt überprüft wurde.

## <a name="error-messages-and-fixes-for-azure-iot-connector-for-fhir-preview"></a>Fehlermeldungen und Korrekturen bei Azure IoT-Konnektor für FHIR (Vorschauversion)

|`Message`|Angezeigt wird|Bedingung|Fix| 
|-------|---------|---------|---|
|Invalid mapping name, mapping name should be device or FHIR. (Ungültiger Zuordnungsname; Zuordnungsname muss „Gerät“ oder „FHIR“ sein).|API|Der angegebene Zuordnungstyp ist kein Gerät oder FHIR.|Verwenden Sie einen der beiden unterstützten Zuordnungstypen (z. B.: „Gerät“ oder „FHIR“).|
|Fehler bei der Überprüfung. Erforderliche Informationen fehlen oder sind ungültig.|API und Azure-Portal|Beim Versuch, eine Konvertierungszuordnung zu speichern, fehlen erforderliche Informationen oder Elemente.|Fügen Sie die fehlenden Informationen oder Elemente hinzu, und versuchen Sie erneut, die Konvertierungszuordnung zu speichern.|
|Regenerate key parameters not defined. (Erneutes Generieren von Schlüsselparametern wurde nicht definiert.)|API|Anforderung zum erneuten Generieren des Schlüssels.|Beziehen Sie die Parameter in die Anforderung zur erneuten Schlüsselgenerierung ein.|
|Reached the maximum number of IoT Connector instances that can be provisioned in this subscription. (Die maximale Anzahl von IoT-Konnektor-Instanzen, die in diesem Abonnement bereitgestellt werden können, wurde erreicht.)|API und Azure-Portal|Das Abonnementkontingent für Azure IoT-Konnektor für FHIR wurde erreicht (der Standardwert ist „2“ pro Abonnement).|Löschen Sie eine der vorhandenen Instanzen von Azure IoT-Konnektor für FHIR.  Verwenden Sie ein anderes Abonnement, bei dem das Abonnementkontingent noch nicht erreicht wurde.  Fordern Sie eine Erhöhung des Abonnementkontingents an.|
|Move resource is not supported for IoT Connector enabled Azure API for FHIR resource. (Ressourcenverschiebung wird bei IoT-Konnektor-fähiger Azure-API für FHIR-Ressource nicht unterstützt).|API und Azure-Portal|Es wurde versucht, einen Verschiebevorgang bei einer Azure-API für FHIR-Ressource durchzuführen, die mindestens eine Instanz von Azure IoT-Konnektor für FHIR enthält.|Löschen Sie die vorhandene(n) Instanz(en) von Azure IoT-Konnektor für FHIR, um den Verschiebevorgang ausführen zu können.|
|IoT Connector not provisioned. (IoT-Konnektor nicht bereitgestellt.)|API|Es wurde versucht, untergeordnete Dienste (Verbindungen und Zuordnungen) zu nutzen, doch das übergeordnete Element (Azure IoT-Konnektor für FHIR) war nicht bereitgestellt.|Stellen Sie einen Azure IoT-Konnektor für FHIR bereit.|
|Die Anforderung wird nicht unterstützt.|API|Eine bestimmte API-Anforderung wird nicht unterstützt.|Verwenden Sie die richtige API-Anforderung.|
|Konto nicht vorhanden.|API|Beim Versuch zum Hinzufügen eines Azure IoT-Konnektors für FHIR war die Azure-API für FHIR-Ressource nicht vorhanden.|Erstellen Sie die Azure-API für FHIR-Ressource, und wiederholen Sie dann den Vorgang.|
|Azure API for FHIR resource FHIR version is not supported for IoT Connector. (Die FHIR-Version der Azure-API für FHIR-Ressource wird bei IoT-Konnektor nicht unterstützt.)|API|Es wurde versucht, einen Azure IoT-Konnektor für FHIR mit einer inkompatiblen Version der Azure-API für FHIR-Ressource zu verwenden.|Erstellen Sie eine neue Azure-API für FHIR-Ressource (Version R4), oder verwenden Sie eine vorhandene Azure-API für FHIR-Ressource (Version R4).

##  <a name="why-is-my-azure-iot-connector-for-fhir-preview-data-not-showing-up-in-azure-api-for-fhir"></a>Warum werden meine Daten zu Azure IoT-Konnektor für FHIR (Vorschauversion) in der Azure-API für FHIR nicht angezeigt?

|Mögliche Probleme|Fehlerbehebungen|
|----------------|-----|
|Die Daten werden noch verarbeitet.|Daten werden an die Azure-API für FHIR in Batches (ca. alle 15 Minuten) ausgegeben.  Es ist möglich, dass die Daten noch verarbeitet werden und zusätzliche Zeit erforderlich ist, damit die Daten in der Azure-API für FHIR persistent gespeichert werden.|
|JSON-Datei für Konvertierungszuordnung des Typs „Gerät“ wurde nicht konfiguriert.|Konfigurieren und speichern Sie die entsprechende JSON-Datei für Konvertierungszuordnung des Typs „Gerät“.|
|JSON-Datei für Konvertierungszuordnung des Typs „FHIR“ wurde nicht konfiguriert.|Konfigurieren und speichern Sie die entsprechende JSON-Datei für Konvertierungszuordnung des Typs „FHIR“.|
|Die Gerätenachricht enthält keinen erwarteten Ausdruck, der in der Gerätezuordnung definiert wurde.|Überprüfen Sie, ob die in der Gerätezuordnung definierten „JsonPath“-Ausdrücke mit den in der Gerätenachricht definierten Token übereinstimmen.|
|In der Azure-API für FHIR wurde keine Patientenressource erstellt (Lösungstyp: Nur Suche)*.|Erstellen Sie in der Azure-API für FHIR eine gültige Geräteressource. Sorgen Sie dafür, dass die Geräteressource einen Bezeichner enthält, der mit der in der eingehenden Nachricht angegebenen Geräte-ID übereinstimmt.|
|In der Azure-API für FHIR wurde keine Patientenressource erstellt (Lösungstyp: Nur Suche)*.|Erstellen Sie in der Azure-API für FHIR eine gültige Patientenressource.|
|Der Verweis „Device.patient“ wurde nicht festgelegt, oder der Verweis ist ungültig (Lösungstyp: Nur Suche)*.|Stellen Sie sicher, dass die Geräteressource einen gültigen [Verweis](https://www.hl7.org/fhir/device-definitions.html#Device.patient) auf eine Patientenressource enthält.| 

*Referenz [Schnellstart: Bereitstellen von Azure IoT-Konnektor für FHIR (Vorschauversion) mithilfe des Azure-Portals](iot-fhir-portal-quickstart.md#create-new-azure-iot-connector-for-fhir-preview). Sie finden darin eine Funktionsbeschreibung der Lösungstypen zu Azure IoT-Konnektor (Beispiel: Suche oder Erstellen).

## <a name="creating-copies-of-the-azure-iot-connector-for-fhir-preview-conversion-mapping-json"></a>Erstellen von Kopien der JSON-Datei für Konvertierungszuordnung zu Azure IoT-Konnektor für FHIR (Vorschauversion)
Das Kopieren von Azure IoT-Konnektor für FHIR-Zuordnungsdateien kann zur Bearbeitung und Archivierung außerhalb der Azure-Portalwebsite nützlich sein.

Die Kopien der Zuordnungsdatei sollten beim Öffnen eines Supporttickets für den Azure Technical Support als Hilfe bei der Problembehandlung bereitgestellt werden.

> [!NOTE]
> JSON ist zurzeit das einzige unterstützte Format für Geräte- und FHIR-Zuordnungsdateien.

> [!TIP]
> Informieren Sie sich ausführlicher zum Thema [JSON-Datei für Geräte- und FHIR-Konvertierungszuordnung](https://docs.microsoft.com/azure/healthcare-apis/iot-mapping-templates) zu Azure IoT-Konnektor für FHIR.

1. Wählen Sie auf der unteren linken Seite des Ressourcendashboards für Azure-API für FHIR im Abschnitt **„Add-Ins“** die Option **„IoT-Konnektor (Vorschauversion)“** aus.

   :::image type="content" source="media/iot-troubleshoot/map-files-main-with-box.png" alt-text="IoT-Konnektor1" lightbox="media/iot-troubleshoot/map-files-main-with-box.png":::

2. Wählen Sie den **„Konnektor“** aus, aus dem Sie die JSON-Datei für Konvertierungszuordnung kopieren werden.

   :::image type="content" source="media/iot-troubleshoot/map-files-select-connector-with-box.png" alt-text="IoT-Konnektor1" lightbox="media/iot-troubleshoot/map-files-select-connector-with-box.png":::

> [!NOTE]
> Dieser Prozess kann auch zum Kopieren und Speichern der Inhalte der JSON-Datei für **„FHIR-Zuordnung konfigurieren“** verwendet werden.

3. Wählen Sie **„Gerätezuordnung konfigurieren“** aus.

    :::image type="content" source="media/iot-troubleshoot/map-files-select-device-with-box.png" alt-text="IoT-Konnektor1" lightbox="media/iot-troubleshoot/map-files-select-device-with-box.png":::

4. Wählen Sie den Inhalt der JSON-Datei aus, und führen Sie einen Kopiervorgang aus (Beispiel: Drücken Sie STRG+C). 

   :::image type="content" source="media/iot-troubleshoot/map-files-select-device-json-with-box.png" alt-text="IoT-Konnektor1" lightbox="media/iot-troubleshoot/map-files-select-device-json-with-box.png":::

5. Führen Sie einen Einfügevorgang (Beispiel: STRG+C drücken) in eine neue Datei in einem Text-Editor (Beispiel: Visual Studio Code, Notepad) aus, und speichern Sie die Datei mit der Erweiterung „*.JSON“.

> [!TIP]
> Wenn Sie ein [Azure Technical Support](https://azure.microsoft.com/support/create-ticket/)-Ticket zu Azure IoT-Konnektor für FHIR öffnen, müssen Sie unbedingt Kopien Ihrer JSON-Datei für Konvertierungszuordnung als Hilfe bei der Problembehandlung einbeziehen.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie häufig gestellte Fragen zu Azure IoT-Konnektor für FHIR.

>[!div class="nextstepaction"]
>[Azure IoT-Konnektor für FHIR – Häufig gestellte Fragen](fhir-faq.md#azure-iot-connector-for-fhir-preview)

*Im Azure-Portal wird Azure IoT-Konnektor für FHIR als IoT-Konnektor (Vorschauversion) bezeichnet.

FHIR ist ein eingetragenes Markenzeichen von HL7 und wird mit Erlaubnis von HL7 verwendet.
