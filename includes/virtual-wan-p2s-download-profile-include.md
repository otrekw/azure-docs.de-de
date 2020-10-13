---
title: include file
description: include file
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/06/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e1a9256cc9a015a5d8286de5e5bd7b61ed915a3b
ms.sourcegitcommit: 5abc3919a6b99547f8077ce86a168524b2aca350
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91812756"
---
1. Wählen Sie auf der Seite für Ihr virtuelles WAN die Option **Benutzer-VPN-Konfigurationen** aus.
1. Wählen Sie oben auf der Seite die Option **Benutzer-VPN-Konfiguration herunterladen** aus. Beim Herunterladen der Konfiguration auf WAN-Ebene wird ein integriertes Benutzer-VPN-Profil bereitgestellt, das auf Traffic Manager basiert. Weitere Informationen zu globalen oder hubbasierten Profilen finden Sie unter [Herunterladen eines globalen oder hubbasierten Profils für Benutzer-VPN-Clients](https://docs.microsoft.com/azure/virtual-wan/global-hub-profile). Failoverszenarien werden durch ein globales Profil vereinfacht.

   Wenn ein Hub aus irgendeinem Grund nicht verfügbar ist, stellt die integrierte Datenverkehrsverwaltung des Diensts die Konnektivität mit Azure-Ressourcen für Point-to-Site-Benutzer (über einen anderen Hub) sicher. Sie können jederzeit eine hubspezifische VPN-Konfiguration herunterladen, indem Sie zum entsprechenden Hub navigieren. Laden Sie unter **Benutzer-VPN (Point-to-Site)** das Profil **Benutzer-VPN** für den virtuellen Hub herunter.
1. Klicken Sie nach Abschluss der Erstellung der Datei auf den Link, um die Datei herunterzuladen.
1. Verwenden Sie die Profildatei, um die VPN-Clients zu konfigurieren.
