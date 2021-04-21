---
ms.openlocfilehash: 9ddc240257bda29d5db5cae3eb6a830273d339cd
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107590095"
---
| Programmiersprache/Framework | Projekt auf<br/>GitHub                                                                                     | Paket                                                                               | Erste Schritte<br/>gestartet                        | Anmelden von Benutzern                                         | Zugriff auf Web-APIs                                                 | Allgemein verfügbar (Generally Available, GA) *oder*<br/>Öffentliche Vorschau<sup>1</sup> |
|----------------------|-----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|:------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Electron             | [MSAL Node.js](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node) | [msal-node](https://www.npmjs.com/package/@azure/msal-node)                    | —                                          | ![Bibliothek kann ID-Token für die Benutzeranmeldung anfordern][y] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Public Preview                                               |
| Java                 | [MSAL4J](https://github.com/AzureAD/microsoft-authentication-library-for-java)                            | [msal4j](https://mvnrepository.com/artifact/com.microsoft.azure/msal4j)               | —                                          | ![Bibliothek kann ID-Token für die Benutzeranmeldung anfordern][y] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar                                                           |
| macOS (Swift/Obj-C)  | [MSAL für iOS und macOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc)            | [MSAL](https://cocoapods.org/pods/MSAL)                                               | [Tutorial](../articles/active-directory/develop/tutorial-v2-ios.md)             | ![Bibliothek kann ID-Token für die Benutzeranmeldung anfordern][y] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar                                                           |
| UWP                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [Tutorial](../articles/active-directory/develop/tutorial-v2-windows-uwp.md)     | ![Bibliothek kann ID-Token für die Benutzeranmeldung anfordern][y] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar                                                           |
| WPF                  | [MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)                        | [Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client) | [Tutorial](../articles/active-directory/develop/tutorial-v2-windows-desktop.md) | ![Bibliothek kann ID-Token für die Benutzeranmeldung anfordern][y] | ![Bibliothek kann Zugriffstoken für geschützte Web-APIs anfordern][y] | Allgemein verfügbar                                                           |
<!--
| Java | Scribe | [Scribe Java](https://mvnrepository.com/artifact/org.scribe/scribe) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
| React Native | [React Native App Auth](https://github.com/FormidableLabs/react-native-app-auth/blob/main/docs/config-examples/azure-active-directory.md) | [react-native-app-auth](https://www.npmjs.com/package/react-native-app-auth) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> Für Bibliotheken in der *öffentlichen Vorschau* gelten [ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen][preview-tos].

<!--Image references-->

[y]: ../articles/active-directory/develop/media/common/yes.png
[n]: ../articles/active-directory/develop/media/common/no.png


<!--Reference-style links -->

[preview-tos]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/