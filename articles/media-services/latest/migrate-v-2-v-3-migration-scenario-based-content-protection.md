---
title: Migrationsleitfaden für den Inhaltsschutz
description: In diesem Artikel finden Sie einen szenariobasierten Migrationsleitfaden für den Inhaltsschutz, der Sie bei der Migration von Azure Media Services v2 zu v3 unterstützt.
services: media-services
author: IngridAtMicrosoft
manager: femila
ms.service: media-services
ms.topic: conceptual
ms.workload: media
ms.date: 04/05/2021
ms.author: inhenkel
ms.openlocfilehash: 59f74740117ba9f549133c4ca9bcb510928eb105
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108138875"
---
# <a name="content-protection-scenario-based-migration-guidance"></a>Szenariobasierter Migrationsleitfaden für den Inhaltsschutz

![Logo des Migrationsleitfadens](./media/migration-guide/azure-media-services-logo-migration-guide.svg)

<hr color="#5ea0ef" size="10">

![Schritte bei der Migration 2](./media/migration-guide/steps-4.svg)

Dieser Artikel enthält ausführliche Informationen und Anleitungen zur Migration von Inhaltsschutz-Anwendungsfällen von der v2-API bis zur neuen Azure Media Services v3-API.

## <a name="protect-content-in-v3-api"></a>Inhaltsschutz in der V3-API

Verwenden Sie die Unterstützung für Funktionen mit [mehreren Schlüsseln](architecture-design-multi-drm-system.md) in der neuen V3-API.

Die zugehörigen Schritte finden Sie in den Konzepten, Tutorials und Schrittanleitungen zum Inhaltsschutz am Ende dieses Artikels.

> [!NOTE]
> Im weiteren Verlauf dieses Artikels wird erläutert, wie Sie Ihren v2-Inhaltsschutz mit .NET zu v3 migrieren können.  Wenn Sie Anweisungen oder Beispielcode für eine andere Sprache oder Methode benötigen, erstellen Sie ein GitHub-Problem für diese Seite.

## <a name="v3-visibility-of-v2-assets-streaminglocators-and-properties"></a>v3-Sichtbarkeit von v2-Ressourcen, StreamingLocators und Eigenschaften

In der v2-API wurden `Assets`, `StreamingLocators` und `ContentKeys` verwendet, um Ihre Streaminginhalte zu schützen. Wenn Sie zur v3-API migrieren, werden Ihre v2-API-`Assets`, -`StreamingLocators` und -`ContentKeys` automatisch in der v3-API verfügbar gemacht, und Sie können auf alle auf ihnen befindlichen Daten zugreifen.

Allerdings können Sie keine Eigenschaften von v2-Entitäten über die v3-API *aktualisieren*, die in v2 erstellt wurden.

Wenn Sie in v2-Entitäten gespeicherte Inhalte aktualisieren oder ändern müssen, aktualisieren Sie diese mit der v2-API, oder erstellen Sie neue v3-API-Entitäten, um sie zu migrieren.

## <a name="asset-identifier-differences"></a>Unterschiede bei Ressourcenbezeichnern

Um zu migrieren, müssen Sie auf Eigenschaften oder Inhaltsschlüssel aus Ihren v2-Ressourcen zugreifen.  Sie müssen wissen, dass die v2-API die `AssetId` als primären Identifikationsschlüssel verwendet, aber die neue v3-API den *Azure-Ressourcenverwaltungsnamen* der Entität als primären Bezeichner verwendet.  (Die v2-`Asset.Name`-Eigenschaft wird nicht als eindeutiger Bezeichner verwendet.) Mit der v3-API wird der v2-Ressourcenname nun als `Asset.Description` angezeigt.

Wenn Sie z. B. zuvor über eine v2-Ressource mit der ID `nb:cid:UUID:8cb39104-122c-496e-9ac5-7f9e2c2547b8` verfügten, befindet sich der Bezeichner nun am Ende der GUID `8cb39104-122c-496e-9ac5-7f9e2c2547b8`. Dies wird angezeigt, wenn Sie Ihre v2-Ressourcen über die v3-API auflisten.

Alle Ressourcen, die mit der v2-API erstellt und veröffentlicht wurden, verfügen in der v3-API sowohl über eine `ContentKeyPolicy` als auch einen `ContentKey` anstelle einer standardmäßigen Inhaltsschlüsselrichtlinie in der `StreamingPolicy`.

Weitere Informationen finden Sie in den Dokumentationen [Richtlinien für Inhaltsschlüssel](./drm-content-key-policy-concept.md) und [Streamingrichtlinien](./stream-streaming-policy-concept.md).

## <a name="use-azure-media-services-explorer-amse-v2-and-amse-v3-tools-side-by-side"></a>Parallele Verwendung von Azure Media Services Explorer (AMSE) v2- und AMSE v3-Tools

