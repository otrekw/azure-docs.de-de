---
title: 'Schnellstart: Teams-Interoperabilität unter Azure Communication Services'
titleSuffix: An Azure Communication Services quickstart
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie mit dem Azure Communication Calling SDK einer Teams-Besprechung beitreten.
author: chpalm
ms.author: chpalm
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-plat-web-ios-android
ms.openlocfilehash: 0e75d2b480a9cbfd2977d9d449c1ea12bdfe4920
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106095610"
---
# <a name="quickstart-join-your-calling-app-to-a-teams-meeting"></a>Schnellstart: Beitreten mit einer Telefonie-App zu einer Teams-Besprechung

[!INCLUDE [Public Preview](../../includes/public-preview-include-document.md)]

> [!IMPORTANT]
> Um die [Interoperabilität von Teams-Mandanten](../../concepts/teams-interop.md) zu aktivieren/deaktivieren, füllen Sie [dieses Formular](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR21ouQM6BHtHiripswZoZsdURDQ5SUNQTElKR0VZU0VUU1hMOTBBMVhESS4u) aus.

Steigen Sie in die Nutzung von Azure Communication Services ein, indem Sie Ihre Telefonielösung per JavaScript SDK mit Microsoft Teams verbinden.

::: zone pivot="platform-web"
[!INCLUDE [Calling with JavaScript](./includes/teams-interop-javascript.md)]
::: zone-end

::: zone pivot="platform-android"
[!INCLUDE [Calling with Android](./includes/teams-interop-android.md)]
::: zone-end

::: zone pivot="platform-ios"
[!INCLUDE [Calling with iOS](./includes/teams-interop-ios.md)]
::: zone-end

Die in diesem Dokument beschriebenen Funktionen verwenden die allgemein verfügbare Version der Communication Services SDKs. Für Teams-Interoperabilität ist die Betaversion der Communication Services SDKs erforderlich. Sie können sich die Beta-SDKs auf der [Seite mit den Versionshinweisen](https://github.com/Azure/Communication/tree/master/releasenotes) ansehen.

Wenn Sie den Schritt „Paket installieren“ mit den Beta-SDKs ausführen, ändern Sie die Version Ihres Pakets in das aktuelle Betarelease, indem Sie im Paketnamen `communication-calling` die Version `@1.0.0-beta.10` (Version zum Zeitpunkt der Artikelerstellung) angeben. Sie müssen den Paketbefehl `communication-common` nicht ändern. Beispiel:

```console
npm install @azure/communication-calling@1.0.0-beta.10 --save
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Communication Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind. Weitere Informationen zum Bereinigen von Ressourcen finden Sie [hier](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

- Sehen Sie sich unser [Hero-Beispiel für Anrufe](../../samples/calling-hero-sample.md) an.
- Informieren Sie sich über die [Funktionen des Calling SDK](./calling-client-samples.md).
- Informieren Sie sich über die [Funktionsweise von Anrufen](../../concepts/voice-video-calling/about-call-types.md).
