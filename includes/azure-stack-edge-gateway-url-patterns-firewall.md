---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/19/2021
ms.author: alkohli
ms.openlocfilehash: aef81c1ad80d29df05ab8e8c5f27b8827d7993f7
ms.sourcegitcommit: eda26a142f1d3b5a9253176e16b5cbaefe3e31b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109805491"
---
|    URL-Muster |    Komponente oder Funktion  |
|-----------------------------------------------------|-----------------------------------------|
|    https://\*.databoxedge.azure.com/\*<br>https://\*.servicebus.windows.net/\*<br>https://login.microsoftonline.com |    Azure Stack Edge-Dienst<br>Azure-Servicebus<br>Authentifizierungsdienst: Azure Active Directory                           |
|    http:\//crl.microsoft.com/pki/\*<br>http:\//www.microsoft.com/pki/\*                                                                                                                                                                                                                                                                                                                                                                                                  |    Zertifikatswiderruf                                                                               |
|    https://\*.core.windows.net/\*<br>https://\*.data.microsoft.com<br>http://\*.msftncsi.com                                                                                                                                                                                                                                                                                                                                                                            |    Azure-Speicherkonten und Überwachung                                                                |
|    http:\//windowsupdate.microsoft.com<br>http://\*.windowsupdate.microsoft.com<br>https://\*.windowsupdate.microsoft.com<br>http://\*.update.microsoft.com<br>https://\*.update.microsoft.com<br>http://\*.windowsupdate.com<br>http://download.microsoft.com<br>http://\*.download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://\*.ws.microsoft.com<br>https://\*.ws.microsoft.com<br>http://\*.mp.microsoft.com |    Microsoft Update-Server                                                                             |
|    http://\*.deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                                          |    Akamai CDN                                                                                           |
|    http://\*.data.microsoft.com     |    Telemetriedienst in Windows. Informationen finden Sie im Update für Kundenzufriedenheit und Diagnosetelemetrie. |
<!--|    http://www.msftconnecttest.com/connecttest.txt  |    Für die Diagnose     ||  |-->   

