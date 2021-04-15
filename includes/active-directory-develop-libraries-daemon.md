---
ms.openlocfilehash: 0837c0a23c837065dc2214f947912ee25e4f1d2d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103007996"
---
| Programmiersprache/Framework | Projekt auf<br/>GitHub                                                                 | Paket                                                                                | Erste Schritte<br/>gestartet                           | Anmelden von Benutzern                                            | Zugriff auf Web-APIs                                                 | Allgemein verfügbar (Generally Available, GA) *oder*<br/>Öffentliche Vorschau<sup>1</sup> |
|----------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|:---------------------------------------------:|:--------------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| .NET                 | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)    | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client/) | [Schnellstart](../articles/active-directory/develop/quickstart-v2-netcore-daemon.md) | ![Bibliothek kann keine ID-Token für die Benutzeranmeldung anfordern][n] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar                                                           |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)        | [msal4j](https://javadoc.io/doc/com.microsoft.azure/msal4j/latest/index.html)          | —                                             | ![Bibliothek kann keine ID-Token für die Benutzeranmeldung anfordern][n] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar                                                           |
| Node               | [MSAL Node](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-node](https://www.npmjs.com/package/@azure/msal-node)  | [Schnellstart](../articles/active-directory/develop/quickstart-v2-nodejs-console.md)  | ![Bibliothek kann keine ID-Token für die Benutzeranmeldung anfordern][n] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar  |
| Python               | [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python) | [msal-python](https://github.com/AzureAD/microsoft-authentication-library-for-python)  | [Schnellstart](../articles/active-directory/develop/quickstart-v2-python-daemon.md)  | ![Bibliothek kann keine ID-Token für die Benutzeranmeldung anfordern][n] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar                                                           |
<!--
|PHP| [The PHP League oauth2-client](https://oauth2-client.thephpleague.com/usage/) | [League\OAuth2](https://oauth2-client.thephpleague.com/) | ![Green check mark.][n] | ![X indicating no.][n] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> Für Bibliotheken in der *öffentlichen Vorschau* gelten [ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen][preview-tos].

<!--Image references-->

[y]: ../articles/active-directory/develop/media/common/yes.png
[n]: ../articles/active-directory/develop/media/common/no.png

<!--Reference-style links -->

[preview-tos]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/