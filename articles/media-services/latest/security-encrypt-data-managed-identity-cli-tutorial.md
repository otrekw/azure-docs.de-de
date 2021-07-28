---
title: Verschlüsseln von Daten in einem Media Services-Konto mithilfe eines Key Vault-Schlüssels
description: Wenn von Media Services Daten mithilfe eines Schlüssels aus Ihrer Key Vault-Instanz verschlüsselt werden sollen, muss dem Media Services-Konto *Zugriff* auf die Key Vault-Instanz gewährt werden. Führen Sie die folgenden Schritte aus, um mithilfe der Media Services-CLI eine verwaltete Identität für das Media Services-Konto zu erstellen und dieser Identität Key Vault-Zugriff zu gewähren.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: tutorial
ms.date: 05/14/2021
ms.author: inhenkel
ms.openlocfilehash: 20b0ec0025fcac72005eb8b2343ede0ef9e5d9b4
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110464325"
---
# <a name="tutorial-use-a-key-vault-key-to-encrypt-data-into-a-media-services-account"></a>Tutorial: Verschlüsseln von Daten in einem Media Services-Konto mithilfe eines Key Vault-Schlüssels

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Wenn von Media Services Daten mithilfe eines Schlüssels aus Ihrer Key Vault-Instanz verschlüsselt werden sollen, muss dem Media Services-Konto *Zugriff* auf die Key Vault-Instanz gewährt werden. Führen Sie die folgenden Schritte aus, um mithilfe der Media Services-CLI eine verwaltete Identität für das Media Services-Konto zu erstellen und dieser Identität Zugriff auf Ihre Key Vault-Instanz zu gewähren.

:::image type="content" source="media/diagrams/managed-identities-scenario-keyvault-media-services-account.svg" alt-text="Verwendung von Key Vault durch ein Media Services-Konto mit verwalteter Identität":::



In diesem Tutorial wird die Media Services-API 2020-05-01 verwendet.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Um die Befehle in diesem Artikel verwenden zu können, müssen Sie bei dem Abonnement angemeldet sein, das Sie verwenden möchten.

 [!INCLUDE [Sign in to Azure with the CLI](./includes/task-sign-in-azure-cli.md)]

### <a name="set-subscription"></a>Festlegen des Abonnements

Verwenden Sie den folgenden Befehl, um das Abonnement festzulegen, mit dem Sie arbeiten möchten.

[!INCLUDE [Sign in to Azure with the CLI](./includes/task-set-azure-subscription-cli.md)]

## <a name="resource-names"></a>Ressourcennamen

Überlegen Sie sich zunächst, wie die von Ihnen erstellten Ressourcen heißen sollen.  Sie sollten problemlos als Gruppe identifizierbar sein – insbesondere, wenn Sie nicht vorhaben, sie nach dem Testen weiter zu verwenden. Da sich Benennungsregeln für viele Ressourcentypen unterscheiden, empfiehlt es sich, nur Kleinbuchstaben zu verwenden. Beispiel: „mediatest1rg“ für den Namen Ihrer Ressourcengruppe und „mediatest1stor“ für den Namen Ihres Speicherkontos. Verwenden Sie für jeden Schritt in diesem Artikel die gleichen Namen.

Auf diese Namen wird in den Befehlen weiter unten verwiesen.  Sie benötigen Namen für folgende Ressourcen:

- „your-resource-group-name“ (Name Ihrer Ressourcengruppe)
- „your-storage-account-name“ (Name Ihres Speicherkontos)
- „your-media-services-account-name“ (Name Ihres Media Services-Kontos)
- „your-keyvault-name“ (Name Ihres Schlüsseltresors)
- „your-key-name“ (Name Ihres Schlüssels)
- „your-region“ (Ihre Region)

> [!NOTE]
> Die Bindestriche dienen lediglich zur besseren Lesbarkeit. Verwenden Sie aufgrund der Inkonsistenz bei der Benennung von Ressourcen in Azure-Diensten keine Bindestriche in Ihren Ressourcennamen.
> Der Regionsname wird außerdem nicht von Ihnen erstellt,  sondern von Azure bestimmt.

### <a name="list-azure-regions"></a>Auflisten der Azure-Regionen

Falls Sie nicht genau wissen, welchen Regionsnamen Sie verwenden sollen, können Sie mithilfe des folgenden Befehls eine Liste abrufen:

[!INCLUDE [List Azure regions with the CLI](./includes/task-list-azure-regions-cli.md)]

## <a name="sequence"></a>Sequenz

