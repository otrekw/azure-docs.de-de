---
title: Unformatierte Azure CDN-HTTP-Protokolle
description: In diesem Artikel werden die unformatierten Azure CDN-HTTP-Protokolle beschrieben.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2020
ms.author: allensu
ms.openlocfilehash: 79ced4df8df12bf2ef1fbe0075b53d02fafb2aff
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79129838"
---
# <a name="azure-cdn-http-raw-logs"></a>Unformatierte Azure CDN-HTTP-Protokolle
Unformatierte Protokolle bieten umfassende Informationen zu Vorgängen und Fehlern, die für die Überwachung und Problembehandlung wichtig sind. Unformatierte Protokolle unterscheiden sich von Aktivitätsprotokollen. Aktivitätsprotokolle geben Einblicke in die Vorgänge, die für Azure-Ressourcen ausgeführt wurden. Unformatierte Protokolle stellen einen Datensatz für Vorgänge der Ressourcen bereit.

> [!IMPORTANT]
> Das Feature für unformatierte HTTP-Protokolle ist für Azure CDN von Microsoft verfügbar.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="configuration"></a>Konfiguration

So konfigurieren Sie unformatierte Protokolle für Azure CDN über Microsoft-Profile 

1. Wählen Sie im Menü des Azure-Portals **Alle Ressourcen** >>  **\<Ihr-CDN-Profil>** aus.

2. Wählen von unter **Überwachung** die Option **Diagnoseeinstellungen** aus.

3. Wählen Sie **+Diagnoseeinstellung hinzufügen**  aus.

    ![Diagnoseeinstellung von CDN](./media/cdn-raw-logs/raw-logs-01.png)

    > [!IMPORTANT]
    > Unformatierte Protokolle sind nur auf Profilebene verfügbar, während aggregierte HTTP-Statuscodeprotokolle auf Endpunktebene verfügbar sind.

4. Geben Sie unter **Diagnoseeinstellungen** einen Namen für die Diagnoseeinstellung unter **Name der Diagnoseeinstellungen** ein.

5. Wählen Sie das **Protokoll** aus, und legen Sie die Aufbewahrungsdauer in Tagen fest.

6. Wählen Sie die **Zieldetails** aus. Folgende Optionen für das Ziel sind verfügbar:
    * **An Log Analytics senden**
        * Wählen Sie das **Abonnement** und den **Log Analytics-Arbeitsbereich** aus.
    * **In einem Speicherkonto archivieren**
        * Wählen Sie das **Abonnement** und das **Speicherkonto** aus.
    * **An einen Event Hub streamen**
        * Wählen Sie das **Abonnement**, den **Event Hub-Namespace**, den **Event Hub-Namen** (optional) und den **Event Hub-Richtliniennamen** aus.

    ![Diagnoseeinstellung von CDN](./media/cdn-raw-logs/raw-logs-02.png)

7. Wählen Sie **Speichern** aus.

## <a name="raw-logs-properties"></a>Eigenschaften von unformatierten Protokollen

Azure CDN vom Microsoft-Dienst stellt zurzeit unformatierte Protokolle bereit (stündlich in Batches zusammengefasst). Unformatierte Protokolle enthalten einzelne API-Anforderungen, wobei jeder Eintrag folgendem Schema entspricht: 

| Eigenschaft              | BESCHREIBUNG                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrackingReference     | Die eindeutige Verweiszeichenfolge zur Identifizierung einer Anforderung, die von der Front Door-Instanz verarbeitet wird. Diese wird auch als X-Azure-Ref-Header an den Client gesendet. Sie ist für die Suche nach Informationen in den Zugriffsprotokollen für eine bestimmte Anforderung erforderlich. |
| HttpMethod            | Von der Anforderung verwendete HTTP-Methode                                                                                                                                                                     |
| HttpVersion           | Typ der Anforderung oder Verbindung.                                                                                                                                                                   |
| RequestUri            | URI der empfangenen Anforderung                                                                                                                                                                         |
| RequestBytes          | Die Größe der HTTP-Anforderungsnachricht in Byte, einschließlich Anforderungsheader und -text.                                                                                                   |
| ResponseBytes         | Die vom Back-End-Server als Antwort gesendeten Bytes.                                                                                                                                                    |
| UserAgent             | Der vom Client verwendete Browsertyp                                                                                                                                                               |
| ClientIp              | Die IP-Adresse des Clients, der die Anforderung gestellt hat.                                                                                                                                                  |
| TimeTaken             | Die Dauer der Aktion in Millisekunden.                                                                                                                                            |
| SecurityProtocol      | Die TLS-/SSL-Protokollversion, die von der Anforderung verwendet wird, oder NULL, wenn keine Verschlüsselung verwendet wird.                                                                                                                           |
| Endpunkt              | Der CDN-Endpunkthost wurde unter dem übergeordneten CDN-Profil konfiguriert.                                                                                                                                   |
| Back-End-Hostname     | Der Name des Back-End-Hosts oder Ursprungs, an den Anforderungen gesendet werden.                                                                                                                                |
| An Origin Shield gesendet | Wenn der Wert TRUE ist, wurde die Anforderung vom Origin Shield-Cache anstelle des Edge-POP beantwortet. Origin Shield ist ein übergeordneter Cache zum Verbessern der Cachetrefferquote.                                       |
| HttpStatusCode        | Der HTTP-Statuscode, der vom Proxy zurückgegeben wurde.                                                                                                                                                        |
| HttpStatusDetails     | Resultierender Status in der Anforderung. Informationen zur Bedeutung dieses Zeichenfolgenwerts finden Sie in der Verweistabelle zum Status.                                                                                              |
| POP                   | Der Edge-POP, der auf die Benutzeranforderung geantwortet hat. Die POP-Abkürzungen sind Flughafencodes der jeweiligen Regionen.                                                                                   |
| Cachestatus          | Gibt an, ob das Objekt aus dem Cache zurückgegeben wurde oder vom Ursprung stammt.                                                                                                             |

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie unformatierte HTTP-Protokolle für den Microsoft CDN-Dienst aktiviert.

Weitere Informationen zu Azure CDN und den anderen in diesem Artikel erwähnten Azure-Diensten finden Sie unter den folgenden Quellen:

* [Analysieren](cdn-log-analysis.md) Sie Azure CDN-Verwendungsmuster.

* Weitere Informationen zu Azure Monitor finden Sie [hier](https://docs.microsoft.com/azure/azure-monitor/overview).

* Konfigurieren Sie [Log Analytics in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).