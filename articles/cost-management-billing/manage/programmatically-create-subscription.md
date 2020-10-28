---
title: Programmgesteuertes Erstellen von Azure-Abonnements mit den neuesten APIs
description: Hier erfahren Sie, wie Sie mithilfe der neuesten Versionen der REST-API, der Azure CLI und von Azure PowerShell programmgesteuert Azure-Abonnements erstellen.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 10/12/2020
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 59d3a44b1eff544f7214014f2dd31edc92bfc6bc
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92748178"
---
# <a name="programmatically-create-azure-subscriptions-with-the-latest-apis"></a>Programmgesteuertes Erstellen von Azure-Abonnements mit den neuesten APIs

In diesem Artikel wird erläutert, wie Sie mithilfe der aktuellen API-Versionen programmgesteuert Azure-Abonnements erstellen. Wenn Sie noch die ältere Vorschauversion verwenden, finden Sie weitere Informationen unter [Programmgesteuertes Erstellen von Azure-Abonnements mit Vorschau-APIs](programmatically-create-subscription-preview.md). 

Azure-Kunden mit einem Abrechnungskonto für die folgenden Vereinbarungstypen können Abonnements programmgesteuert erstellen:

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft-Kundenvereinbarung (Microsoft Customer Agreement, MCA)](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/)
- [Microsoft-Partnervereinbarung (MPA)](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement)

Dieser Artikel enthält Informationen zum programmgesteuerten Erstellen von Abonnements mithilfe von Azure Resource Manager.

