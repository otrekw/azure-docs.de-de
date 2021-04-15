---
title: include file
description: include file
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/24/2020
ms.openlocfilehash: aa8aea66c5481454f0d7d4d118934f5fbf34a911
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89147254"
---
Bei der **standardmäßigen verwalteten Identität** handelt es sich um die systemseitig zugewiesene verwaltete Identität oder die erste benutzerseitig zugewiesene verwaltete Identität.

Während einer Ausführung gibt es zwei Anwendungen einer Identität:

1. Das System verwendet eine Identität zum Einrichten der Speichereinbindungen, der Containerregistrierung und der Datenspeicher des Benutzers.

    * In diesem Fall verwendet das System die standardmäßig verwaltete Identität.

1. Der Benutzer wendet eine Identität für den Zugriff auf Ressourcen aus dem Code für eine übermittelte Ausführung an.

    * Geben Sie in diesem Fall den Wert für *client_id* entsprechend der verwalteten Identität an, die Sie zum Abrufen von Anmeldeinformationen verwenden möchten.
    * Alternativ können Sie die Client-ID der benutzerseitig zugewiesenen Identität über die Umgebungsvariable *DEFAULT_IDENTITY_CLIENT_ID* abrufen.

    So rufen Sie beispielsweise ein Token für einen Datenspeicher mit der standardmäßig verwalteten Identität ab:

    ```python
    client_id = os.environ.get('DEFAULT_IDENTITY_CLIENT_ID')
    credential = ManagedIdentityCredential(client_id=client_id)
    token = credential.get_token('https://storage.azure.com/')
    ```