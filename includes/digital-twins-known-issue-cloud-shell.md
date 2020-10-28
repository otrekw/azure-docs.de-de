---
author: baanders
description: 'Includedatei für Azure Digital Twins: bekanntes Problem mit der Cloud Shell-Authentifizierung'
ms.service: digital-twins
ms.topic: include
ms.date: 9/1/2020
ms.author: baanders
ms.openlocfilehash: 574ca9d42565e897e49d6800e61bb2c33717891b
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92321497"
---
>[!NOTE]
>Derzeit besteht ein **bekanntes Problem** in Cloud Shell, das sich auf die folgenden Befehlsgruppen auswirkt, wenn die Ausführung über *https://shell.azure.com* erfolgt: `az dt route`, `az dt model`, `az dt twin`.
>
>Verwenden Sie für die Behebung eine der folgenden Vorgehensweisen:
> * Führen Sie `az login` in Cloud Shell aus, bevor Sie den Befehl ausführen.
> * Öffnen Sie den Cloud Shell-Bereich im Azure-Portal, und setzen Sie Ihre Cloud Shell-Arbeitsschritte darin fort.
>  :::image type="content" source="../articles/digital-twins/media/includes/portal-cloud-shell.png" alt-text="Ansicht des Azure-Portals mit hervorgehobenem Symbol „Cloud Shell“ und Anzeige von Cloud Shell im unteren Bereich des Portalfensters":::
> * Verwenden Sie die [lokale CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) anstelle von Cloud Shell.
>
>Weitere Informationen zu diesem Problem finden Sie unter [*Problembehandlung: Bekannte Probleme in Azure Digital Twins*](../articles/digital-twins/troubleshoot-known-issues.md#400-client-error-bad-request-in-cloud-shell).