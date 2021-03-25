---
title: Verbinden von Azure Front Door Premium mit einem Speicherkonto-Ursprung mithilfe von Private Link
titleSuffix: Azure Private Link
description: Hier erfahren Sie, wie Sie Azure Front Door Premium privat mit einem Speicherkonto verbinden.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 03/04/2021
ms.author: tyao
ms.openlocfilehash: 885b4d132208ab6f8b470d147438e26a5fd4bab7
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102201667"
---
# <a name="connect-azure-front-door-premium-to-a-storage-account-origin-with-private-link"></a>Verbinden von Azure Front Door Premium mit einem Speicherkonto-Ursprung mithilfe von Private Link

Dieser Artikel führt Sie durch die Konfiguration der Azure Front Door Premium-SKU für die private Verbindung mit Ihrem Speicherkonto-Ursprung unter Verwendung des Azure Private Link-Diensts.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

## <a name="enable-private-link-to-a-storage-account"></a>Aktivieren von Private Link für ein Speicherkonto
 
In diesem Abschnitt ordnen Sie den Private Link-Dienst einem privaten Endpunkt zu, der im privaten Netzwerk von Azure Front Door erstellt wurde. 

1. Wählen Sie in Ihrem Azure Front Door Premium-Profil unter *Einstellungen* die Option **Ursprungsgruppen** aus.

1. Wählen Sie die Ursprungsgruppe aus, die den Speicherkonto-Ursprung enthält, für den Sie Private Link aktivieren möchten.

1. Wählen Sie **+ Ursprung hinzufügen** aus, um einen neuen Speicherkonto-Ursprung hinzuzufügen, oder wählen Sie einen zuvor erstellten Speicherkonto-Ursprung aus der Liste aus.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-storage-account.png" alt-text="Screenshot: Aktivieren von Private Link für ein Speicherkonto":::

1. Wählen Sie unter **Azure-Ressource auswählen** die Option **In mein Verzeichnis** aus. Wählen Sie die folgenden Einstellungen aus, oder geben Sie sie ein, um die Site zu konfigurieren, mit der Azure Front Door Premium privat verbunden werden soll.

    | Einstellung | Wert |
    | ------- | ----- |
    | Region | Wählen Sie die Region aus, die identisch ist oder Ihrem Ursprung am nächsten ist. |
    | Ressourcentyp | Wählen Sie **Microsoft.Storage/storageAccounts** aus. |
    | Resource | Wählen Sie dann Ihr Speicherkonto aus. |
    | Untergeordnete Zielressource | Zur Auswahl stehen *Blob* und *Web*. |
    | Request-Nachricht | Passen Sie die Nachricht an, oder wählen Sie den Standardwert aus. |

1. Wählen Sie dann **Hinzufügen** aus, um Ihre Konfiguration zu speichern.

## <a name="approve-private-endpoint-connection-from-the-storage-account"></a>Genehmigen der privaten Endpunktverbindung über das Speicherkonto

1. Wechseln Sie zu dem Speicherkonto, für das Sie Private Link im letzten Abschnitt konfiguriert haben. Wählen Sie **Netzwerk** unter **Einstellungen** aus.

1. Wählen Sie unter **Netzwerk** die Option **Verbindungen mit privatem Endpunkt** aus. 

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/storage-account-configure-endpoint.png" alt-text="Screenshot der Netzwerkeinstellungen in einer Web-App.":::

1. Wählen Sie die *ausstehende* Anforderung für den privaten Endpunkt aus Azure Front Door Premium aus, und wählen Sie dann **Genehmigen** aus.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-pending-approval.png" alt-text="Screenshot: Ausstehende Anforderung für den privaten Speicherendpunkt":::

1. Nach der Genehmigung sollte der Screenshot wie der folgende Screenshot aussehen. Es dauert einige Minuten, bis die Verbindung vollständig hergestellt ist. Sie können jetzt über Azure Front Door Premium auf Ihr Speicherkonto zugreifen.

    :::image type="content" source="../media/how-to-enable-private-link-storage-account/private-endpoint-approved.png" alt-text="Screenshot: Genehmigte Speicherendpunktanforderung":::

## <a name="next-steps"></a>Nächste Schritte

[Verwenden privater Endpunkte für Azure Storage](../../storage/common/storage-private-endpoints.md)