Wenn Sie ein Azure-Abonnement programmgesteuert erstellen, unterliegt es der Vereinbarung, auf deren Grundlage Sie Azure-Dienste von Microsoft oder einem autorisierten Handelspartner erhalten haben. Weitere Informationen finden Sie unter [Rechtliche Hinweise zu Microsoft Azure](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="create-subscriptions-for-an-ea-billing-account"></a>Erstellen von Abonnements für ein Abrechnungskonto vom Typ „EA“

Verwenden Sie die Informationen in den folgenden Abschnitten, um EA-Abonnements zu erstellen.

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen über die Rolle „Besitzer“ für ein Registrierungskonto verfügen, um ein Abonnement zu erstellen. Es gibt zwei Möglichkeiten, wie Sie die Rolle erhalten:

* Der Enterprise-Administrator Ihrer Registrierung kann [Sie zum Kontobesitzer machen](https://ea.azure.com/helpdocs/addNewAccount) (Anmeldung erforderlich), damit Sie zum Besitzer des Registrierungskontos werden.
* Ein vorhandener Besitzer des Registrierungskontos kann [Ihnen Zugriff gewähren](grant-access-to-create-subscription.md). Wenn Sie einen Dienstprinzipal verwenden möchten, um ein EA-Abonnement zu erstellen, müssen Sie entsprechend [diesem Dienstprinzipal die Berechtigung zum Erstellen von Abonnements erteilen](grant-access-to-create-subscription.md).

### <a name="find-accounts-you-have-access-to"></a>Ermitteln der Konten, auf die Sie Zugriff besitzen

Nachdem Sie einem Registrierungskonto hinzugefügt wurden, das einem Kontobesitzer zugeordnet ist, verwendet Azure die Beziehung zwischen dem Konto und der Registrierung für die Ermittlung, wo die Abonnementgebühren angerechnet werden sollen. Alle Abonnements, die unter dem Konto erstellt werden, werden über die EA-Registrierung abgerechnet, in der sich das Konto befindet. Um Abonnements zu erstellen, müssen Sie Werte zum Registrierungskonto und den Benutzerprinzipalen als Besitzer des Abonnements übergeben.

Um die folgenden Befehle ausführen zu können, müssen Sie im *Basisverzeichnis* des Kontobesitzers angemeldet sein. Dies ist das Verzeichnis, in dem Abonnements standardmäßig erstellt werden.

### <a name="rest"></a>[REST](#tab/rest-getEnrollments)

Fordern Sie eine Liste aller Registrierungskonten an, auf die Sie zugreifen können:

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/?api-version=2020-05-01
```

In der API-Antwort sind alle Registrierungskonten aufgelistet, auf die Sie Zugriff haben:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/1234567",
      "name": "1234567",
      "properties": {
        "accountStatus": "Unknown",
        "accountType": "Enterprise",
        "agreementType": "EnterpriseAgreement",
        "soldTo": {
          "companyName": "Contoso",
          "country": "US "
        },
        "billingProfiles": {
          "hasMoreResults": false
        },
        "displayName": "Contoso",
        "enrollmentAccounts": [
          {
            "id": "/providers/Microsoft.Billing/billingAccounts/1234567/enrollmentAccounts/7654321",
            "name": "7654321",
            "type": "Microsoft.Billing/enrollmentAccounts",
            "properties": {
              "accountName": "Contoso",
              "accountOwnerEmail": "kenny@contoso.onmicrosoft.com",
              "costCenter": "Test",
              "isDevTest": false
            }
          }
        ],
        "hasReadAccess": false
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}

```

Notieren Sie sich die ID (`id`) für eines Ihrer Registrierungskonten (`enrollmentAccounts`). Dies ist der Abrechnungsbereich, in dem eine Anforderung zur Abonnementerstellung initiiert wird. 

<!-- 
### [PowerShell](#tab/azure-powershell-getEnrollments)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.

-->


<!--
### [Azure CLI](#tab/azure-cli-getEnrollments)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Erstellen von Abonnements unter einem bestimmten Registrierungskonto

Im folgenden Beispiel wird ein Abonnement namens *Dev Team Subscription* in Registrierungskonto erstellt, das im vorherigen Schritt ausgewählt wurde. 

### <a name="rest"></a>[REST](#tab/rest-EA)

Rufen Sie die PUT-API auf, um eine Anforderung bzw. einen Alias für die Abonnementerstellung zu generieren.

```json
PUT  https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01 
```

Geben Sie im Anforderungstext die ID (`id`) eines Ihrer Registrierungskonten (`enrollmentAccounts`) als `billingScope` an.

```json 
{
  "properties": {
        "billingScope": "/providers/Microsoft.Billing/BillingAccounts/1234567/enrollmentAccounts/7654321",
        "DisplayName": "Dev Team Subscription", //Subscription Display Name
        "Workload": "Production"
  }
}
```

#### <a name="response"></a>Antwort

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

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-EA)

Führen Sie `Install-Module Az.Subscription` aus, um die neueste Version des Moduls zu installieren, das das Cmdlet `New-AzSubscriptionAlias` enthält. Eine aktuelle Version von PowerShellGet finden Sie unter [Abrufen des PowerShellGet-Moduls](/powershell/scripting/gallery/installing-psget).

Führen Sie den folgenden Befehl [New-AzSubscriptionAlias](/powershell/module/az.subscription/new-azsubscription) aus, und verwenden Sie dabei den Abrechnungsbereich `"/providers/Microsoft.Billing/BillingAccounts/1234567/enrollmentAccounts/7654321"`. 

```azurepowershell-interactive
New-AzSubscriptionAlias -AliasName "sampleAlias" -SubscriptionName "Dev Team Subscription" -BillingScope "/providers/Microsoft.Billing/BillingAccounts/1234567/enrollmentAccounts/7654321" -Workload 'Production"
```

Sie erhalten die Abonnement-ID (subscriptionId) als Teil der Befehlsantwort.

```azurepowershell
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  }
}
```

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli-EA)

Installieren Sie zunächst die Erweiterung, indem Sie `az extension add --name account` und `az extension add --name alias` ausführen.

