---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/07/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e40f421c0fa45d772cd333dac51fe2bdf2779f48
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91829096"
---
Ein neues Feature des Windows 10-VPN-Clients namens Always On bietet die Möglichkeit, eine VPN-Verbindung aufrechtzuerhalten. Mit Always On kann das aktive VPN-Profil basierend auf Triggern wie Benutzeranmeldung, Änderung des Netzwerkzustands oder aktiver Gerätebildschirm automatisch eine Verbindung herstellen und aufrechterhalten.

Mithilfe von Gateways können Sie unter Windows 10 Always On persistente Benutzertunnel und Gerätetunnel zu Azure einrichten.

Always On-VPN-Verbindungen umfassen zwei Typen von Tunneln:

* **Gerätetunnel**: Stellt eine Verbindung mit bestimmten VPN-Servern her, bevor sich Benutzer am Gerät anmelden. Ein Gerätetunnel wird für Verbindungsszenarios vor der Anmeldung und zur Geräteverwaltung verwendet.

* **Benutzertunnel**: Stellt erst dann eine Verbindung her, nachdem sich Benutzer am Gerät angemeldet haben. Mithilfe von Benutzertunneln können Sie über VPN-Server auf Organisationsressourcen zugreifen.

Gerätetunnel und Benutzertunnel arbeiten unabhängig von ihren VPN-Profilen. Sie können gleichzeitig verbunden werden und können verschiedene Authentifizierungsmethoden und andere VPN-Konfigurationseinstellungen verwenden.
