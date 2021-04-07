---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ba715d510dc296ffa8f9c0ee58841f284416a118
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86027329"
---
## <a name="protect-your-access-keys"></a>Schützen Ihrer Zugriffsschlüssel

Ihre Speicherkonto-Zugriffsschlüssel ähneln einem Stammkennwort für das Speicherkonto. Achten Sie darauf, die Zugriffsschlüssel immer gut zu schützen. Verwenden Sie Azure Key Vault zum sicheren Verwalten und Rotieren Ihrer Schlüssel. Geben Sie Zugriffsschlüssel nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie die Schlüssel nicht als Klartext, auf den andere Benutzer Zugriff haben. Rotieren Sie die Schlüssel, wenn Sie glauben, dass sie möglicherweise gefährdet sind.

> [!NOTE]
> Microsoft empfiehlt nach Möglichkeit die Verwendung von Azure Active Directory (Azure AD) zum Autorisieren von Anforderungen an Blob- und Warteschlangendaten anstelle eines gemeinsam verwendeten Schlüssels. Azure AD bietet überlegene Sicherheit und Benutzerfreundlichkeit gegenüber dem gemeinsam verwendeten Schüssel. Weitere Informationen zur Autorisierung des Datenzugriffs mit Azure AD finden Sie unter [Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](../articles/storage/common/storage-auth-aad.md).