Führen Sie den folgenden Befehl [az account alias create](/cli/azure/ext/account/account/alias?view=azure-cli-latest#ext_account_az_account_alias_create&preserve-view=true) aus, und geben Sie `billing-scope` und `id` von einem Ihrer Registrierungskonten (`enrollmentAccounts`) an. 

```azurecli-interactive
az account alias create --name "sampleAlias" --billing-scope "/providers/Microsoft.Billing/billingAccounts/1234567/enrollmentAccounts/654321" --display-name "Dev Team Subscription" --workload "Production"
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

### <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Einschränkungen der Azure Enterprise-Abonnementerstellungs-API

- Nur Azure Enterprise-Abonnements werden mithilfe der API erstellt.
- Pro Registrierungskonto sind maximal 2.000 Abonnements zulässig. Im Anschluss können weitere Abonnements für das Konto nur über das Azure-Portal erstellt werden. Wenn Sie über die API weitere Abonnements erstellen möchten, erstellen Sie ein weiteres Registrierungskonto.
- Benutzer, die keine Kontobesitzer sind, aber per Azure RBAC einem Registrierungskonto hinzugefügt wurden, können über das Azure-Portal keine Abonnements erstellen.
- Sie können den Mandanten nicht auswählen, in dem das Abonnement erstellt werden soll. Das Abonnement wird immer im Basismandanten des Kontobesitzers erstellt. Wenn Sie das Abonnement in einen anderen Mandanten verschieben möchten, finden Sie weitere Informationen unter [Ändern des Abonnementmandanten](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).


## <a name="create-subscriptions-for-an-mca-account"></a>Erstellen von Abonnements für ein Konto vom Typ „MCA“

Verwenden Sie die Informationen in den folgenden Abschnitten, um MCA-Abonnements zu erstellen.

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen über die Rolle „Besitzer“, „Mitwirkender“ oder „Azure-Abonnementersteller“ für einen Rechnungsabschnitt oder die Rolle „Besitzer“ oder „Mitwirkender“ für ein Abrechnungsprofil oder -konto verfügen, um Abonnements erstellen zu können. Weitere Informationen finden Sie unter [Rollen und Aufgaben für die Abonnementabrechnung](understand-mca-roles.md#subscription-billing-roles-and-tasks).

In den folgenden Beispielen werden REST-APIs verwendet. PowerShell und Azure CLI werden derzeit nicht unterstützt.

### <a name="find-billing-accounts-that-you-have-access-to"></a>Suchen nach Abrechnungskonten, auf die Sie Zugriff haben

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

<!--
### [PowerShell](#tab/azure-powershell-getBillingAccounts)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getBillingAccounts)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="find-billing-profiles--invoice-sections-to-create-subscriptions"></a>Suchen nach Abrechnungsprofilen und Rechnungsabschnitten zum Erstellen von Abonnements

Die Gebühren für Ihr Abonnement werden in einem Abschnitt der Rechnung eines Abrechnungsprofils angezeigt. Verwenden Sie die folgende API, um die Liste mit den Abrechnungsprofilen und Rechnungsabschnitten abzurufen, für die Sie über die Berechtigung zum Erstellen von Azure-Abonnements verfügen.

Zuerst erhalten Sie die Liste der Abrechnungsprofile unter dem Abrechnungskonto, auf das Sie Zugriff haben.

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

<!--
### [PowerShell](#tab/azure-powershell-getBillingProfiles)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getBillingProfiles)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="create-a-subscription-for-an-invoice-section"></a>Erstellen eines Abonnements für einen Rechnungsabschnitt

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

Führen Sie den folgenden Befehl [az account alias create](/cli/azure/ext/account/account/alias?view=azure-cli-latest#ext_account_az_account_alias_create&preserve-view=true) aus.

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

## <a name="create-subscriptions-for-an-mpa-billing-account"></a>Erstellen von Abonnements für ein Abrechnungskonto vom Typ „MPA“

Verwenden Sie die Informationen in den folgenden Abschnitten, um MPA-Abonnements zu erstellen.

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen im Cloudlösungsanbieter-Konto Ihrer Organisation über die Rolle „Globaler Administrator“ oder „Administrator-Agent“ verfügen, um ein Abonnement für Ihr Abrechnungskonto erstellen zu können. Weitere Informationen finden Sie unter [Zuweisen von Rollen und Berechtigungen zu Benutzern](/partner-center/permissions-overview).

In den folgenden Beispielen werden REST-APIs verwendet. PowerShell und Azure CLI werden derzeit nicht unterstützt.

### <a name="find-the-billing-accounts-that-you-have-access-to"></a>Suchen nach Abrechnungskonten, auf die Sie Zugriff haben

Verwenden Sie die folgende Anforderung, um alle Abrechnungskonten aufzulisten, auf die Sie Zugriff haben:

### <a name="rest"></a>[REST](#tab/rest-getBillingAccount-MPA)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/?api-version=2020-05-01
```

In der API-Antwort sind die Abrechnungskonten aufgeführt.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountStatus": "Active",
        "accountType": "Partner",
        "agreementType": "MicrosoftPartnerAgreement",
        "billingProfiles": {
          "hasMoreResults": false
        },
        "displayName": "Contoso",
        "hasReadAccess": true
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}
```

Verwenden Sie die `displayName`-Eigenschaft, um das Abrechnungskonto zu identifizieren, für das Sie Abonnements erstellen möchten. Vergewissern Sie sich, dass als „agreementType“ des Kontos die Option *MicrosoftPartnerAgreement* festgelegt ist. Kopieren Sie den Namen (`name`) für das Konto. Wenn Sie beispielsweise ein Abonnement für das Abrechnungskonto `Contoso` erstellen möchten, kopieren Sie `99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Halten Sie den Wert für den nächsten Schritt bereit.

