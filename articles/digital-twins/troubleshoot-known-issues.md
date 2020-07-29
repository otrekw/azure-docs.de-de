---
title: 'Bekannte Probleme: Azure Digital Twins'
description: Hilfreiche Informationen zur Erkennung und Behebung von bekannten Problemen mit Azure Digital Twins.
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.service: digital-twins
ms.date: 07/14/2020
ms.openlocfilehash: 8fce451d9b806d2fa9a4f3d9e1c117de0aaa9fc0
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86528145"
---
# <a name="known-issues-in-azure-digital-twins"></a>Bekannte Probleme in Azure Digital Twins

Dieser Artikel enthält Informationen zu bekannten Problemen mit Azure Digital Twins.

## <a name="managing-event-routes-in-the-azure-portal"></a>Verwalten von Ereignisrouten im Azure-Portal

Wenn Sie sich beim Portal mit einem persönlichen [**Microsoft-Konto (MSA)** ](https://account.microsoft.com/account/Account) angemeldet haben, z. B. einem *@outlook.com* -Konto, wird ein Fenster mit der Meldung *You need permission to view event routes* (Sie benötigen die Berechtigung zum Anzeigen von Ereignisrouten) angezeigt, wenn Sie Ereignisrouten im Portal verwalten möchten. Dies gilt unabhängig von Ihrer Berechtigungsstufe.

:::image type="content" source="media/troubleshoot-known-issues/event-route-need-permission.png" alt-text="Screenshot: Berechtigungsfehler im Azure-Portal beim Versuch, auf einer Azure Digital Twins-Instanz Ereignisrouten zu erstellen":::

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Benutzer, die derzeit im Portal keine Ereignisrouten verwalten können, können die Azure Digital Twins-APIs oder die CLI nutzen, um die Verwaltung für Ereignisrouten durchzuführen. Der Wechsel zu einem dieser Tools für die Verwaltung von Ereignisrouten ist die empfohlene Strategie zur Behebung dieses Problems.

Eine Anleitung hierzu finden Sie unter [*Gewusst wie: Verwalten von Endpunkten und Routen*](how-to-manage-routes.md).

### <a name="possible-causes"></a>Mögliche Ursachen

Sie haben sich beim Portal mit einem persönlichen [Microsoft-Konto (MSA)](https://account.microsoft.com/account/Account) angemeldet, z. B. einem *@outlook.com* -Konto. Das Verwalten von Ereignisrouten im Azure-Portal ist derzeit nur für Azure-Benutzer mit Unternehmensdomänenkonten verfügbar.

## <a name="400-client-error-bad-request-in-cloud-shell"></a>„400 Clientfehler: Ungültige Anforderung“ in Cloud Shell

Für Befehle in Cloud Shell kann zeitweilig der folgende Fehler auftreten: „400 Clientfehler: Ungültige Anforderung für URL: http://localhost:50342/oauth2/token„ (gefolgt von der vollständigen Stapelüberwachung).

### <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Sie können dieses Problem beheben, indem Sie den Befehl `az login` erneut ausführen und die nachfolgenden Anmeldeschritte ausführen.

Anschließend sollten Sie wieder in der Lage sein, den Befehl auszuführen.

### <a name="possible-causes"></a>Mögliche Ursachen

Dies ist das Ergebnis eines bekannten Problems in Cloud Shell: [*Beim Abrufen des Tokens aus Cloud Shell tritt zeitweilig „400 Clientfehler: Ungültige Anforderung“ auf*](https://github.com/Azure/azure-cli/issues/11749).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über die Sicherheit und Berechtigungen in Azure Digital Twins:
* [*Konzepte: Sicherheit für Azure Digital Twins-Lösungen*](concepts-security.md)