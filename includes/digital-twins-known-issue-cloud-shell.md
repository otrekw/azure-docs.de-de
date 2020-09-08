---
author: baanders
description: 'Includedatei für Azure Digital Twins: bekanntes Problem mit der Cloud Shell-Authentifizierung'
ms.service: digital-twins
ms.topic: include
ms.date: 9/1/2020
ms.author: baanders
ms.openlocfilehash: bd3a192ed493a886da427cb6d89239a4eb8e4d17
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89290095"
---
>[!NOTE]
>Zurzeit besteht ein **bekanntes Problem** in Cloud Shell, das sich auf diese Befehlsgruppen auswirkt: `az dt route`, `az dt model`, `az dt twin`.
>
>Problemlösung: Führen Sie vor Ausführung des Befehls das Cmdlet `az login` in Cloud Shell aus, oder verwenden Sie die [lokale Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) anstelle von Cloud Shell. Weitere Informationen hierzu finden Sie unter [*Problembehandlung: Bekannte Probleme in Azure Digital Twins*](../articles/digital-twins/troubleshoot-known-issues.md#400-client-error-bad-request-in-cloud-shell).