<!--
### [PowerShell](#tab/azure-powershell-getBillingAccounts-MPA)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getBillingAccounts-MPA)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="find-customers-that-have-azure-plans"></a>Suchen nach Kunden mit Azure-Plänen

Senden Sie die folgende Anforderung mit dem im ersten Schritt (```99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) kopierten `name`-Element, um alle Kunden unter dem Abrechnungskonto aufzulisten, für die Sie Azure-Abonnements erstellen können.

### <a name="rest"></a>[REST](#tab/rest-getCustomers)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers?api-version=2020-05-01
```

In der API-Antwort sind die Kunden des Abrechnungskontos aufgelistet, die über Azure-Pläne verfügen. Sie können Abonnements für diese Kunden erstellen.

```json
{
  "totalCount": 2,
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Fabrikam toys Billing Profile",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
        "displayName": "Fabrikam toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Contoso toys Billing Profile",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
        "displayName": "Contoso toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    }
  ]
}

```

Verwenden Sie die `displayName`-Eigenschaft, um den Kunden zu identifizieren, für den Sie Abonnements erstellen möchten. Kopieren Sie die `id` für den Kunden. Wenn Sie beispielsweise ein Abonnement für `Fabrikam toys` erstellen möchten, kopieren Sie `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. Halten Sie diesen Wert für spätere Schritte bereit.

<!--
### [PowerShell](#tab/azure-powershell-getCustomers)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getCustomers)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="optional-for-indirect-providers-get-the-resellers-for-a-customer"></a>Optional für indirekte Anbieter: Abrufen der Handelspartner für einen Kunden

Falls Sie im Rahmen des zweistufigen Cloudlösungsanbieter-Modells als indirekter Anbieter fungieren, können Sie einen Handelspartner angeben, während Sie Abonnements für Kunden erstellen.

Senden Sie die folgende Anforderung mit dem im zweiten Schritt (```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopierten `id`-Element, um alle Handelspartner aufzulisten, die für einen Kunden verfügbar sind.

### <a name="rest"></a>[REST](#tab/rest-getIndirectResellers)

```json
GET "https://management.azure.com/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx?$expand=resellers&api-version=2020-05-01"
```
In der API-Antwort sind die Handelspartner für den Kunden aufgeführt:

```json
{
  "value": [{
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Billing/billingAccounts/customers",
  "properties": {
    "billingProfileDisplayName": "Fabrikam toys Billing Profile",
    "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
    "displayName": "Fabrikam toys",
    "resellers": [
      {
        "resellerId": "3xxxxx",
        "description": "Wingtip"
      }
    ]
  }
}]
}
```

Verwenden Sie die `description`-Eigenschaft, um den Handelspartner zu identifizieren, der dem Abonnement zugeordnet ist. Kopieren Sie die `resellerId` für den Handelspartner. Kopieren Sie `3xxxxx`, um beispielsweise `Wingtip` zuzuordnen. Halten Sie den Wert für den nächsten Schritt bereit.

<!--
### [PowerShell](#tab/azure-powershell-getIndirectResellers)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getIndirectResellers)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="create-a-subscription-for-a-customer"></a>Erstellen eines Abonnements für einen Kunden

Im folgenden Beispiel wird ein Abonnement mit dem Namen *Dev Team subscription* für *Fabrikam toys* erstellt, und anschließend wird dem Abonnement der Handelspartner *Wingtip* zugeordnet. Sie verwenden den kopierten Abrechnungsbereich aus dem vorherigen Schritt: `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. 

### <a name="rest"></a>[REST](#tab/rest-MPA)

```json
PUT  https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="request-body"></a>Anforderungstext

```json
{
  "properties":
    {
        "billingScope": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
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

Übergeben Sie das optionale Element *resellerId* , das Sie im zweiten Schritt in den Anforderungstext der API kopiert haben.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-MPA)

Führen Sie `Install-Module Az.Subscription` aus, um die neueste Version des Moduls zu installieren, das das Cmdlet `New-AzSubscriptionAlias` enthält. Eine aktuelle Version von PowerShellGet finden Sie unter [Abrufen des PowerShellGet-Moduls](/powershell/scripting/gallery/installing-psget).

Führen Sie den folgenden Befehl [New-AzSubscriptionAlias](/powershell/module/az.subscription/new-azsubscription) aus, und verwenden Sie dabei den Abrechnungsbereich `"/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx"`. 

```azurepowershell-interactive
New-AzSubscriptionAlias -AliasName "sampleAlias" -SubscriptionName "Dev Team Subscription" -BillingScope "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx" -Workload 'Production"
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

Übergeben Sie das optionale Element *resellerId* , das Sie im zweiten Schritt in den Aufruf von `New-AzSubscriptionAlias` kopiert haben.

### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli-MPA)

Installieren Sie zunächst die Erweiterung, indem Sie `az extension add --name account` und `az extension add --name alias` ausführen.

Führen Sie den folgenden Befehl [az account alias create](/cli/azure/ext/account/account/alias?view=azure-cli-latest#ext_account_az_account_alias_create&preserve-view=true) aus. 

```azurecli-interactive
az account alias create --name "sampleAlias" --billing-scope "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --display-name "Dev Team Subscription" --workload "Production"
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

Übergeben Sie das optionale Element *resellerId* , das Sie im zweiten Schritt in den Aufruf von `az account alias create` kopiert haben.

---

## <a name="create-subscriptions-using-arm-templates"></a>Erstellen von Abonnements mithilfe von ARM-Vorlagen

Sie können Abonnements für Ihre Azure Resource Manager-Vorlage (ARM-Vorlage) erstellen, sodass Sie Ihre Produktions-/Testbereitstellungsprozesse automatisieren können. Im folgenden Beispiel verwenden Sie eine ARM-Vorlage zum Erstellen eines Azure-Abonnements und einer Azure-Ressourcengruppe.

### <a name="prerequisites"></a>Voraussetzungen

Sie müssen über die Rolle „Besitzer“, „Mitwirkender“ oder „Azure-Abonnementersteller“ für einen Rechnungsabschnitt oder die Rolle „Besitzer“ oder „Mitwirkender“ für ein Abrechnungsprofil oder -konto verfügen, um Abonnements erstellen zu können. Weitere Informationen finden Sie unter [Rollen und Aufgaben für die Abonnementabrechnung](understand-mca-roles.md#subscription-billing-roles-and-tasks).

Da Sie eine ARM-Vorlagenbereitstellung durchführen, benötigen Sie darüber hinaus Schreibberechtigungen für das Stammobjekt. Wenn Sie also die ARM-Bereitstellung unter einer Verwaltungsgruppe erstellen, müssen Sie über Schreibberechtigungen für die Verwaltungsgruppe verfügen. Beachten Sie, dass die Aktion ausschließlich zum Erstellen einer ARM-Bereitstellung dient: Wenn ein Abonnement erstellt wird, wird es nur in der in der ARM-Vorlage angegebenen Verwaltungsgruppe erstellt.

In den folgenden Beispielen werden REST-APIs verwendet. PowerShell und Azure CLI werden derzeit nicht unterstützt.

### <a name="find-billing-accounts-that-you-have-access-to"></a>Suchen nach Abrechnungskonten, auf die Sie Zugriff haben

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

Verwenden Sie die `displayName`-Eigenschaft, um das Abrechnungskonto zu identifizieren, für das Sie Abonnements erstellen möchten. Vergewissern Sie sich, dass als „agreementType“ des Kontos die Option *MicrosoftCustomerAgreement* festgelegt ist. Kopieren Sie den Namen (`name`) des Kontos. Wenn Sie beispielsweise ein Abonnement für das Abrechnungskonto `Contoso` erstellen möchten, kopieren Sie `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Halten Sie den Wert für den nächsten Schritt bereit.

<!--
### [PowerShell](#tab/azure-powershell-getBillingAccounts)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getBillingAccounts)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="find-billing-profiles--invoice-sections-to-create-subscriptions"></a>Suchen nach Abrechnungsprofilen und Rechnungsabschnitten zum Erstellen von Abonnements

Die Gebühren für Ihr Abonnement werden in einem Abschnitt der Rechnung eines Abrechnungsprofils angezeigt. Verwenden Sie die folgende API, um die Liste mit den Abrechnungsprofilen und Rechnungsabschnitten abzurufen, für die Sie über die Berechtigung zum Erstellen von Azure-Abonnements verfügen.

Zuerst erhalten Sie die Liste der Abrechnungsprofile unter dem Abrechnungskonto, auf das Sie Zugriff haben.

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

<!--
### [PowerShell](#tab/azure-powershell-getBillingProfiles)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.

### [Azure CLI](#tab/azure-cli-getBillingProfiles)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="create-a-subscription-and-resource-group-with-a-template"></a>Erstellen eines Abonnements und einer Ressourcengruppe mit einer Vorlage

Die folgende ARM-Vorlage erstellt ein Abonnement mit dem Namen *Dev Team subscription* für den Rechnungsabschnitt *Development* . Die Kosten für das Abonnement werden dem Abrechnungsprofil *Contoso Billing Profile* berechnet und im Abschnitt *Development* der Rechnung aufgeführt. Sie verwenden den kopierten Abrechnungsbereich aus dem vorherigen Schritt: `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx`. 

#### <a name="request"></a>Anforderung

```rest
PUT https://management.azure.com/providers/Microsoft.Resources/deployments/sampleTemplate?api-version=2019-10-01
```

#### <a name="request-body"></a>Anforderungstext

```json
{
  "properties":
    {
        "location": "westus",
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {
                    "uniqueAliasName": "sampleAlias"
                },
                "resources": [
                    {
                        "type": "Microsoft.Resources/deployments",
                        "apiVersion": "2019-10-01",
                        "name": "sampleTemplate",
                        "location": "westus",
                        "properties": {
                            "expressionEvaluationOptions": {
                                "scope": "inner"
                            },
                            "mode": "Incremental",
                            "template": {
                                  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                                "contentVersion": "1.0.0.0",
                                "variables": {
                                    "uniqueAliasName": "sampleAlias"
                                },
                                "resources": [
                                    {
                                        "name": "[variables('uniqueAliasName')]",
                                        "type": "Microsoft.Subscription/aliases",
                                        "apiVersion": "2020-09-01",
                                        "properties": {
                                            "workLoad": "Production",
                                            "displayName": "Dev Team subscription",
                                            "billingScope": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx"
                                        },
                                        "dependsOn": [],
                                        "tags": {}
                                    }
                                ],
                                "outputs": {
                                    "subscriptionId": {
                                        "type": "string",
                                        "value": "[replace(reference(variables('uniqueAliasName')).subscriptionId, 'invalidrandom/', '')]"
                                    }
                                }
                            }
                        }
                    },
                    {
                        "name": "sampleOuterResource",
                        "type": "Microsoft.Resources/deployments",
                        "apiVersion": "2019-10-01",
                        "location": "westus",
                        "properties": {
                            "expressionEvaluationOptions": {
                                "scope": "inner"
                            },
                            "mode": "Incremental",
                            "parameters": {
                                "subscriptionId": {
                                    "value": "[reference('sampleTemplate').outputs.subscriptionId.value]"
                                }
                            },
                            "template": {
                                "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                            "contentVersion": "1.0.0.0",
                                "parameters": {
                                    "subscriptionId": {
                                        "type": "string"
                                    }
                                },
                                "variables": {},
                                "resources": [
                                    {
                                        "name": "sampleInnerResource",
                                        "type": "Microsoft.Resources/deployments",
                                        "subscriptionId": "[parameters('subscriptionId')]",
                                        "apiVersion": "2019-10-01",
                                        "location": "westus",
                                        "properties": {
                                            "expressionEvaluationOptions": {
                                                "scope": "inner"
                                            },
                                            "mode": "Incremental",
                                            "parameters": {},
                                            "template": {
                                                "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                                                "contentVersion": "1.0.0.0",
                                                "parameters": {},
                                                "variables": {},
                                                "resources": [
                                                    {
                                                        "type": "Microsoft.Resources/resourceGroups",
                                                        "apiVersion": "2020-05-01",
                                                        "location": "[deployment().location]",
                                                        "name": "sampleRG",
                                                        "properties": {},
                                                        "tags": {}
                                                    }
                                                ],
                                                "outputs": {}
                                            }
                                        }
                                    }
                                ],
                                "outputs": {}
                            }
                        }
                    }
                ],
                "outputs": {
                    "messageFromLinkedTemplate": {
                        "type": "string",
                        "value": "[reference('sampleTemplate').outputs.subscriptionId.value]"
                    }
                }
            },
            "mode": "Incremental"
        }
    }
}
```

#### <a name="response"></a>Antwort

```json
{
  "id": "/providers/Microsoft.Resources/deployments/sampleTemplate",
  "name": "sampleTemplate",
  "type": "Microsoft.Resources/deployments",
  "location": "westus",
  "properties": {
    "templateHash": "16005880870587497948",
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted",
    "timestamp": "2020-10-07T19:06:34.110811Z",
    "duration": "PT0.1345459S",
    "correlationId": "2b57ddf6-7e27-42cb-90b4-90eeccd11a28",
    "providers": [
      {
        "namespace": "Microsoft.Resources",
        "resourceTypes": [
          {
            "resourceType": "deployments",
            "locations": [
              "westus"
            ]
          }
        ]
      }
    ],
    "dependencies": [
      {
        "dependsOn": [
          {
            "id": "/providers/Microsoft.Resources/deployments/sampleTemplate",
            "resourceType": "Microsoft.Resources/deployments",
            "resourceName": "anuragTemplate1"
          }
        ],
        "id": "/providers/Microsoft.Resources/deployments/sampleOuterResource",
        "resourceType": "Microsoft.Resources/deployments",
        "resourceName": "sampleOuterResource"
      }
    ]
  }
}
```

Mithilfe von GET können Sie den Status der Bereitstellung abrufen, um den Fortschritt zu überwachen.

```json
GET https://management.azure.com/providers/Microsoft.Resources/deployments/sampleTemplate?api-version=2019-10-01
```

#### <a name="response"></a>Antwort

```json
{
  "id": "/providers/Microsoft.Resources/deployments/sampleDeployment5",
  "name": "sampleDeployment5",
  "type": "Microsoft.Resources/deployments",
  "location": "westus",
  "properties": {
    "templateHash": "16005880870587497948",
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Succeeded",
    "timestamp": "2020-10-07T19:07:20.8007311Z",
    "duration": "PT46.824466S",
    "correlationId": "2b57ddf6-7e27-42cb-90b4-90eeccd11a28",
    "providers": [
      {
        "namespace": "Microsoft.Resources",
        "resourceTypes": [
          {
            "resourceType": "deployments",
            "locations": [
              "westus"
            ]
          }
        ]
      }
    ],
    "dependencies": [
      {
        "dependsOn": [
          {
            "id": "/providers/Microsoft.Resources/deployments/sampleTemplate",
            "resourceType": "Microsoft.Resources/deployments",
            "resourceName": "sampleTemplate"
          }
        ],
        "id": "/providers/Microsoft.Resources/deployments/sampleOuterResource",
        "resourceType": "Microsoft.Resources/deployments",
        "resourceName": "sampleOuterResource"
      }
    ],
    "outputs": {
      "messageFromLinkedTemplate": {
        "type": "String",
        "value": "16edf959-11fd-48bb-9a46-85190963ead9"
      }
    },
    "outputResources": [
      {
        "id": "/providers/Microsoft.Subscription/aliases/sampleAlias"
      },
      {
        "id": "/subscriptions/16edf959-11fd-48bb-9a46-85190963ead9/resourceGroups/sampleRG"
      }
    ]
  }
}
```

Im vorherigen Beispiel sehen Sie, dass das Abonnement `16edf959-11fd-48bb-9a46-85190963ead9` und die Ressourcengruppe `sampleRG` erstellt wurden.

## <a name="next-steps"></a>Nächste Schritte

* Da Sie nun ein Abonnement erstellt haben, können Sie diese Möglichkeit auch für andere Benutzer und Dienstprinzipale eröffnen. Weitere Informationen finden Sie unter [Gewähren des Zugriffs zum Erstellen von Azure Enterprise-Abonnements (Vorschau) ](grant-access-to-create-subscription.md).
* Weitere Informationen zum Verwalten einer großen Anzahl von Abonnements mithilfe von Verwaltungsgruppen finden Sie unter [Was sind Azure-Verwaltungsgruppen?](../../governance/management-groups/overview.md).
