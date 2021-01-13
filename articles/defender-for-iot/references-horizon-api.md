---
title: Horizon-API
description: In dieser Anleitung werden häufig verwendete Horizon-Methoden beschrieben.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 11/19/2020
ms.topic: article
ms.service: azure
ms.openlocfilehash: 3266517b9ddc8fc7ac7b06a137286c05ea9a28fa
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97836029"
---
# <a name="horizon-api"></a>Horizon-API 

In dieser Anleitung werden häufig verwendete Horizon-Methoden beschrieben.

### <a name="getting-more-information"></a>Weitere Informationen

Weitere Informationen zum Arbeiten mit Horizon und der CyberX-Plattform finden Sie an folgenden Stellen:

- Informationen zum Horizon ODE-SDK (Open Development Environment) erhalten Sie von Ihrem CyberX-Vertriebsmitarbeiter.
- Informationen zu Support und Problembehandlung erhalten Sie bei <support@cyberx-labs.com>.
- Wenn Sie über die CyberX-Konsole auf das CyberX-Benutzerhandbuch zugreifen möchten, wählen Sie :::image type="icon" source="media/references-horizon-api//image3.png"::: und anschließend **Benutzerhandbuch herunterladen** aus.

## `horizon::protocol::BaseParser`

Abstrakt für alle Plug-Ins. Dies enthält zwei Methoden:

- Zum Verarbeiten von Plug-In-Filtern, die Sie definiert haben. Auf diese Weise wird Horizon informiert, wie die Kommunikation mit dem Parser erfolgen soll
- Zum Verarbeiten der tatsächlichen Daten.

## `std::shared_ptr<horizon::protocol::BaseParser> create_parser()`

Mit der ersten Funktion, die für das Plug-In aufgerufen wird, wird eine Instanz des Parsers für Horizon erstellt, die erkannt und registriert wird.

### <a name="parameters"></a>Parameter 

Keine

### <a name="return-value"></a>Rückgabewert

„shared_ptr“ an die Parserinstanz.

## `std::vector<uint64_t> horizon::protocol::BaseParser::processDissectAs(const std::map<std::string, std::vector<std::string>> &) const`

Diese Funktion wird für jedes oben registrierte Plug-In aufgerufen. 

In den meisten Fällen bleibt dieser Parameter leer. Löst eine Ausnahme aus, damit Horizon weiß, dass ein Fehler aufgetreten ist.

### <a name="parameters"></a>Parameter 

- Eine Karte mit der Struktur von „dissect_as“, wie in der Datei „config.json“ eines anderen Plug-Ins definiert, das für Sie registriert werden soll.

### <a name="return-value"></a>Rückgabewert 

Ein Array von „uint64_t“, bei dem es sich um die Registrierung handelt, die in eine Art von „uint64_t“ verarbeitet wird. Das heißt, dass in der Karte eine Liste von Ports enthalten ist, deren Werte „uin64_t“ bilden.

## `horizon::protocol::ParserResult horizon::protocol::BaseParser::processLayer(horizon::protocol::management::IProcessingUtils &,horizon::general::IDataBuffer &)`

Die Hauptfunktion, insbesondere die Logik des Plug-ins (jedes Mal, wenn ein neues Paket Ihren Parser erreicht). Diese Funktion wird aufgerufen. Alles im Zusammenhang mit der Paketverarbeitung sollte hier ausgeführt werden.

### <a name="considerations"></a>Überlegungen

Ihr Plug-In sollte threadsicher sein, da diese Funktion möglicherweise von unterschiedlichen Threads aufgerufen wird. Alles im Stapel zu definieren, wäre ein guter Ansatz.

### <a name="parameters"></a>Parameter

- Die SDK-Steuerungseinheit, die für das Speichern der Daten und das Erstellen von SDK-bezogenen Objekten (z. B. ILayer, Felder usw.) verantwortlich ist.
- Ein Hilfsprogramm zum Lesen der Daten aus dem Rohdatenpaket. Für das Hilfsprogramm ist bereits die Bytereihenfolge festgelegt, die Sie in der Datei „config.json“ definiert haben.

### <a name="return-value"></a>Rückgabewert 

Das Ergebnis der Verarbeitung. Mögliche Werte: „Success/Malformed/SanityFailure“ (Erfolg/Falsch formatiert/Integritätsfehler).

## `horizon::protocol::SanityFailureResult: public horizon::protocol::ParserResult`

Kennzeichnet die Verarbeitung als Integritätsfehler, d. h., das Paket wird vom aktuellen Protokoll nicht erkannt, und Horizon sollte es an einen anderen Parser übergeben (falls ein solcher für dieselben Filter registriert ist).

## `horizon::protocol::SanityFailureResult::SanityFailureResult(uint64_t)`

Konstruktor

### <a name="parameters"></a>Parameter 