Die folgenden Schritte werden in einer bestimmten Reihenfolge ausgeführt, da jeweils einzelne oder mehrere Werte aus den JSON-Antworten im nächsten Schritt der Sequenz verwendet werden.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Die von Ihnen erstellten Ressourcen müssen einer Ressourcengruppe angehören. Erstellen Sie zuerst die Ressourcengruppe. Für die Erstellung des Media Services-Kontos und in nachfolgenden Schritten wird `your-resource-group-name` verwendet.

[!INCLUDE [Create a resource group with the CLI](./includes/task-create-resource-group-cli.md)]

## <a name="create-a-storage-account"></a>Erstellen eines Speicherkontos

Dem von Ihnen erstellten Media Services-Konto muss ein Speicherkonto zugeordnet sein. Erstellen Sie zuerst das Speicherkonto für das Media Services-Konto. In nachfolgenden Schritten wird `your-storage-account-name` verwendet.

[!INCLUDE [Create a Storage account with the CLI](./includes/task-create-storage-account-cli.md)]

## <a name="create-a-media-services-account-with-a-service-principal-managed-identity"></a>Erstellen eines Media Services-Kontos mit einem Dienstprinzipal (verwaltete Identität)

Erstellen Sie nun das Media Services-Konto mit einem Dienstprinzipal. Dieser wird auch als verwaltete Identität bezeichnet.

> [!IMPORTANT]
> Wichtig: In dem Befehl muss das Flag „--mi“ verwendet werden.  Andernfalls finden Sie die für einen späteren Schritt benötigte Prinzipal-ID (`principalId`) nicht.

[!INCLUDE [Create a Media Services account with the CLI](./includes/task-create-media-services-account-managed-identity-cli.md)]

## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors

Erstellen Sie die Key Vault-Instanz.  Die Key Vault-Instanz wird zum Verschlüsseln von Mediendaten verwendet. `your-keyvault-name` wird für die Schlüsselerstellung sowie für spätere Schritte verwendet.

[!INCLUDE [Create a Media Services account with the CLI](./includes/task-create-key-vault-cli.md)]

## <a name="grant-the-media-services-system-assigned-managed-identity-access-to-the-key-vault"></a>Gewähren von Zugriff auf die Key Vault-Instanz für die systemseitig zugewiesene verwaltete Media Services-Identität

Gewähren Sie der verwalteten Media Services-Identität Zugriff auf die Key Vault-Instanz. Es gibt zwei Befehle:

### <a name="get-show-the-managed-identity-of-the-media-services-account"></a>Abrufen (Anzeigen) der verwalteten Identität des Media Services-Kontos

Der erste Befehl zeigt die verwaltete Identität des Media Services-Kontos an. Hierbei handelt es sich um die Prinzipal-ID (`principalId`) aus der JSON-Rückgabe des Befehls.

[!INCLUDE [Show the Managed Identity of a Media Services account with the CLI](./includes/task-show-account-managed-identity-cli.md)]

### <a name="set-the-key-vault-policy"></a>Festlegen der Key Vault-Richtlinie

Durch den zweiten Befehl wird der Prinzipal-ID Zugriff auf Key Vault gewährt. Legen Sie `object-id` auf den Wert von `principalId` fest, den Sie im vorherigen Schritt erhalten haben.

[!INCLUDE [Set the Key Vault policy with the CLI](./includes/task-set-key-vault-policy-cli.md)]

### <a name="set-media-services-to-use-the-key-from-key-vault"></a>Konfigurieren von Media Services für die Verwendung des Schlüssels aus Key Vault

Konfigurieren Sie Media Services für die Verwendung des erstellten Schlüssels. Der Wert der Eigenschaft `key-identifier` stammt aus der Ausgabe der Schlüsselerstellung. Wenn das Auffüllen der Zugriffssteuerungsänderungen zu lange dauert, kann es vorkommen, dass der Befehl nicht erfolgreich ausgeführt wird. Versuchen Sie es in diesem Fall nach einigen Minuten erneut.

[!INCLUDE [Set Media Services to use the key from Key Vault](./includes/task-set-encryption-cli.md)]

## <a name="validation"></a>Überprüfung

Überprüfen Sie anhand der Kontoverschlüsselungseigenschaften, ob das Konto mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist:

[!INCLUDE [Set Media Services to use the key from Key Vault](./includes/task-show-account-encryption-cli.md)]

Für die Eigenschaft `type` sollte `CustomerKey` angezeigt werden, und der aktuelle Schlüsselbezeichner (`currentKeyIdentifier`) sollte auf den Pfad eines Schlüssels in der Key Vault-Instanz des Kunden festgelegt sein.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die von Ihnen erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe.

[!INCLUDE [Create a Media Services account with the CLI](./includes/clean-up-resources-cli.md)]
