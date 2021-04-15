---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/10/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 91dc6ba0bcd455aefe9de2efdff2feb20938152d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100376383"
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Schritt 1: Navigieren zum Gateway für virtuelle Netzwerke

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Alle Ressourcen**. 
2. Um die Seite mit dem Gateway für virtuelle Netzwerke zu öffnen, navigieren Sie zu dem Gateway für virtuelle Netzwerke, und klicken Sie darauf, um es auszuwählen. 

### <a name="step-2-delete-connections"></a>Schritt 2: Löschen von Verbindungen

1. Klicken Sie auf der Seite für das Gateway für virtuelle Netzwerke auf **Verbindungen**, um alle Verbindungen mit dem Gateway anzuzeigen.
2. Klicken Sie auf **...** in der Zeile mit dem Namen der Verbindung, und wählen Sie in der Dropdownliste die Option **Löschen** aus.
3. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie die Verbindung löschen möchten. Wenn Sie über mehrere Verbindungen verfügen, löschen Sie jede Verbindung.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Schritt 3: Löschen des Gateways für virtuelle Netzwerke

Hinweis: Wenn Sie neben der S2S-Konfiguration noch über eine P2S-Konfiguration für dieses VNET verfügen, werden automatisch alle P2S-Clients ohne Warnung getrennt, wenn Sie das Gateway des virtuellen Netzwerks löschen.

1. Klicken Sie auf der Seite für das Gateway für virtuelle Netzwerke auf **Übersicht**.
2. Klicken Sie auf der Seite **Übersicht** auf **Löschen**, um das Gateway zu löschen.
