---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/12/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 95639caddfa627290f4e7153d50f9f7799a1f320
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75460550"
---
## <a name="protect-your-access-keys"></a>Schützen Ihrer Zugriffsschlüssel

Ihre Speicherkonto-Zugriffsschlüssel ähneln einem Stammkennwort für das Speicherkonto. Achten Sie darauf, die Zugriffsschlüssel immer gut zu schützen. Verwenden Sie Azure Key Vault zum sicheren Verwalten und Rotieren Ihrer Schlüssel. Geben Sie Zugriffsschlüssel nicht an andere Benutzer weiter, vermeiden Sie das Hartcodieren, und speichern Sie die Schlüssel nicht als Klartext, auf den andere Benutzer Zugriff haben. Rotieren Sie die Schlüssel, wenn Sie glauben, dass sie möglicherweise gefährdet sind.

Verwenden Sie nach Möglichkeit Azure Active Directory (Azure AD) zum Autorisieren von Anforderungen an Blob Storage und Queue Storage anstelle eines gemeinsam verwendeten Schlüssels. Azure AD bietet überlegene Sicherheit und Benutzerfreundlichkeit gegenüber dem gemeinsam verwendeten Schüssel. Weitere Informationen zur Autorisierung des Datenzugriffs mit Azure AD finden Sie unter [Autorisieren des Zugriffs auf Azure-Blobs und -Warteschlangen mit Azure Active Directory](../articles/storage/common/storage-auth-aad.md).
