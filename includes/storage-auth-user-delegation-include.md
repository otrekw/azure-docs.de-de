---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/15/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0a4a10797109cb3363027e2445259d06d9aa071
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "76118113"
---
## <a name="about-the-user-delegation-sas"></a>Informationen zur SAS für die Benutzerdelegierung

Ein SAS-Token für den Zugriff auf einen Container oder ein Blob kann entweder mit Azure AD-Anmeldeinformationen oder einem Kontoschlüssel geschützt werden. Eine mit Azure AD-Anmeldeinformationen gesicherte SAS wird als SAS für die Benutzerdelegierung bezeichnet, da das OAuth 2.0-Token, das zum Signieren der SAS verwendet wird, im Namen des Benutzers angefordert wird.

Microsoft empfiehlt als bewährte Methode, nach Möglichkeit Azure AD-Anmeldeinformationen anstelle des Kontoschlüssels zu verwenden, der leichter kompromittiert werden kann. Wenn Ihr Anwendungsentwurf Shared Access Signatures erfordert, verwenden Sie Azure AD-Anmeldeinformationen, um eine SAS für die Benutzerdelegierung zu erstellen und damit die Sicherheit zu erhöhen. Weitere Informationen zur SAS für die Benutzerdelegierung finden Sie unter [Erstellen einer SAS für die Benutzerdelegierung](/rest/api/storageservices/create-user-delegation-sas).

> [!CAUTION]
> Jeder Client, der über eine gültige SAS verfügt, kann auf Daten in Ihrem Speicherkonto zugreifen, sofern von dieser SAS zugelassen. Es ist wichtig, eine SAS vor böswilliger oder unbeabsichtigter Verwendung zu schützen. Verteilen Sie eine SAS mit Diskretion, und halten Sie einen Plan für den Widerruf einer kompromittierten SAS bereit.

Weitere Informationen zu SAS (Shared Access Signatures) finden Sie unter [Gewähren von eingeschränktem Zugriff auf Azure Storage-Ressourcen mithilfe von SAS (Shared Access Signature)](../articles/storage/common/storage-sas-overview.md).
