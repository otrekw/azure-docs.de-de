---
title: Programmgesteuertes Erstellen von Azure-Abonnements für eine Microsoft-Kundenvereinbarung mit den neuesten APIs
description: Hier erfahren Sie, wie Sie mithilfe der neuesten Versionen der REST-API, der Azure CLI und von Azure PowerShell programmgesteuert Azure-Abonnements für eine Microsoft-Kundenvereinbarung erstellen.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 11/17/2020
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 61a658cc9654a93b4c92fda6cc1f38cd2e77dafa
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102216087"
---
# <a name="programmatically-create-azure-subscriptions-for-a-microsoft-customer-agreement-with-the-latest-apis"></a>Programmgesteuertes Erstellen von Azure-Abonnements für eine Microsoft-Kundenvereinbarung mit den neuesten APIs

In diesem Artikel wird erläutert, wie Sie mithilfe der aktuellen API-Versionen programmgesteuert Azure-Abonnements für eine Microsoft-Kundenvereinbarung erstellen. Wenn Sie noch die ältere Vorschauversion verwenden, finden Sie weitere Informationen unter [Programmgesteuertes Erstellen von Azure-Abonnements mit Vorschau-APIs](programmatically-create-subscription-preview.md). 

Dieser Artikel enthält Informationen zum programmgesteuerten Erstellen von Abonnements mithilfe von Azure Resource Manager.

