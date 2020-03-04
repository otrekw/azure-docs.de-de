---
title: Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel
description: Verwenden Sie die Fusion-Technologie in Azure Sentinel, um die „Alarmmüdigkeit“ zu reduzieren und verwertbare Incidents zu erstellen, die auf der erweiterten Erkennung von mehrstufigen Angriffen basieren.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 87ca322cbdfdd8a53a3ecefcb120a961ea1bb936
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587922"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel


> [!IMPORTANT]
> Einige Fusion-Features in Azure Sentinel befinden sich zurzeit in der Public Preview.
> Diese Features werden ohne Vereinbarung zum Servicelevel bereitgestellt und nicht für Produktionsworkloads empfohlen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Durch die Verwendung der auf maschinellem Lernen basierenden Fusion-Technologie kann Azure Sentinel mehrstufige Angriffe automatisch erkennen. Hierzu werden anomale Verhaltensweisen und verdächtige Aktivitäten kombiniert, die an verschiedenen Stellen der Kill Chain beobachtet werden. Azure Sentinel generiert dann Incidents, die auf andere Weise nur sehr schwer abgefangen werden können. Diese Incidents umfassen mindestens zwei Warnungen oder Aktivitäten. Standardmäßig weisen diese Incidents ein geringes Volumen, eine hohe Qualität und einen hohen Schweregrad auf.

Diese Erkennung ist für Ihre Umgebung angepasst und bewirkt nicht nur eine Reduzierung der False Positive-Rate, sondern kann auch Angriffe mit eingeschränkten oder fehlenden Informationen erkennen.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Konfiguration für die erweiterte Erkennung von mehrstufigen Angriffen

Diese Erkennung ist in Azure Sentinel standardmäßig aktiviert. Befolgen Sie diese Anleitung, um den Status zu überprüfen oder zu deaktivieren, weil Sie beispielsweise eine andere Lösung nutzen, um Incidents basierend auf mehreren Warnungen zu erstellen:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.

2. Navigieren Sie zu **Azure Sentinel** > **Konfiguration** > **Analyse**.

3. Wählen Sie in der Spalte **NAME** die Option **Active rules** (Aktive Regeln) und dann **Advanced Multistage Attack Detection** (Erweiterte Erkennung von mehrstufigen Angriffen) aus. Überprüfen Sie in der Spalte **STATUS**, ob diese Erkennung aktiviert oder deaktiviert ist.

4. Wählen Sie zum Ändern des Status diesen Eintrag und auf dem Blatt **Advanced Multistage Attack Detection** (Erweiterte Erkennung von mehrstufigen Angriffen) die Option **Bearbeiten** aus.

5. Auf dem Blatt **Assistent zum Erstellen von Regeln** ist die Statusänderung automatisch für Sie ausgewählt. Wählen Sie **Weiter: Überprüfen** und dann **Speichern** aus. 

Regelvorlagen können für die erweiterte Erkennung von mehrstufigen Angriffen nicht genutzt werden.

> [!NOTE]
> Azure Sentinel verwendet zurzeit 30 Tage an Verlaufsdaten, um die Systeme für das maschinelle Lernen zu trainieren. Diese Daten werden immer mit den Schlüsseln von Microsoft verschlüsselt, wenn sie die Pipeline für maschinelles Lernen durchlaufen. Die Trainingsdaten werden jedoch nicht mit [vom Kunden verwalteten Schlüsseln (Customer Managed Keys, CMK)](customer-managed-keys.md) verschlüsselt, wenn Sie CMK in Ihrem Azure Sentinel-Arbeitsbereich aktiviert haben. Um Fusion zu deaktivieren, navigieren Sie zu **Azure Sentinel** \> **Konfiguration** \> **Analyse \> Aktive Regeln \> Erweiterte mehrstufige Angriffserkennung**, und wählen Sie in der Spalte **Status** die Option **Deaktivieren** aus.

## <a name="fusion-using-palo-alto-networks-and-microsoft-defender-atp"></a>Fusion mithilfe von Palo Alto Networks und Microsoft Defender ATP

