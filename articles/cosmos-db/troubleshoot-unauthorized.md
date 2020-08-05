---
title: Troubleshooting für nicht autorisierte Ausnahmen in Azure Cosmos DB
description: Diagnose für und Beheben von nicht autorisierten Ausnahmen
author: j82w
ms.service: cosmos-db
ms.date: 07/13/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 872780ff65dabe5a931ae712c0f99b6e4f3e726d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87293781"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-unauthorized-exception"></a>Diagnose und Troubleshooting für nicht autorisierte Ausnahmen in Azure Cosmos DB

HTTP 401: The MAC signature found in the HTTP request is not the same as the computed signature. (Die in der HTTP-Anforderung gefundene MAC-Signatur entspricht nicht der berechneten Signatur.)
Wenn Sie die folgende 401-Fehlermeldung erhalten haben: „Die in der HTTP-Anforderung gefundene MAC-Signatur entspricht nicht der berechneten Signatur.“ kann durch die folgenden Szenarien verursacht werden.

Bei älteren SDKs kann die Ausnahme als ungültige JSON-Ausnahme anstatt der richtigen nicht autorisierten 401-Ausnahme angezeigt werden. Bei neueren SDKs ist die Anzeige in diesem Szenario korrekt, und es wird eine gültige Fehlermeldung angezeigt.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Die folgende Liste enthält bekannte Gründe und Lösungen für nicht autorisierte Ausnahmen.

### <a name="1-key-was-not-properly-rotated-is-the-most-common-scenario"></a>1. Das häufigste Szenario ist, dass der Schlüssel nicht ordnungsgemäß rotiert wurde.
Die 401-MAC-Signatur wird kurz nach der Schlüsselrotation angezeigt und schließlich ohne Änderungen angehalten. 

#### <a name="solution"></a>Lösung:
Der Schlüssel wurde rotiert und entsprach nicht den [bewährten Methoden](secure-access-to-data.md#key-rotation). Abhängig von der Größe des Cosmos DB-Kontos kann die Schlüsselrotation für das Cosmos DB-Konto von nur wenigen Sekunden bis möglicherweise Tage dauern.

### <a name="2-the-key-is-misconfigured"></a>2. Der Schlüssel ist falsch konfiguriert. 
Das Problem mit der 401-MAC-Signatur ist konsistent und tritt bei allen Aufrufen auf, für die dieser Schlüssel verwendet wird.

#### <a name="solution"></a>Lösung:
Der Schlüssel ist für die Anwendung falsch konfiguriert, und für das Konto wird der falsche Schlüssel verwendet, oder es wurde nicht der gesamte Schlüssel kopiert.

### <a name="3-the-application-is-using-the-read-only-keys-for-write-operations"></a>3. Die Anwendung verwendet die schreibgeschützten Schlüssel für Schreibvorgänge.
Das Problem mit der 401-MAC-Signatur tritt nur bei Schreibvorgängen wie dem Erstellen oder Ersetzen auf. Leseanforderungen sind jedoch erfolgreich.

#### <a name="solution"></a>Lösung:
Ändern Sie die Anwendung so, dass ein Schlüssel für Lese- und Schreibvorgänge verwendet wird, der es ermöglicht, dass Vorgänge erfolgreich abgeschlossen werden.

### <a name="4-race-condition-with-create-container"></a>4. Es gibt eine Racebedingung beim Erstellen eines Containers.
Das Problem mit der 401-MAC-Signatur tritt oft kurz nach der Erstellung eines Containers auf. Der Fehler tritt aber nur solange auf, bis die Containererstellung abgeschlossen ist.

#### <a name="solution"></a>Lösung:
Es liegt eine Racebedingung bei der Containererstellung vor. Eine Anwendungsinstanz versucht, auf den Container zuzugreifen, bevor die Containererstellung abgeschlossen ist. Das häufigste Szenario für diese Racebedingung ist, dass der Container gelöscht und mit demselben Namen neu erstellt wird, während die Anwendung ausgeführt wird. Das SDK versucht, den neuen Container zu verwenden, aber die Containererstellung wird noch ausgeführt, sodass die Schlüssel noch nicht verfügbar sind.

## <a name="next-steps"></a>Nächste Schritte
* [Diagnostizieren und Behandeln von Problemen bei Verwendung des .NET SDK für Azure Cosmos DB](troubleshoot-dot-net-sdk.md)
* Weitere Informationen zu Leistungsrichtlinien für [.NET-Version 3](performance-tips-dotnet-sdk-v3-sql.md) und [.NET-Version 2](performance-tips.md)