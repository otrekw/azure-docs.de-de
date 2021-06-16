---
title: Datei einfügen
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/26/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1345df8991acba09feb65eb11e9eadc21b9c7af4
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579694"
---
Ein neues Feature des Windows 10-VPN-Clients namens Always On bietet die Möglichkeit, eine VPN-Verbindung aufrechtzuerhalten. Mit Always On kann das aktive VPN-Profil basierend auf Triggern wie Benutzeranmeldung, Änderung des Netzwerkzustands oder aktiver Gerätebildschirm automatisch eine Verbindung herstellen und aufrechterhalten.

Mithilfe von Gateways können Sie unter Windows 10 Always On persistente Benutzertunnel und Gerätetunnel zu Azure einrichten.

Always On-VPN-Verbindungen umfassen zwei Typen von Tunneln:

* **Gerätetunnel**: Stellt eine Verbindung mit bestimmten VPN-Servern her, bevor sich Benutzer am Gerät anmelden. Ein Gerätetunnel wird für Verbindungsszenarios vor der Anmeldung und zur Geräteverwaltung verwendet.

* **Benutzertunnel**: Stellt erst dann eine Verbindung her, nachdem sich Benutzer am Gerät angemeldet haben. Mithilfe von Benutzertunneln können Sie über VPN-Server auf Organisationsressourcen zugreifen.

Gerätetunnel und Benutzertunnel arbeiten unabhängig von ihren VPN-Profilen. Sie können gleichzeitig verbunden werden und können verschiedene Authentifizierungsmethoden und andere VPN-Konfigurationseinstellungen verwenden.
