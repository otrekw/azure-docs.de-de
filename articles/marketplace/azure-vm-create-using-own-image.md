---
title: Erstellen eines Azure-VM-Angebots im Azure Marketplace mit Ihrem eigenen Image
description: Veröffentlichen Sie ein VM-Angebot im Azure Marketplace mit Ihrem eigenen Image.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: krsh
ms.author: krsh
ms.date: 06/02/2021
ms.openlocfilehash: 44a31ee5fc8118ad163b5edb582b2a182b75dd3e
ms.sourcegitcommit: 70ce9237435df04b03dd0f739f23d34930059fef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2021
ms.locfileid: "111525010"
---
# <a name="create-a-virtual-machine-using-your-own-image"></a>Erstellen einer VM mit einem eigenen Image

In diesem Artikel wird beschrieben, wie Sie ein VM-Image veröffentlichen, das Sie lokal erstellt haben.

## <a name="bring-your-image-into-azure"></a>Einbinden Ihres Image in Azure

Laden Sie Ihre VHD in eine Azure Shared Image Gallery hoch.

1. Suchen Sie im Azure-Portal nach **Kataloge mit freigegebenen Images**.
2. Erstellen Sie eine Shared Image Gallery oder verwenden Sie eine bestehende. Wir empfehlen, eine separate Shared Image Gallery für Images zu erstellen, die im Marketplace veröffentlicht werden.
3. Erstellen Sie eine Imagedefinition oder verwenden Sie eine bestehende.
4. Wählen Sie **Version erstellen** aus.
5. Wählen Sie die Region und die Imageversion aus.
6. Wenn Ihre VHD noch nicht ins Azure-Portal hochgeladen wurde, wählen Sie **Speicher-Blobs (VHDs)** als **Quelle** und dann **Durchsuchen aus**. Falls nicht bereits geschehen, können Sie ein **Speicherkonto** und einen **Speichercontainer** erstellen. Laden Sie Ihre VHD hoch.
7. Klicken Sie auf **Überprüfen + erstellen**. Wählen Sie nach Abschluss der Überprüfung **Erstellen** aus.

> [!TIP]
> Für das Herausgeberkonto muss die Zugriffsberechtigung „Besitzer“ festgelegt sein, damit das SIG-Image veröffentlicht werden kann. Führen Sie bei Bedarf die Schritte im folgenden Abschnitt – **Festlegen der richtigen Berechtigungen** – aus, um Zugriff zu gewähren.

## <a name="set-the-right-permissions"></a>Festlegen der richtigen Berechtigungen

Wenn Ihr Partner Center-Konto der Inhaber des Abonnements ist, das die Shared Image Gallery hostet, ist für Berechtigungen nichts weiter erforderlich.

Wenn Sie nur Lesezugriff auf das Abonnement haben, verwenden Sie eine der beiden folgenden Optionen.

### <a name="option-one--ask-the-owner-to-grant-owner-permission"></a>Option 1: Bitten Sie den Besitzer, eine Besitzerberechtigung zu erteilen.

Schritte zum Erteilen der Besitzerberechtigung durch den Besitzer:

1. Wechseln Sie zur Shared Image Gallery-Instanz (SIG).
2. Wählen Sie im linken Bereich **Zugriffssteuerung (IAM)** aus.
3. Wählen Sie **Hinzufügen** und dann **Rollenzuweisung hinzufügen** aus.<br>
    :::image type="content" source="media/create-vm/add-role-assignment.png" alt-text="Screenshot des Fensters „Rollenzuweisung hinzufügen“":::
1. Wählen Sie für **Rolle** die Option **Besitzer** aus.
1. Wählen Sie für **Zugriff zuweisen zu** die Option **Benutzer, Gruppe oder Dienstprinzipal** aus.
1. Geben Sie unter **Auswählen** die Azure-E-Mail-Adresse der Person ein, die das Image veröffentlichen wird.
1. Wählen Sie **Speichern** aus.

### <a name="option-two--run-a-command"></a>Option 2: Führen Sie einen Befehl aus.

Bitten Sie den Besitzer, einen dieser Befehle auszuführen (verwenden Sie in beiden Fällen die Abonnement-ID des Abonnements, in dem Sie die Shared Image Gallery erstellt haben).

```azurecli
az login
az provider register --namespace Microsoft.PartnerCenterIngestion --subscription {subscriptionId}
```
 
```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionId {subscriptionId}
Register-AzResourceProvider -ProviderNamespace Microsoft.PartnerCenterIngestion
```

> [!NOTE]
> Sie müssen keinen URI (Uniform Resource Identifier) generieren, da Sie nun ein Shared Image Gallery-Image in Partner Center veröffentlichen können. Wenn Sie dennoch Informationen zu den Schritten zum Erstellen eines SAS-URIs benötigen, finden Sie diese unter [Generieren eines SAS-URIs für ein VM-Image](azure-vm-get-sas-uri.md).

## <a name="next-steps"></a>Nächste Schritte

- [Testen Sie das VM-Image](azure-vm-image-test.md), um sicherzustellen, dass es die Veröffentlichungsanforderungen von Azure Marketplace erfüllt (optional).
- Wenn Sie Ihr VM-Image nicht testen möchten, melden Sie sich beim [Partner Center](https://partner.microsoft.com/) an und veröffentlichen Sie das SIG-Image.
- Wenn beim Erstellen der neuen Azure-basierten VHD Probleme auftreten, informieren Sie sich in den [FAQ zu VMs für Azure Marketplace](azure-vm-create-faq.md).
