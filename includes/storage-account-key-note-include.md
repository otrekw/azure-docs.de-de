---
title: Datei einfügen
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ef0ad097f0dff99de71096cb24eba511a21bf06a
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2021
ms.locfileid: "122261178"
---
## <a name="protect-your-access-keys"></a>Schützen Ihrer Zugriffsschlüssel

Ihre Speicherkonto-Zugriffsschlüssel ähneln einem Stammkennwort für das Speicherkonto. Achten Sie darauf, die Zugriffsschlüssel immer gut zu schützen. Verwenden Sie Azure Key Vault zum sicheren Verwalten und Rotieren Ihrer Schlüssel. Geben Sie Zugriffsschlüssel nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie die Schlüssel nicht als Klartext, auf den andere Benutzer Zugriff haben. Rotieren Sie die Schlüssel, wenn Sie glauben, dass sie möglicherweise gefährdet sind.

> [!NOTE]
> Microsoft empfiehlt nach Möglichkeit die Verwendung von Azure Active Directory (Azure AD) zum Autorisieren von Anforderungen an Blob- und Warteschlangendaten anstelle eines gemeinsam verwendeten Schlüssels. Azure AD bietet überlegene Sicherheit und Benutzerfreundlichkeit gegenüber dem gemeinsam verwendeten Schüssel. Weitere Informationen zur Autorisierung des Datenzugriffs mit Azure AD finden Sie unter [Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](../articles/storage/blobs/authorize-access-azure-active-directory.md).