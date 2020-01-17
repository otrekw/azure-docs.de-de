---
title: include file
description: include file
services: signalr
author: wesmc7777
ms.service: signalr
ms.topic: include
ms.date: 04/17/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 39bff26baea622e6c0ed524ca68c3c8bae4e770d
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76020987"
---
1. Melden Sie sich zunächst beim [Azure-Portal](https://portal.azure.com) an, um eine Azure SignalR Service-Ressource zu erstellen. Wählen Sie links oben auf der Seite die Option **+ Ressource erstellen** aus. Geben Sie im Textfeld **Marketplace durchsuchen** **SignalR Service** ein.

2. Wählen Sie in den Ergebnissen **SignalR Service** und dann **Erstellen** aus.

3. Fügen Sie auf der neuen **SignalR**-Seite „Einstellungen“ die folgenden Einstellungen für die neue SignalR-Ressource hinzu:

    | Name | Empfohlener Wert | Beschreibung |
    | ---- | ----------------- | ----------- |
    | Ressourcenname | *testsignalr* | Geben Sie für die SignalR-Ressource einen eindeutigen Ressourcennamen ein. Der Name muss zwischen 1 und 63 Zeichen lang sein und darf nur Zahlen, Buchstaben und den Bindestrich (`-`) enthalten. Der Name darf weder mit dem Bindestrich beginnen oder enden, noch mehrere aufeinanderfolgende Bindestriche enthalten.|
    | Subscription | Auswählen Ihres Abonnements |  Wählen Sie das Azure-Abonnement aus, das Sie zum Testen von SignalR verwenden möchten. Wenn das Konto nur über ein einzelnes Abonnement verfügt, wird automatisch dieses Abonnement ausgewählt, und die Dropdownliste **Abonnement** wird nicht angezeigt.|
    | Resource group | Erstellen einer Ressourcengruppe namens *SignalRTestResources*| Wählen Sie eine Ressourcengruppe für Ihre SignalR-Ressource aus, oder erstellen Sie eine. Diese Gruppe ist beim Organisieren mehrerer Ressourcen hilfreich, die Sie möglicherweise zur gleichen Zeit löschen möchten, indem Sie die Ressourcengruppe löschen. Weitere Informationen finden Sie unter [Verwenden von Ressourcengruppen zum Verwalten von Azure-Ressourcen](../articles/azure-resource-manager/management/overview.md). |
    | Location | *USA, Osten* | Verwenden Sie **Standort**, um den geografischen Standort anzugeben, an dem Ihre SignalR-Ressource gehostet wird. Für die beste Leistung wird empfohlen, dass Sie die Ressource in derselben Region wie andere Komponenten Ihrer Anwendung erstellen. |
    | Tarif | *Free* | Aktuell sind die Optionen **Free** und **Standard** verfügbar. |
    | An Dashboard anheften | ✔ | Aktivieren Sie dieses Kontrollkästchen, um die Ressource an Ihr Dashboard anzuheften, damit sie einfacher zu finden ist. |

4. Klicken Sie auf **Erstellen**. Die Bereitstellung kann einige Minuten dauern.

5. Nachdem die Bereitstellung abgeschlossen ist, wählen Sie unter **EINSTELLUNGEN** die Option **Schlüssel** aus. Kopieren Sie Ihre Verbindungszeichenfolge für den Primärschlüssel. Sie werden die Zeichenfolge später zum Konfigurieren Ihrer App verwenden, um die Azure SignalR Service-Ressource zu verwenden.

    Die Verbindungszeichenfolge weist das folgende Format auf:
    
        Endpoint=<service_endpoint>;AccessKey=<access_key>;