- Definiert den von Horizon für die Protokollierung verwendeten Fehlercode, wie er in der Datei „config.json“ definiert ist.

## `horizon::protocol::MalformedResult: public horizon::protocol::ParserResult`

Ergebnis: Falsch formatiert. Das heißt, dass das Paket bereits als Protokoll erkannt wurde, einige Überprüfungen jedoch fehlerhaft waren (reservierte Bits sind aktiviert, ein Feld fehlt usw.).

## `horizon::protocol::MalformedResult::MalformedResult(uint64_t)`

Konstruktor

### <a name="parameters"></a>Parameter  

- Fehlercode, wie in der Datei „config.json“ definiert.

## `horizon::protocol::SuccessResult: public horizon::protocol::ParserResult`

Benachrichtigt Horizon über die erfolgreiche Verarbeitung. Bei erfolgreicher Verarbeitung wurde das Paket akzeptiert, die Datenzugehörigkeit ist richtig, und alle Daten wurden extrahiert.

## `horizon::protocol::SuccessResult()`

Konstruktor. Es wurde ein erfolgreiches Basisergebnis erstellt. Das heißt, dass weder die Richtung noch andere Metadaten im Zusammenhang mit dem Paket bekannt sind.

## `horizon::protocol::SuccessResult(horizon::protocol::ParserResultDirection)`

Konstruktor

### <a name="parameters"></a>Parameter 

- Die Richtung des Pakets (sofern definiert). Mögliche Werte: REQUEST (Anforderung), RESPONSE (Antwort)

## `horizon::protocol::SuccessResult(horizon::protocol::ParserResultDirection, const std::vector<uint64_t> &)`

Konstruktor

### <a name="parameters"></a>Parameter

- Die Richtung des Pakets kann, sofern sie definiert wurde, „REQUEST“ oder „RESPONSE“ lauten
- Warnungen. Diese Ereignisse schlagen nicht fehl, aber Horizon wird benachrichtigt.

## `horizon::protocol::SuccessResult(const std::vector<uint64_t> &)`

Konstruktor

### <a name="parameters"></a>Parameter 

-  Warnungen. Diese Ereignisse schlagen nicht fehl, aber Horizon wird benachrichtigt.

## `HorizonID HORIZON_FIELD(const std::string_view &)`

Konvertiert einen zeichenfolgenbasierten Verweis auf einen Feldnamen (z. B. „function_code“) in eine Horizon-ID (HorizonID)

### <a name="parameters"></a>Parameter 

- Zeichenfolge, die konvertiert werden soll

### <a name="return-value"></a>Rückgabewert

- Aus der Zeichenfolge erstellte HorizonID.

## `horizon::protocol::ILayer &horizon::protocol::management::IProcessingUtils::createNewLayer()`

Erstellt eine neue Ebene, damit Horizon weiß, dass das Plug-In Daten speichern möchte. Dies ist die Basisspeichereinheit, die Sie verwenden sollten.

### <a name="return-value"></a>Rückgabewert

Ein Verweis auf eine erstellte Ebene, damit Sie dort Daten hinzufügen können.

## `horizon::protocol::management::IFieldManagement &horizon::protocol::management::IProcessingUtils::getFieldsManager()`

Ruft das Feldverwaltungsobjekt ab, das für das Erstellen von Feldern auf unterschiedlichen Objekten (z. B. ILayer) verantwortlich ist

### <a name="return-value"></a>Rückgabewert

Verweis auf den Manager

## `void horizon::protocol::management::IFieldManagement::create(horizon::protocol::ILayer &, HorizonID, uint64_t)`

Erstellt auf der Ebene mit der angeforderten ID ein neues numerisches Feld mit 64 Bits.

### <a name="parameters"></a>Parameter 

- Die zuvor erstellte Ebene
- Vom Makro „HORIZON_FIELD“ erstellte HorizonID
- Der Rohwert, den Sie speichern möchten

## `void horizon::protocol::management::IFieldManagement::create(horizon::protocol::ILayer &, HorizonID, std::string)`

Erstellt auf der Ebene mit der angeforderten ID ein neues Zeichenfolgenfeld. Der Speicher wird verschoben. Seien Sie daher vorsichtig. Sie können diesen Wert nicht noch einmal verwenden.

### <a name="parameters"></a>Parameter  

- Die zuvor erstellte Ebene
- Vom Makro „HORIZON_FIELD“ erstellte HorizonID
- Der Rohwert, den Sie speichern möchten

## `void horizon::protocol::management::IFieldManagement::create(horizon::protocol::ILayer &, HorizonID, std::vector<char> &)`

Erstellt auf der Ebene mit der angeforderten ID ein neues Feld mit Rohwerten (Bytearray). Der Speicher wird verschoben.Seien Sie daher vorsichtig. Sie können diesen Wert nicht noch einmal verwenden.

### <a name="parameters"></a>Parameter