Verwenden Sie das [v2 Azure Media Services Explorer-Tool](https://github.com/Azure/Azure-Media-Services-Explorer/releases/tag/v4.3.15.0) zusammen mit dem [v3 Azure Media Services Explorer-Tool](https://github.com/Azure/Azure-Media-Services-Explorer), um die Daten für eine über v2-APIs erstellte und veröffentlichte Ressource nebeneinander zu vergleichen. Alle Eigenschaften sollten sichtbar sein, aber an verschiedenen Orten.

## <a name="use-the-net-content-protection-migration-sample"></a>Verwenden des .NET-Inhaltsschutz-Migrationsbeispiels

Ein Codebeispiel zum Vergleichen der Unterschiede in Ressourcenbezeichnern mit [v2tov3MigrationSample](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/ContentProtection/v2tov3Migration) finden Sie unter ContentProtection in den Media Services-Codebeispielen.

## <a name="list-the-streaming-locators"></a>Auflisten der Streaminglocators

Sie können die `StreamingLocators`, die mit den in der v2-API mit der neuen v3-Methode [ListStreamingLocators](/rest/api/media/assets/liststreaminglocators) erstellten Ressourcen verknüpft sind, auf der Ressourcenentität abfragen.  Verweisen Sie auch auf die .NET-Client-SDK-Version von [ListStreamingLocatorsAsync](/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocatorsasync?preserve-view=true&view=azure-dotnet).

Die Ergebnisse der `ListStreamingLocators`-Methode stellen für Sie den `Name` und die `StreamingLocatorId` des Locators zusammen mit dem `StreamingPolicyName` bereit.

## <a name="find-the-content-keys"></a>Suchen der Inhaltsschlüssel

Um die mit Ihren `StreamingLocators` verwendeten `ContentKeys` zu suchen, können Sie die [StreamingLocator.ListContentKeysAsync](/dotnet/api/microsoft.azure.management.media.streaminglocatorsoperationsextensions.listcontentkeysasync?preserve-view=true&view=azure-dotnet)-Methode aufrufen.  

Weitere Informationen zum Inhaltsschutz in der v3-API finden Sie im Artikel [Schützen Sie Ihren Inhalt mit der dynamischen Verschlüsselung von Media Services](./drm-content-protection-concept.md).

## <a name="change-the-v2-contentkeypolicy-keeping-the-same-contentkey"></a>Ändern der v2-ContentKeyPolicy unter Beibehalten desselben ContentKey-Werts

Sie sollten zuerst die Veröffentlichung auf der Ressource über das v2-SDK aufheben (alle Streaminglocators entfernen). Gehen Sie dabei folgendermaßen vor:

1. Löschen Sie den Locator.
1. Heben Sie die Verknüpfung der `ContentKeyAuthorizationPolicy` auf.
1. Heben Sie die Verknüpfung der `AssetDeliveryPolicy` auf.
1. Heben Sie die Verknüpfung des `ContentKey` auf.
1. Löschen Sie den `ContentKey`.
1. Erstellen Sie einen neuen `StreamingLocator` in v3 mit einer v3-`StreamingPolicy` und einer v3-`ContentKeyPolicy`, und geben Sie dabei den spezifischen Inhaltsschlüsselbezeichner und den erforderlichen Schlüsselwert an.

> [!NOTE]
> Es ist möglich, den v2-Locator mit der v3-API zu löschen, aber dadurch wird der Inhaltsschlüssel oder die Inhaltsschlüsselrichtlinie nicht entfernt, wenn sie in der v2-API erstellt wurden.

## <a name="content-protection-concepts-tutorials-and-how-to-guides"></a>Konzepte, Tutorials und Schrittanleitungen zum Inhaltsschutz

### <a name="concepts"></a>Konzepte

- [Schützen Sie Ihren Inhalt mit der dynamischen Verschlüsselung von Media Services](drm-content-protection-concept.md)
- [Entwurf eines Multi-DRM-Inhaltsschutzsystems mit Zugriffssteuerung](architecture-design-multi-drm-system.md)
- [Media Services v3 mit PlayReady-Lizenzvorlage](drm-playready-license-template-concept.md)
- [Übersicht zu Media Services v3 mit Widevine-Lizenzvorlage](drm-widevine-license-template-concept.md)
- [Anforderungen und Konfiguration der Apple FairPlay-Lizenz](drm-fairplay-license-overview.md)
- [Streamingrichtlinien](stream-streaming-policy-concept.md)
- [Richtlinien für Inhaltsschlüssel](drm-content-key-policy-concept.md)

### <a name="tutorials"></a>Lernprogramme

[Schnellstart: Verschlüsseln von Inhalten mithilfe des Portals](drm-encrypt-content-how-to.md)

### <a name="how-to-guides"></a>Anleitungen

- [Abrufen eines Signaturschlüssels aus der vorhandenen Richtlinie](drm-get-content-key-policy-dotnet-how-to.md)
- [FairPlay-Offlinestreaming für iOS mit Media Services v3](drm-offline-fairplay-for-ios-concept.md)
- [Widevine-Offlinestreaming für Android mit Media Services v3](drm-offline-widevine-for-android.md)
- [Offlinestreaming mit PlayReady für Windows 10 mit Media Services v3](drm-offline-playready-streaming-for-windows-10.md)

## <a name="samples"></a>Beispiele

- [v2tov3MigrationSample](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/ContentProtection/v2tov3Migration)
- Sehen Sie sich auch den [Vergleich von V2- und V3-Code in den Codebeispielen](migrate-v-2-v-3-migration-samples.md) an.

## <a name="tools"></a>Tools

- [v3 Azure Media Services-Explorer-Tool](https://github.com/Azure/Azure-Media-Services-Explorer)
- [v2 Azure Media Services-Explorer-Tool](https://github.com/Azure/Azure-Media-Services-Explorer/releases/tag/v4.3.15.0)