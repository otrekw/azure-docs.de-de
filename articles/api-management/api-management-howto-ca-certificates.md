---
title: Hinzufügen eines benutzerdefinierten ZS-Zertifikats – Azure API Management | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie in Azure API Management ein benutzerdefiniertes ZS-Zertifikat hinzufügen. Sie können auch Anleitungen zum Löschen eines Zertifikats anzeigen.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/20/2018
ms.author: apimpm
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 51719f23302bfaa036f99e88fcd440d97ca3bc02
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107817808"
---
# <a name="how-to-add-a-custom-ca-certificate-in-azure-api-management"></a>Hinzufügen eines benutzerdefinierten ZS-Zertifikats in Azure API Management

Azure API Management ermöglicht die Installation von ZS-Zertifikaten (Zertifizierungsstellenzertifikaten) auf dem Computer im vertrauenswürdigen Stammzertifikatspeicher und den Zwischenzertifikatspeichern. Diese Funktion sollte verwendet werden, wenn Ihre Dienste ein benutzerdefiniertes ZS-Zertifikat erfordern.

Dieser Artikel erläutert die Verwaltung von ZS-Zertifikaten einer Azure API Management-Dienstinstanz im Azure-Portal.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="upload-a-ca-certificate"></a><a name="step1"> </a>Hochladen eines ZS-Zertifikats

![Hinzufügen von ZS-Zertifikaten](media/api-management-howto-ca-certificates/00.png)

Gehen Sie folgendermaßen vor, um ein neues ZS-Zertifikat hochzuladen. Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Tutorial [Erstellen einer API Management-Dienstinstanz](get-started-create-service-instance.md).

1. Navigieren Sie im Azure-Portal zu Ihrer Azure API Management-Dienstinstanz.

2. Wählen Sie im Menü die Option **ZS-Zertifikate** aus.

3. Klicken Sie auf die Schaltfläche **+ Hinzufügen**.  

    ![Screenshot der Schaltfläche „+ Hinzufügen“ zum Hinzufügen eines ZS-Zertifikats](media/api-management-howto-ca-certificates/01.png)  

4. Suchen Sie nach dem Zertifikat, und wählen Sie den Zertifikatspeicher aus. Es ist nur der öffentliche Schlüssel erforderlich, kein Kennwort.

    ![Screenshot der Navigation zum Zertifikat](media/api-management-howto-ca-certificates/02.png)  

5. Klicken Sie auf **Speichern**. Dieser Vorgang kann einige Minuten dauern.

    ![Screenshot der Speicherung des Zertifikats](media/api-management-howto-ca-certificates/03.png)  

> [!NOTE]
> Sie können ein ZS-Zertifikat mithilfe des PowerShell-Befehls `New-AzApiManagementSystemCertificate` hochladen.

## <a name="delete-a-client-certificate"></a><a name="step1a"> </a>Löschen eines Clientzertifikats

Um ein Zertifikat zu löschen, klicken Sie auf das Kontextmenü **...**, und wählen Sie neben dem betreffenden Zertifikat **Löschen** aus.

![Löschen von ZS-Zertifikaten](media/api-management-howto-ca-certificates/04.png)  

[Upload a CA certificate]: #step1
[Delete a CA certificate]: #step1a
