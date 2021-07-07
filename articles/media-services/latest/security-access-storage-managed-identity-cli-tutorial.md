---
title: Zugreifen auf Speicher mit einer verwalteten Media Services-Identität
description: Wenn Sie auf ein Speicherkonto zugreifen möchten und das Speicherkonto so konfiguriert ist, dass Anforderungen von unbekannten IP-Adressen blockiert werden, muss dem Media Services-Konto Zugriff auf das Speicherkonto gewährt werden. Führen Sie die folgenden Schritte aus, um mithilfe der Media Services-CLI eine verwaltete Identität für das Media Services-Konto zu erstellen und dieser Identität Speicherzugriff zu gewähren.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: tutorial
ms.date: 05/17/2021
ms.author: inhenkel
ms.openlocfilehash: 77eb737a48213701e424bbc8b1a49ead0c95e88c
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110464289"
---
# <a name="tutorial-access-storage-with-a-media-services-managed-identity"></a>Tutorial: Zugreifen auf Speicher mit einer verwalteten Media Services-Identität

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Wenn Sie auf ein Speicherkonto zugreifen möchten und das Speicherkonto so konfiguriert ist, dass Anforderungen von unbekannten IP-Adressen blockiert werden, muss dem Media Services-Konto Zugriff auf das Speicherkonto gewährt werden. Führen Sie die folgenden Schritte aus, um mithilfe der Media Services-CLI eine verwaltete Identität für das Media Services-Konto zu erstellen und dieser Identität Speicherzugriff zu gewähren.

:::image type="content" source="media/diagrams/managed-identities-scenario-storage-permissions-media-services-account.svg" alt-text="Verwendung einer verwalteten Identität für den Speicherzugriff durch das Media Services-Konto":::

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

## <a name="grant-the-media-services-managed-identity-access-to-the-storage-account"></a>Gewähren von Speicherkontozugriff für die verwaltete Media Services-Identität

Gewähren Sie der verwalteten Media Services-Identität Zugriff auf das Speicherkonto. Es gibt drei Befehle:

### <a name="get-show-the-managed-identity-of-the-media-services-account"></a>Abrufen (Anzeigen) der verwalteten Identität des Media Services-Kontos

Der erste Befehl zeigt die verwaltete Identität des Media Services-Kontos an. Hierbei handelt es sich um die Prinzipal-ID (`principalId`) aus der JSON-Rückgabe des Befehls.

[!INCLUDE [Show the Managed Identity of a Media Services account with the CLI](./includes/task-show-account-managed-identity-cli.md)]

### <a name="create-the-storage-blob-contributor-role-assignment"></a>Erstellen der Rollenzuweisung vom Typ „Mitwirkender an Storage-Blobdaten“

[!INCLUDE [Create the Storage Blob Contributor role assignment](./includes/task-create-storage-blob-contributor-role-cli.md)]

### <a name="create-the-reader-role-assignment"></a>Erstellen der Rollenzuweisung vom Typ „Leser“

[!INCLUDE [Create the Reader role assignment](./includes/task-create-reader-role-cli.md)]

## <a name="use-the-managed-identity-to-access-the-storage-account"></a>Zugreifen auf das Speicherkonto mithilfe der verwalteten Identität

[!INCLUDE [Use the Managed Identity to access the Storage account](./includes/task-set-storage-managed-identity-cli.md)]

## <a name="validation"></a>Überprüfung

Überprüfen Sie anhand der Kontoverschlüsselungseigenschaften, ob das Konto mit einem kundenseitig verwalteten Schlüssel verschlüsselt ist:

[!INCLUDE [Set Media Services to use the key from Key Vault](./includes/task-show-account-managed-identity-cli.md)]

Für die Eigenschaft `storageAuthentication` sollte „ManagedIdentity“ angezeigt werden.

Zur weiteren Überprüfung können Sie anhand der Azure Storage-Protokolle ermitteln, welche Authentifizierungsmethode für die einzelnen Anforderungen verwendet wird.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die von Ihnen erstellten Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe.

[!INCLUDE [Create a Media Services account with the CLI](./includes/clean-up-resources-cli.md)]
