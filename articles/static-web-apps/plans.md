---
title: Hostingpläne für Azure Static Web Apps
description: Vergleichen Sie die verschiedenen Hostingpläne für Azure Static Web Apps, und stellen Sie sie einander gegenüber.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/14/2021
ms.author: cshoe
ms.openlocfilehash: 2fcb9805b752273cb5320114342108ed1f844a5b
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110104112"
---
# <a name="azure-static-web-apps-hosting-plans"></a>Hostingpläne für Azure Static Web Apps

Azure Static Web Apps wird mit zwei Plänen angeboten: Free und Standard. Informationen zu den [Kosten des Standard-Plans finden Sie auf der Preisseite](https://azure.microsoft.com/pricing/details/app-service/static/).

## <a name="features"></a>Features

| Funktion | Free-Plan <br> (Für private Projekte) | Standard-Plan <br> (Für Produktions-Apps) |
| --- | --- | --- |
| Webhosting | ✔ | ✔ |
| GitHub-Integration | ✔ | ✔ |
| Azure DevOps-Integration | ✔ | ✔ |
| Global verteilter statischer Inhalt | ✔ | ✔ |
| Kostenlose, automatische Verlängerung von SSL-Zertifikaten | ✔ | ✔ |
| Stagingumgebungen | 3 pro App | 10 pro App |
| Max. App-Größe | 250 MB pro App | 500 MB pro App |
| Benutzerdefinierte Domänen | 2 pro App | 5 pro App |
| APIs über Azure Functions | Verwaltet | Verwaltet oder<br>[Eigene Funktions-App verwenden](functions-bring-your-own.md) |
| Integration des Authentifizierungsanbieters | [Vorkonfiguriert](authentication-authorization.md)<br>(Vom Dienst definiert) | [Benutzerdefinierte Registrierungen](authentication-custom.md) |
| [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/app-service-static/v1_0/) | Ohne  | ✔ |

## <a name="selecting-a-plan"></a>Auswählen eines Plans

Die folgenden Szenarien können Ihnen bei der Entscheidung helfen, ob der Standard-Plan am besten zu Ihren Bedürfnissen passt.

- Das erwartete Datenverkehrsvolumen überschreitet Bandbreitenobergrenzen.
- Die vorhandene Azure Functions-App, die Sie verwenden möchten, verfügt entweder über Trigger und Bindungen, die über HTTP-Endpunkte hinausgehen, oder kann nicht in eine verwaltete Funktions-App konvertiert werden.
- Sicherheitsanforderungen, die eine [benutzerdefinierte Anbieterregistrierung](authentication-custom.md) erfordern.
- Die Gesamtgröße der Webressourcen der Website überschreitet die Speicherhöchstwerte.
- Sie benötigen formalen Kundensupport.
- Sie benötigen mehr als drei [Stagingumgebungen](review-publish-pull-requests.md).

Informationen zu Einschränkungen finden Sie im Leitfaden zu [Kontingenten](quotas.md).

## <a name="changing-plans"></a>Ändern von Plänen

Sie können im Azure-Portal zwischen Free- und Standard-Plan wechseln.

1. Navigieren Sie im Azure-Portal zu einer Static Web Apps-Ressource.

1. Wählen Sie im Menü _Einstellungen_ die Option **Hostingplan** aus.

1. Wählen Sie für Ihre statische Web-App den gewünschten Hostingplan aus.

1. Wählen Sie **Speichern** aus.