In diesen Szenarien werden zwei der grundlegenden Protokolle kombiniert, die von Sicherheitsanalysten verwendet werden: Firewallprotokolle von Palo Alto Networks und Endpunkt-Erkennungsprotokolle von Microsoft Defender ATP. In allen unten aufgeführten Szenarien wird eine verdächtige Aktivität am Endpunkt erkannt, die eine externe IP-Adresse betrifft, und darauf folgt anomaler Datenverkehr von der externen IP-Adresse zurück zur Firewall. In Palo Alto-Protokollen sucht Azure Sentinel hauptsächlich nach [Bedrohungen](https://docs.paloaltonetworks.com/pan-os/8-1/pan-os-admin/monitoring/view-and-manage-logs/log-types-and-severity-levels/threat-logs), und der Datenverkehr wird als verdächtig eingestuft, wenn Bedrohungen durchgelassen werden (verdächtige Daten, Dateien, Überflutungen, Pakete, Scans, Spyware, URLs, Viren, Sicherheitsrisiken, Wildfireviren, Wildfires).

### <a name="network-request-to-tor-anonymization-service-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Netzwerkanforderung an den TOR-Anonymisierungsdienst gefolgt von anomalem, durch die Palo Alto Networks-Firewall gekennzeichnetem Datenverkehr

In diesem Szenario erkennt Azure Sentinel zuerst eine Warnung, dass Microsoft Defender Advanced Threat Protection eine Netzwerkanforderung an einen TOR-Anonymisierungsdienst erkannt hat, der zu anomalen Aktivitäten führte. Dies wurde unter dem Konto {account name} mit der SID-ID {sid} um {time} initiiert. Die ausgehende IP-Adresse der Verbindung lautete {IndividualIp}.
Anschließend wurden ungewöhnliche Aktivitäten von der Palo Alto Networks-Firewall um {TimeGenerated} erkannt. Dies deutet darauf hin, dass schädlicher Datenverkehr in Ihr Netzwerk eingedrungen ist. Die Ziel-IP-Adresse für den Netzwerkdatenverkehr lautet {DestinationIP}.

Dieses Szenario befindet sich zurzeit in der Public Preview.


### <a name="powershell-made-a-suspicious-network-connection-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>PowerShell hat eine verdächtige Netzwerkverbindung hergestellt, auf die anomaler, durch die Palo Alto Networks-Firewall gekennzeichneter Datenverkehr folgte.

In diesem Szenario erkennt Azure Sentinel zuerst eine Warnung, dass Microsoft Defender Advanced Threat Protection erkannt hat, dass PowerShell eine verdächtige Netzwerkverbindung hergestellt hat und diese zu anomalen Aktivitäten führte, die von einer Palo Alto Network-Firewall erkannt wurden. Dies wurde durch das Konto {account name} mit der SID-ID {sid} um {time} initiiert. Die ausgehende IP-Adresse der Verbindung lautete {IndividualIp}. Anschließend wurden ungewöhnliche Aktivitäten von der Palo Alto Networks-Firewall um {TimeGenerated} erkannt. Dies deutet darauf hin, dass schädlicher Datenverkehr in Ihr Netzwerk eingedrungen ist. Die Ziel-IP-Adresse für den Netzwerkdatenverkehr lautet {DestinationIP}.

Dieses Szenario befindet sich zurzeit in der Public Preview.

### <a name="outbound-connection-to-ip-with-a-history-of-unauthorized-access-attempts-followed-by-anomalous-traffic-flagged-by-palo-alto-networks-firewall"></a>Ausgehende Verbindung mit IP mit einem Verlauf nicht autorisierter Zugriffsversuche, gefolgt von anomalem, durch die Palo Alto Networks-Firewall gekennzeichnetem Datenverkehr

In diesem Szenario erkennt Azure Sentinel eine Warnung, dass Microsoft Defender Advanced Threat Protection eine ausgehende Verbindung mit einer IP-Adresse erkannt hat, die bereits in der Vergangenheit nicht autorisierte Zugriffsversuche durchgeführt hat, die zu von der Palo Alto Networks-Firewall erkannten anomalen Aktivitäten geführt haben. Dies wurde durch das Konto {account name} mit der SID-ID {sid} um {time} initiiert. Die ausgehende IP-Adresse der Verbindung lautete {IndividualIp}. Danach wurden ungewöhnliche Aktivitäten von der Palo Alto Networks-Firewall um {TimeGenerated} erkannt. Dies deutet darauf hin, dass schädlicher Datenverkehr in Ihr Netzwerk eingedrungen ist. Die Ziel-IP-Adresse für den Netzwerkdatenverkehr lautet {DestinationIP}.

Dieses Szenario befindet sich zurzeit in der Public Preview.



## <a name="fusion-using-identity-protection-and-microsoft-cloud-app-security"></a>Fusion mithilfe von Identity Protection und Microsoft Cloud App Security

Per erweiterter Erkennung von mehrstufigen Angriffen unterstützt Azure Sentinel die folgenden Szenarien, in denen Anomalieereignisse aus Azure Active Directory Identity Protection und Microsoft Cloud App Security kombiniert werden:

- [Unmöglicher Ortswechsel zu atypischen Orten, gefolgt von anomaler Office 365-Aktivität](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität für unbekannten Standort, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität mit infiziertem Gerät, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität von anonymer IP-Adresse, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität eines Benutzers mit kompromittierten Anmeldeinformationen, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

Sie müssen den [Azure AD Identity Protection-Datenconnector](connect-azure-ad-identity-protection.md) und die [Cloud App Security](connect-cloud-app-security.md)-Connectors konfiguriert haben.

In den folgenden Beschreibungen wird mit Azure Sentinel der tatsächliche Wert aus Ihren Daten angezeigt, der auf dieser Seite in Form von Variablen in Klammern dargestellt ist. Dies ist beispielsweise der tatsächliche Anzeigename anstelle von \<*account name*> und die tatsächliche Zahl anstelle von \<*number*>.

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Unmöglicher Ortswechsel zu atypischen Orten, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einem unmöglichen Ortswechsel zu atypischen Orten (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Unmöglicher Ortswechsel zu atypischen Orten, der zu einer Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden.

- **Unmöglicher Ortswechsel zu atypischen Orten, der zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number*> Verwaltungsaktivitäten durchgeführt.

- **Unmöglicher Ortswechsel zu atypischen Orten mit Massenlöschung von Dateien**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> am atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Unmöglicher Ortswechsel zu atypischen Orten mit Massendownload von Dateien**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Unmöglicher Ortswechsel zu atypischen Orten, der zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung eine ungewöhnliche Menge (\<*number of activities*>) von Identitätswechselaktivitäten durchgeführt.

- **Unmöglicher Ortswechsel zu atypischen Orten mit Massenfreigabe von Dateien**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Unmöglicher Ortswechsel zu atypischen Orten, der zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität für unbekannten Standort, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität an einem unbekannten Ort (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden.

- **Anmeldeereignis von einem unbekannten Ort, das zu einer Exfiltration des Exchange Online-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> am unbekannten Ort \<*location*>, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis von einem unbekannten Ort, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis von einem unbekannten Ort, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis von einem unbekannten Ort, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis von einem unbekannten Ort, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>.
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis von einem unbekannten Ort, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis von einem unbekannten Ort, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität mit infiziertem Gerät, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität mit einem infizierten Gerät (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Anmeldeereignis mit einem infizierten Gerät, das zu einer Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von einem Gerät, das unter Umständen mit Schadsoftware infiziert ist, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis mit einem infizierten Gerät, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis mit einem infizierten Gerät, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis mit einem infizierten Gerät, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis mit einem infizierten Gerät, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis mit einem infizierten Gerät, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis mit einem infizierten Gerät, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität von anonymer IP-Adresse, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität von einer anonymen IP-Adresse (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu einer Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität eines Benutzers mit kompromittierten Anmeldeinformationen, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität eines Benutzers mit kompromittierten Anmeldeinformationen (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen verwendet wurden, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers. 
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die erweiterte Erkennung von mehrstufigen Angriffen erfahren haben, ist für Sie ggf. die folgende Schnellstartanleitung interessant. Darin wird veranschaulicht, wie Sie Einblicke in Ihre Daten und in potenzielle Bedrohungen erhalten: [Erste Schritte mit Azure Sentinel](quickstart-get-visibility.md).

Arbeiten Sie das folgende Tutorial durch, wenn Sie bereit zum Untersuchen der Incidents sind, die für Sie erstellt wurden: [Untersuchen von Vorfällen mit Azure Sentinel](tutorial-investigate-cases.md).

