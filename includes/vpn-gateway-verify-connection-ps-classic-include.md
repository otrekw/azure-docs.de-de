---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: dd2dd84cbcd50fba48011e1836cdc64a6ad5855d
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86040746"
---
Mit dem Cmdlet „Get-AzureVNetConnection“ können Sie überprüfen, ob die Verbindung erfolgreich hergestellt wurde.

1. Verwenden Sie das folgende Cmdlet-Beispiel, und konfigurieren Sie die Werte so, dass sie Ihren eigenen Werten entsprechen. Der Name des virtuellen Netzwerks muss in Anführungszeichen gesetzt werden, wenn er Leerstellen enthält.

   ```azurepowershell
   Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
   ```
2. Sehen Sie sich nach Abschluss des Cmdlets die Werte an. Im Beispiel weiter unten ist der Verbindungsstatus als „Connected“ (Verbunden) angegeben, und Sie sehen die Eingangs- und Ausgangsbytes.

```output
ConnectivityState         : Connected
EgressBytesTransferred    : 181664
IngressBytesTransferred   : 182080
LastConnectionEstablished : 1/7/2016 12:40:54 AM
LastEventID               : 24401
LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                            Connected.
LastEventTimeStamp        : 1/7/2016 12:40:54 AM
LocalNetworkSiteName      : RMVNetLocal
```