- Die zuvor erstellte Ebene
- Vom Makro „HORIZON_FIELD“ erstellte HorizonID
- Der Rohwert, den Sie speichern möchten

## `horizon::protocol::IFieldValueArray &horizon::protocol::management::IFieldManagement::create(horizon::protocol::ILayer &, HorizonID, horizon::protocol::FieldValueType)`

Erstellt auf der Ebene des angegebenen Typs mit der angeforderten ID ein Feld mit Arraywerten (Array).

### <a name="parameters"></a>Parameter

- Die zuvor erstellte Ebene
- Vom Makro „HORIZON_FIELD“ erstellte HorizonID
- Typ der Werte, die im Array gespeichert werden

### <a name="return-value"></a>Rückgabewert

Verweis auf ein Array, an das Sie Werte anfügen sollten

## `void horizon::protocol::management::IFieldManagement::create(horizon::protocol::IFieldValueArray &, uint64_t)`

Fügt dem zuvor erstellten Array einen neuen ganzzahligen Wert an

### <a name="parameters"></a>Parameter

- Das zuvor erstellte Array
- Rohwert, der im Array gespeichert werden soll

## `void horizon::protocol::management::IFieldManagement::create(horizon::protocol::IFieldValueArray &, std::string)`

Fügt dem zuvor erstellten Array einen neuen Zeichenfolgenwert an. Der Speicher wird verschoben.Seien Sie daher vorsichtig. Sie können diesen Wert nicht noch einmal verwenden.

### <a name="parameters"></a>Parameter

- Das zuvor erstellte Array
- Rohwert, der im Array gespeichert werden soll

## `void horizon::protocol::management::IFieldManagement::create(horizon::protocol::IFieldValueArray &, std::vector<char> &)`

Fügt dem zuvor erstellten Array einen neuen Rohwert an. Der Speicher wird verschoben.Seien Sie daher vorsichtig. Sie können diesen Wert nicht noch einmal verwenden.

### <a name="parameters"></a>Parameter

- Das zuvor erstellte Array
- Rohwert, der im Array gespeichert werden soll

## `bool horizon::general::IDataBuffer::validateRemainingSize(size_t)`

Überprüft, ob der Puffer mindestens x Bytes enthält.

### <a name="parameters"></a>Parameter

Anzahl der Bytes, die vorhanden sein müssen 

### <a name="return-value"></a>Rückgabewert

TRUE, wenn der Puffer mindestens x Bytes enthält. Andernfalls FALSE.

## `uint8_t horizon::general::IDataBuffer::readUInt8()`

Liest entsprechend der Bytereihenfolge den Wert „uint8“ (1 Byte) aus dem Puffer.

### <a name="return-value"></a>Rückgabewert

Aus dem Puffer gelesener Wert.

## `uint16_t horizon::general::IDataBuffer::readUInt16()`

Liest entsprechend der Bytereihenfolge den Wert „uint16“ (2 Bytes) aus dem Puffer.

### <a name="return-value"></a>Rückgabewert

Aus dem Puffer gelesener Wert.

## `uint32_t horizon::general::IDataBuffer::readUInt32()`

Liest entsprechend der Bytereihenfolge den Wert „uint32“ (4 Bytes) aus dem Puffer.

### <a name="return-value"></a>Rückgabewert

Aus dem Puffer gelesener Wert.

## `uint64_t horizon::general::IDataBuffer::readUInt64()`

Liest entsprechend der Bytereihenfolge den Wert „uint64“ (8 Bytes) aus dem Puffer.

### <a name="return-value"></a>Rückgabewert

Aus dem Puffer gelesener Wert.

## `void horizon::general::IDataBuffer::readIntoRawData(void *, size_t)`

Liest den vorab zugeordneten Speicher einer bestimmten Größe. Kopiert tatsächlich die Daten in Ihren Speicherbereich.

### <a name="parameters"></a>Parameter 

- Speicherbereich, in den die Daten kopiert werden sollen
- Größe des Speicherbereichs. Dieser Parameter definiert auch, wie viele Bytes kopiert werden.

## `std::string_view horizon::general::IDataBuffer::readString(size_t)`

Liest eine Zeichenfolge aus dem Puffer

### <a name="parameters"></a>Parameter 

- Anzahl der Bytes, die gelesen werden sollen.

### <a name="return-value"></a>Rückgabewert

Verweis auf den Speicherbereich der Zeichenfolge.

## `size_t horizon::general::IDataBuffer::getRemainingData()`

Gibt an, wie viele Bytes im Puffer verbleiben.

### <a name="return-value"></a>Rückgabewert

Verbleibende Größe des Puffers.

## `void horizon::general::IDataBuffer::skip(size_t)`

Überspringt x Bytes im Puffer.

### <a name="parameters"></a>Parameter

- Anzahl der zu überspringenden Bytes.
