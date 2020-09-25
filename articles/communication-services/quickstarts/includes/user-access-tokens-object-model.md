---
title: include file
description: include file
services: azure-communication-services
author: matthewrobertson
manager: nimag
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 08/20/2020
ms.topic: include
ms.custom: include file
ms.author: marobert
ms.openlocfilehash: c0ba8e76e069bf9dc1c96aecdc670699c32b3c96
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90944588"
---
## <a name="object-model"></a>Objektmodell

Mit Benutzerzugriffstoken können Sie die Benutzer Ihrer Anwendung autorisieren, sodass sie eine direkte Verbindung mit Ihrer Azure Communication Services-Ressource herstellen können. Diese Token werden auf Ihrem Server generiert, an die Clientanwendung zurückgegeben und dann zum Initialisieren der Clientbibliotheken von Communication Services verwendet. Hierzu sollte ein vertrauenswürdiger Dienstendpunkt erstellt werden, der Ihre Benutzer authentifizieren und Benutzerzugriffstoken ausstellen kann. Von diesem Dienst sollte eine persistente Zuordnung zwischen den Benutzeridentitäten Ihrer Anwendung und den Azure Communication Services-Benutzern gepflegt werden.

Sämtliche Funktionen im Zusammenhang mit Benutzern und Zugriffstoken werden von der Klasse „CommunicationIdentityClient“ bereitgestellt. Sie wird mit einer Verbindungszeichenfolge instanziiert und anschließend verwendet, um Benutzer zu verwalten und Zugriffstoken auszustellen.

> [!WARNING]
> Token sind vertrauliche Daten, da sie den Zugriff auf Ressourcen eines Benutzers gewähren. Deshalb müssen Token unbedingt vor Gefährdung geschützt werden. Es empfiehlt sich, einen vertrauenswürdigen Dienstendpunkt zu erstellen, von dem nur Zugriffstoken für autorisierte Benutzer Ihrer Anwendung ausgestellt werden. Wenn Sie Benutzerzugriffstoken in einen Sicherungsspeicher zwischenspeichern, sollte unbedingt eine Verschlüsselung verwendet werden.

### <a name="access-token-scopes"></a>Bereiche für Zugriffstoken

Mithilfe von Bereichen können Sie die exakte Azure Communications Services-Funktion angeben, die durch ein Benutzerzugriffstoken autorisiert werden kann. Bereiche werden auf individuelle Benutzerzugriffstoken angewendet. Von Azure Communication Services werden die folgenden Bereiche für Benutzerzugriffstoken unterstützt:

| Name   | Beschreibung                                                                         |
| ------ | ----------------------------------------------------------------------------------- |
| `Chat` | Ermöglicht die Teilnahme an einem Chat                                         |
| `VoIP` | Ermöglicht aus- und eingehende VoIP-Anrufe unter Verwendung der Clientbibliothek für Telefonie* |
| `Pstn` | Ermöglicht ausgehende PSTN-Anrufe unter Verwendung der Clientbibliothek für Telefonie*           |

\* Dieses Feature wird noch nicht unterstützt, aber demnächst verfügbar gemacht.

Wenn Sie einem Benutzer den Zugriff auf bestimmte Funktionen entziehen möchten, müssen Sie zunächst [alle vorhandenen Zugriffstoken](#revoke-user-access-tokens) widerrufen, die ggf. nicht gewünschte Bereiche enthalten, und anschließend ein neues Token mit stärker eingeschränkten Bereichen ausstellen.