Wenn Sie ein Azure-Abonnement programmgesteuert erstellen, unterliegt es der Vereinbarung, auf deren Grundlage Sie Azure-Dienste von Microsoft oder einem autorisierten Handelspartner erhalten haben. Weitere Informationen finden Sie unter [Rechtliche Hinweise zu Microsoft Azure](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen über die Rolle „Besitzer“, „Mitwirkender“ oder „Azure-Abonnementersteller“ für einen Rechnungsabschnitt oder die Rolle „Besitzer“ oder „Mitwirkender“ für ein Abrechnungsprofil oder -konto verfügen, um Abonnements erstellen zu können. Weitere Informationen finden Sie unter [Rollen und Aufgaben für die Abonnementabrechnung](understand-mca-roles.md#subscription-billing-roles-and-tasks).

Wenn Sie nicht wissen, ob Sie Zugriff auf ein Konto für eine Microsoft-Kundenvereinbarung haben, finden Sie weitere Informationen unter [Überprüfen des Zugriffs auf eine Microsoft-Kundenvereinbarung](../understand/mca-overview.md#check-access-to-a-microsoft-customer-agreement).

In den folgenden Beispielen werden REST-APIs verwendet. PowerShell und Azure CLI werden derzeit nicht unterstützt.

## <a name="find-billing-accounts-that-you-have-access-to"></a>Suchen nach Abrechnungskonten, auf die Sie Zugriff haben

Verwenden Sie die folgende Anforderung, um alle Abrechnungskonten aufzulisten:

### <a name="rest"></a>[REST](#tab/rest-getBillingAccounts)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/?api-version=2020-05-01

```
In der API-Antwort sind die Abrechnungskonten aufgeführt, auf die Sie Zugriff haben.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "billingProfiles": {
          "hasMoreResults": false
        },
        "displayName": "Contoso",
        "hasReadAccess": false
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}
```

Verwenden Sie die `displayName`-Eigenschaft, um das Abrechnungskonto zu identifizieren, für das Sie Abonnements erstellen möchten. Vergewissern Sie sich, dass als „agreementType“ des Kontos die Option *MicrosoftCustomerAgreement* festgelegt ist. Kopieren Sie den Namen (`name`) des Kontos.  Wenn Sie beispielsweise ein Abonnement für das Abrechnungskonto `Contoso` erstellen möchten, kopieren Sie `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Halten Sie den Wert für den nächsten Schritt bereit.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-getBillingAccounts)

```azurepowershell-interactive
PS C:\WINDOWS\system32> Get-AzBillingAccount
```
Sie erhalten eine Liste aller Abrechnungskonten, auf die Sie Zugriff haben. 

```json
Name          : 5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx
DisplayName   : Contoso
AccountStatus : Active
AccountType   : Enterprise
AgreementType : MicrosoftCustomerAgreement
HasReadAccess : True
```
Verwenden Sie die `displayName`-Eigenschaft, um das Abrechnungskonto zu identifizieren, für das Sie Abonnements erstellen möchten. Vergewissern Sie sich, dass als „agreementType“ des Kontos die Option *MicrosoftCustomerAgreement* festgelegt ist. Kopieren Sie den Namen (`name`) des Kontos.  Wenn Sie beispielsweise ein Abonnement für das Abrechnungskonto `Contoso` erstellen möchten, kopieren Sie `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Halten Sie den Wert für den nächsten Schritt bereit.


### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli-getBillingAccounts)
```azurecli
> az billing account list
```
Sie erhalten eine Liste aller Abrechnungskonten, auf die Sie Zugriff haben. 

```json
[
  {
    "accountStatus": "Active",
    "accountType": "Enterprise",
    "agreementType": "MicrosoftCustomerAgreement",
    "billingProfiles": {
      "hasMoreResults": false,
      "value": null
    },
    "departments": null,
    "displayName": "Contoso",
    "enrollmentAccounts": null,
    "enrollmentDetails": null,
    "hasReadAccess": true,
    "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
    "name": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
    "soldTo": null,
    "type": "Microsoft.Billing/billingAccounts"
  }
]
```

Verwenden Sie die `displayName`-Eigenschaft, um das Abrechnungskonto zu identifizieren, für das Sie Abonnements erstellen möchten. Vergewissern Sie sich, dass als „agreementType“ des Kontos die Option *MicrosoftCustomerAgreement* festgelegt ist. Kopieren Sie den Namen (`name`) des Kontos.  Wenn Sie beispielsweise ein Abonnement für das Abrechnungskonto `Contoso` erstellen möchten, kopieren Sie `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Halten Sie den Wert für den nächsten Schritt bereit.

---

## <a name="find-billing-profiles--invoice-sections-to-create-subscriptions"></a>Suchen nach Abrechnungsprofilen und Rechnungsabschnitten zum Erstellen von Abonnements

Die Gebühren für Ihr Abonnement werden in einem Abschnitt der Rechnung eines Abrechnungsprofils angezeigt. Verwenden Sie die folgende API, um die Liste mit den Abrechnungsprofilen und Rechnungsabschnitten abzurufen, für die Sie über die Berechtigung zum Erstellen von Azure-Abonnements verfügen.

Zuerst wird die Liste der Abrechnungsprofile unter dem Abrechnungskonto abgerufen, auf das Sie Zugriff haben (verwenden Sie den `name`, den Sie im vorherigen Schritt erhalten haben).

### <a name="rest"></a>[REST](#tab/rest-getBillingProfiles)
```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingprofiles/?api-version=2020-05-01
```
In der API-Antwort sind alle Abrechnungsprofile aufgelistet, für die Sie über Zugriff zum Erstellen von Abonnements verfügen:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx",
      "name": "AW4F-xxxx-xxx-xxx",
      "properties": {
        "billingRelationshipType": "Direct",
        "billTo": {
          "addressLine1": "One Microsoft Way",
          "city": "Redmond",
          "companyName": "Contoso",
          "country": "US",
          "email": "kenny@contoso.com",
          "phoneNumber": "425xxxxxxx",
          "postalCode": "98052",
          "region": "WA"
        },
        "currency": "USD",
        "displayName": "Contoso Billing Profile",
        "enabledAzurePlans": [
          {
            "skuId": "0002",
            "skuDescription": "Microsoft Azure Plan for DevTest"
          },
          {
            "skuId": "0001",
            "skuDescription": "Microsoft Azure Plan"
          }
        ],
        "hasReadAccess": true,
        "invoiceDay": 5,
        "invoiceEmailOptIn": false,
        "invoiceSections": {
          "hasMoreResults": false
        },
        "poNumber": "001",
        "spendingLimit": "Off",
        "status": "Active",
        "systemId": "AW4F-xxxx-xxx-xxx",
        "targetClouds": []
      },
      "type": "Microsoft.Billing/billingAccounts/billingProfiles"
    }
  ]
}
```

 Kopieren Sie `id`, um als Nächstes die Rechnungsabschnitte unter dem Abrechnungsprofil zu ermitteln. Kopieren Sie beispielsweise `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx`, und rufen Sie die folgende API auf:

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoicesections?api-version=2020-05-01
```
### <a name="response"></a>Antwort

```json
{
  "totalCount": 1,
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx",
      "name": "SH3V-xxxx-xxx-xxx",
      "properties": {
        "displayName": "Development",
        "state": "Active",
        "systemId": "SH3V-xxxx-xxx-xxx"
      },
      "type": "Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections"
    }
  ]
}
```

Verwenden Sie die `id`-Eigenschaft, um den Rechnungsabschnitt zu identifizieren, für den Sie Abonnements erstellen möchten. Kopieren Sie die gesamte Zeichenfolge. Beispiel: `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx`. 

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-getBillingProfiles)

```powershell-interactive
PS C:\WINDOWS\system32> Get-AzBillingProfile -BillingAccountName 5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx
```

Sie erhalten als Teil der Antwort die Liste der Abrechnungsprofile unter diesem Konto.

```json
Name              : AW4F-xxxx-xxx-xxx
DisplayName       : Contoso Billing Profile
Currency          : USD
InvoiceDay        : 5
InvoiceEmailOptIn : True
SpendingLimit     : Off
Status            : Active
EnabledAzurePlans : {0002, 0001}
HasReadAccess     : True
BillTo            :
CompanyName       : Contoso
AddressLine1      : One Microsoft Way
AddressLine2      : 
City              : Redmond
Region            : WA
Country           : US
PostalCode        : 98052
```

Notieren Sie sich den `name` des Abrechnungsprofils aus der obigen Antwort. In den nächsten Schritten wird der Rechnungsabschnitt abgerufen, auf den Sie unterhalb dieses Abrechnungsprofils zugreifen können. Sie benötigen den `name` des Abrechnungskontos und des Abrechnungsprofils.

```powershell-interactive
PS C:\WINDOWS\system32> Get-AzInvoiceSection -BillingAccountName 5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx -BillingProfileName AW4F-xxxx-xxx-xxx
```

Der Rechnungsabschnitt wird zurückgegeben.

```json
Name        : SH3V-xxxx-xxx-xxx
DisplayName : Development
```

Der obige `name` ist der Name des Rechnungsabschnitts, unter dem Sie ein Abonnement erstellen müssen. Erstellen Sie den Abrechnungsbereich mit dem Format „/providers/Microsoft.Billing/billingAccounts/<BillingAccountName>/billingProfiles/<BillingProfileName>/invoiceSections/<InvoiceSectionName>“. In diesem Beispiel entspricht dies `"/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx"`.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli-getBillingProfiles)

```azurecli-interactive
> az billing profile list --account-name "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx" --expand "InvoiceSections"
```
Diese API gibt die Liste der Abrechnungsprofile und Rechnungsabschnitte unter dem angegebenen Abrechnungskonto zurück.

```json
[
  {
    "billTo": {
      "addressLine1": "One Microsoft Way",
      "addressLine2": "",
      "addressLine3": null,
      "city": "Redmond",
      "companyName": "Contoso",
      "country": "US",
      "district": null,
      "email": null,
      "firstName": null,
      "lastName": null,
      "phoneNumber": null,
      "postalCode": "98052",
      "region": "WA"
    },
    "billingRelationshipType": "Direct",
    "currency": "USD",
    "displayName": "Contoso Billing Profile",
    "enabledAzurePlans": [
      {
        "skuDescription": "Microsoft Azure Plan for DevTest",
        "skuId": "0002"
      },
      {
        "skuDescription": "Microsoft Azure Plan",
        "skuId": "0001"
      }
    ],
    "hasReadAccess": true,
    "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx",
    "indirectRelationshipInfo": null,
    "invoiceDay": 5,
    "invoiceEmailOptIn": true,
    "invoiceSections": {
      "hasMoreResults": false,
      "value": [
        {
          "displayName": "Field_Led_Test_Ace",
          "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx",
          "labels": null,
          "name": "SH3V-xxxx-xxx-xxx",
          "state": "Active",
          "systemId": "SH3V-xxxx-xxx-xxx",
          "targetCloud": null,
          "type": "Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections"
        }
      ]
    },
    "name": "AW4F-xxxx-xxx-xxx",
    "poNumber": null,
    "spendingLimit": "Off",
    "status": "Warned",
    "statusReasonCode": "PastDue",
    "systemId": "AW4F-xxxx-xxx-xxx",
    "targetClouds": [],
    "type": "Microsoft.Billing/billingAccounts/billingProfiles"
  }
]
```
Verwenden Sie die id-Eigenschaft unter dem Rechnungsabschnittsobjekt, um den Rechnungsabschnitt zu identifizieren, für den Sie Abonnements erstellen möchten. Kopieren Sie die gesamte Zeichenfolge. Beispiel: /providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx.

---

## <a name="create-a-subscription-for-an-invoice-section"></a>Erstellen eines Abonnements für einen Rechnungsabschnitt

Im folgenden Beispiel wird ein Abonnement mit dem Namen *Dev Team subscription* für den Rechnungsabschnitt *Development* erstellt. Die Kosten für das Abonnement werden dem Abrechnungsprofil *Contoso Billing Profile* berechnet und im Abschnitt *Development* der Rechnung aufgeführt. Sie verwenden den kopierten Abrechnungsbereich aus dem vorherigen Schritt: `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx`. 

### <a name="rest"></a>[REST](#tab/rest-MCA)

```json
PUT  https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="request-body"></a>Anforderungstext

```json
{
  "properties":
    {
        "billingScope": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx",
        "DisplayName": "Dev Team subscription",
        "Workload": "Production"
    }
}
```

### <a name="response"></a>Antwort

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "subscriptionId": "b5bab918-e8a9-4c34-a2e2-ebc1b75b9d74",
    "provisioningState": "Accepted"
  }
}
```

Sie können eine GET-Anforderung für die gleiche URL ausführen, um den Status der Anforderung zu erhalten.

### <a name="request"></a>Anforderung

```json
GET https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="response"></a>Antwort

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "subscriptionId": "b5bab918-e8a9-4c34-a2e2-ebc1b75b9d74",
    "provisioningState": "Succeeded"
  }
}
```

Der Status „In Bearbeitung“ wird unter `provisioningState` als Status `Accepted` zurückgegeben.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-MCA)

Führen Sie `Install-Module Az.Subscription` aus, um die neueste Version des Moduls zu installieren, das das Cmdlet `New-AzSubscriptionAlias` enthält. Eine aktuelle Version von PowerShellGet finden Sie unter [Abrufen des PowerShellGet-Moduls](/powershell/scripting/gallery/installing-psget).

Führen Sie den folgenden Befehl [New-AzSubscriptionAlias](/powershell/module/az.subscription/new-azsubscription) aus, und verwenden Sie den Abrechnungsbereich `"/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx"`. 

```azurepowershell-interactive
New-AzSubscriptionAlias -AliasName "sampleAlias" -SubscriptionName "Dev Team Subscription" -BillingScope "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx" -Workload 'Production"
```

Sie erhalten die Abonnement-ID (subscriptionId) als Teil der Befehlsantwort.

```azurepowershell
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  },
  "type": "Microsoft.Subscription/aliases"
}
```

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli-MCA)

Installieren Sie zunächst die Erweiterung, indem Sie `az extension add --name account` und `az extension add --name alias` ausführen.

Führen Sie den folgenden Befehl [az account alias create](/cli/azure/ext/account/account/alias#ext_account_az_account_alias_create) aus.

```azurecli-interactive
az account alias create --name "sampleAlias" --billing-scope "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx" --display-name "Dev Team Subscription" --workload "Production"
```

Sie erhalten die Abonnement-ID (subscriptionId) als Teil der Befehlsantwort.

```azurecli
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  },
  "type": "Microsoft.Subscription/aliases"
}
```

---

## <a name="next-steps"></a>Nächste Schritte

* Da Sie nun ein Abonnement erstellt haben, können Sie diese Möglichkeit auch für andere Benutzer und Dienstprinzipale eröffnen. Weitere Informationen finden Sie unter [Gewähren des Zugriffs zum Erstellen von Azure Enterprise-Abonnements (Vorschau) ](grant-access-to-create-subscription.md).
* Weitere Informationen zum Verwalten einer großen Anzahl von Abonnements mithilfe von Verwaltungsgruppen finden Sie unter [Was sind Azure-Verwaltungsgruppen?](../../governance/management-groups/overview.md).
