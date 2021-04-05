---
title: Erstellen von Benutzeroberflächendefinitions-Verweisfunktionen
description: Hier werden die Funktionen beschrieben, mit denen Benutzeroberflächendefinitionen für das Azure-Portal erstellt werden, die auf andere Objekte verweisen.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: ad21c5b34a58c35b2cef5e430be7cb8cd1296402
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87094441"
---
# <a name="createuidefinition-referencing-functions"></a>CreateUiDefinition-Verweisfunktionen

Die Funktionen werden verwendet, um auf Ausgaben der Eigenschaften oder des Kontexts einer CreateUiDefinition-Datei zu verweisen.

## <a name="basics"></a>basics

Gibt die Ausgabewerte eines Elements zurück, das im Schritt [Basics](create-uidefinition-overview.md#basics) definiert ist. Übergeben Sie den Namen des Elements als Parameter an diese Funktion.

Wenn Sie die Ausgabewerte von Elementen in anderen Schritten abrufen möchten, verwenden Sie die [steps()](#steps)-Funktion.

Im folgenden Beispiel wird die Ausgabe des Elements mit dem Namen `clusterName` im Schritt „Basics“ zurückgegeben:

```json
"[basics('clusterName')]"
```

Die zurückgegebenen Werte variieren je nach Typ des abgerufenen Elements.

## <a name="location"></a>location

Gibt den im Schritt „Basics“ oder im aktuellen Kontext ausgewählten Standort zurück.

Im folgenden Beispiel wird ein Wert wie `"westus"` zurückgegeben:

```json
"[location()]"
```

## <a name="resourcegroup"></a>resourceGroup

Gibt Details zu der im Schritt „Basics“ oder im aktuellen Kontext ausgewählten Ressourcengruppe („resourceGroup“) zurück.

Im folgenden Beispiel:

```json
"[resourceGroup()]"
```

Gibt die folgenden Eigenschaften zurück:

```json
{
    "mode": "New" or "Existing",
    "name": "{resourceGroupName}",
    "location": "{resourceGroupLocation}"
}
```

Sie können jeden spezifischen Wert mit der Punktnotation abrufen.

```json
"[resourceGroup().name]"
```

## <a name="steps"></a>steps

Gibt die Elemente eines angegebenen Schritts zurück. Übergeben Sie den Namen des Schritts als Parameter an diese Funktion. Aus den zurückgegebenen Elementen können Sie bestimmte Eigenschaftswerte abrufen.

Wenn Sie die Ausgabewerte von Elementen im Schritt „Basics“ abrufen möchten, verwenden Sie die [basics()](#basics)-Funktion.

Das folgende Beispiel gibt den Schritt namens `vmParameters` zurück. In diesem Schritt gibt es ein Element namens `adminUsername`.

```json
"[steps('vmParameters').adminUsername]"
```

## <a name="subscription"></a>Abonnement

Gibt Eigenschaften für das im Schritt „Basics“ oder im aktuellen Kontext ausgewählte Abonnement zurück.

Im folgenden Beispiel:

```json
"[subscription()]"
```

Gibt die folgenden Eigenschaften zurück:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in die Entwicklung der Portaloberfläche finden Sie unter [Die Datei „CreateUiDefinition.json“ für die Benutzeroberfläche zum Erstellen verwalteter Azure-Anwendungen](create-uidefinition-overview.md).
