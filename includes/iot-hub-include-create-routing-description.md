---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 3a5747912a18b8406cabd03c9823e3f6fe6898a6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "72808813"
---
<!-- description of message routing used in the Azure CLI, PowerShell, and RM routing articles.-->

Sie leiten Nachrichten an verschiedene Ressourcen weiter, abhängig von Eigenschaften, die vom simulierten Gerät an die Nachricht angefügt werden. Nachrichten ohne benutzerdefinierte Weiterleitung, werden an den Standardendpunkt gesendet (Nachrichten/Ereignisse). Im nächsten Tutorial werden Nachrichten an IoT Hub gesendet und an verschiedene Ziele weitergeleitet.

|Wert |Ergebnis|
|------|------|
|level="storage" |Schreibvorgang in Azure Storage.|
|level="critical" |Schreibvorgang in eine Service Bus-Warteschlange. Eine Logik-App ruft die Nachricht aus der Warteschlange ab und sendet sie mithilfe von Office 365.|
|default |Anzeigen dieser Daten mithilfe von Power BI.|

Als Erstes muss der Endpunkt eingerichtet werden, an den die Daten weitergeleitet werden. Danach wird die Nachrichtenroute eingerichtet, die diesen Endpunkt verwendet. Nach Einrichtung des Routings können Sie die Endpunkte und Nachrichtenrouten im Portal anzeigen.