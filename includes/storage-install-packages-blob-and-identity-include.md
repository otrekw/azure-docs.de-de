---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/26/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: de79ea50d12ab322d1e28d0ad650df30ecc0c222
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "74806580"
---
## <a name="install-client-library-packages"></a>Installieren von Clientbibliothekspaketen

> [!NOTE]
> Die hier genannten Beispiele verwenden die Azure Storage-Clientbibliothek unter Version 12. Version 12 der Clientbibliothek ist Teil des Azure SDK. Weiter Informationen über das Azure SDK finden Sie im Azure SDK-Repository unter [GitHub](https://github.com/Azure/azure-sdk).

Führen Sie zum Installieren des Blob Speicher-Pakets den folgenden Befehl über die NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Storage.Blobs
```

In den hier genannten Beispielen wird auch die neueste Version der [Azure Identity-Clientbibliothek für .NET](https://www.nuget.org/packages/Azure.Identity/) zur Authentifizierung mit Azure AD-Anmeldeinformationen verwendet. Führen Sie zum Installieren des Pakets den folgenden Befehl über die NuGet-Paket-Manager-Konsole aus:

```powershell
Install-Package Azure.Identity
```
