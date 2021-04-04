---
title: include file
description: Includedatei
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/08/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 0c5e3a095b312d36cb05a78e84a13bc2a73d95d5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91875552"
---
Die DNS-Einstellungen sind für diese Konfiguration nicht obligatorisch, aber das DNS wird benötigt, wenn Sie die Namensauflösung zwischen Ihren VMs nutzen möchten. Wenn ein Wert angegeben wird, wird kein neuer DNS-Server erstellt. Die IP-Adresse des angegebenen DNS-Servers muss dazu in der Lage sein, die Namen für die Ressourcen aufzulösen, mit denen Sie eine Verbindung herstellen möchten.

Nach Erstellung des virtuellen Netzwerks können Sie für die Namensauflösung die IP-Adresse eines DNS-Servers hinzufügen. Öffnen Sie die Einstellungen für Ihr virtuelles Netzwerk, wählen Sie „DNS-Server“ aus, und fügen Sie die IP-Adresse des gewünschten DNS-Servers hinzu, den Sie für die Namensauflösung verwenden möchten.

1. Suchen Sie im Portal nach dem virtuellen Netzwerk.
2. Wählen Sie auf der Seite für Ihr virtuelles Netzwerk im Abschnitt **Einstellungen** die Option **DNS-Server** aus.
3. Fügen Sie einen DNS-Server hinzu.
4. Wählen Sie oben auf der Seite **Speichern** aus, um Ihre Einstellungen zu speichern.