---
title: include file
description: include file
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/07/2020
ms.custom: include file
ms.openlocfilehash: 9ccdc7a438d1dade534d39dc97a39a3bdae37dc4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "75895399"
---
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie die Startseitenleiste und dann **+ Ressource erstellen** aus. 

   [![Startseitenleiste erweitern, dann „+ Ressource erstellen“ auswählen](./media/create-digital-twins-portal/azure-portal-create-a-resource.png)](./media/create-digital-twins-portal/azure-portal-create-a-resource.png#lightbox)

1. Suchen Sie nach **Digital Twins**, und wählen Sie **Digital Twins** aus. 

   [![Auswahl zum Erstellen einer neuen Digital Twins-Instanz](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png)](./media/create-digital-twins-portal/azure-portal-create-digital-twins.png#lightbox)

   Wählen Sie alternativ **Internet der Dinge** und dann **Digital Twins (Vorschauversion)** aus.

1. Wählen Sie **Erstellen** aus, um den Bereitstellungsprozess zu starten.

   [![Bereitstellung der Ressource erstellen und bestätigen](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png)](./media/create-digital-twins-portal/azure-create-and-confirm-resource.png#lightbox)

1. Geben Sie im Bereich **Digital Twins** die folgenden Informationen ein:
   * **Ressourcenname**: Erstellen Sie einen eindeutigen Namen für Ihre Digital Twins-Instanz.
   * **Abonnement**: Wählen Sie das Abonnement aus, das Sie zum Erstellen dieser Digital Twins-Instanz verwenden möchten. 
   * **Ressourcengruppe**: Hier können Sie eine [Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) für die Digital Twins-Instanz auswählen oder erstellen.
   * **Standort**: Wählen Sie den Standort aus, der Ihren Geräten am nächsten ist.

     [![Digital Twins-Bereich mit eingegebenen Informationen](./media/create-digital-twins-portal/create-digital-twins-param.png)](./media/create-digital-twins-portal/create-digital-twins-param.png#lightbox)

1. Überprüfen Sie Ihre Informationen zu Digital Twins, und wählen Sie **Erstellen** aus. Die Erstellung Ihrer Digital Twins-Instanz kann einige Minuten dauern. Sie können den Fortschritt im Bereich **Benachrichtigungen** überwachen.

1. Öffnen Sie den Bereich **Übersicht** Ihrer Digital Twins-Instanz. Beachten Sie den Link unter **Verwaltungs-API**. Die URL der **Verwaltungs-API** ist wie folgt formatiert: 
   
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger
   ```
   
   Mit dieser URL gelangen Sie zur Dokumentation zur Azure Digital Twins-REST-API, die für Ihre Instanz gilt. Lesen Sie [Verwenden von Digital Twins Swagger](../articles/digital-twins/how-to-use-swagger.md), um zu erfahren, wie diese API-Dokumentation zu lesen und zu verwenden ist. Kopieren und ändern Sie die URL der **Verwaltungs-API** in dieses Format: 
    
   ```URL
   https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/
   ```
    
   Ihre Anwendung verwendet die geänderte URL als Basis-URL für den Zugriff auf Ihre Instanz. Kopieren Sie diese geänderten URL in eine temporäre Datei. Sie wird im nächsten Abschnitt benötigt.

   [![Verwaltungs-API – Übersicht](./media/create-digital-twins-portal/digital-twins-management-api.png)](./media/create-digital-twins-portal/digital-twins-management-api.png#lightbox)