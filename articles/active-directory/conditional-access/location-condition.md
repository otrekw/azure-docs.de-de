---
title: Standortbedingung beim bedingten Zugriff in Azure Active Directory
description: Erfahren Sie, wie Sie die auf dem Netzwerkstandort eines Benutzers beruhende Standortbedingung zum Steuern des Zugriffs auf Ihre Cloud-App verwenden können.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.workload: identity
ms.date: 05/28/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.custom: contperfq4
ms.openlocfilehash: 781d8b89dd1b7fa6b2ed9707f6d4c485b4abdf20
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220593"
---
# <a name="using-the-location-condition-in-a-conditional-access-policy"></a>Verwenden der Standortbedingung in einer Richtlinie für bedingten Zugriff 

Wie im [Übersichtsartikel](overview.md) erläutert wurde, sind Richtlinien für bedingten Zugang in ihrer grundlegendsten Form eine If-Then-Anweisung, die Signale kombiniert, um Entscheidungen zu treffen und Organisationsrichtlinien zu erzwingen. Eines dieser Signale, das in den Entscheidungsfindungsprozess integriert werden kann, ist der Netzwerkstandort.

![Konzeptionelles bedingtes Signal plus Entscheidung zum Erzwingen](./media/location-condition/conditional-access-signal-decision-enforcement.png)

Organisationen können diesen Netzwerkstandort für gängige Aufgaben wie die folgenden nutzen: 

- Das Vorschreiben von mehrstufiger Authentifizierung für Benutzer beim Zugriff auf einen Dienst, wenn sie sich außerhalb des Unternehmensnetzwerks befinden
- Das Blockieren des Zugriffs auf einen Dienst für Benutzer, die sich in bestimmten Ländern oder Regionen aufhalten.

Der Netzwerkstandort wird über die öffentliche IP-Adresse bestimmt, die ein Client in Azure Active Directory angibt. Richtlinien für bedingten Zugriff werden standardmäßig auf alle IPv4- und IPv6-Adressen angewendet. 

> [!TIP]
> IPv6-Bereiche werden nur in der Schnittstelle **[Benannter Standort (Vorschau)](#preview-features)** unterstützt. 

## <a name="named-locations"></a>Benannte Standorte

Standorte werden im Azure-Portal unter **Azure Active Directory** > **Sicherheit** > **Bedingter Zugriff** > **Benannte Standorte** angegeben. Diese benannten Netzwerkstandorte können Standorte wie z. B. die Netzwerkbereiche des Hauptsitzes einer Organisation, VPN-Netzwerkbereiche oder Bereiche umfassen, die Sie blockieren möchten. 

![Benannte Standorte im Azure-Portal](./media/location-condition/new-named-location.png)

Um einen Standort zu konfigurieren, müssen Sie mindestens einen **Namen** und den IP-Adressbereich angeben. 

Die Anzahl von benannten Standorten, die Sie konfigurieren können, wird durch die Größe des zugehörigen Objekts in Azure AD eingeschränkt. Sie können Standorte basierend auf den folgenden Einschränkungen konfigurieren:

- Einen benannten Standort mit bis zu 1.200 IPv4-Bereichen.
- Maximal 90 benannte Standorte, denen jeweils ein IP-Bereich zugewiesen ist.

> [!TIP]
> IPv6-Bereiche werden nur in der Schnittstelle **[Benannter Standort (Vorschau)](#preview-features)** unterstützt. 

### <a name="trusted-locations"></a>Vertrauenswürdige Standorte

Beim Erstellen eines Netzwerkstandorts hat ein Administrator die Option, den Standort als vertrauenswürdig zu markieren. 

![Vertrauenswürdige Standorte im Azure-Portal](./media/location-condition/new-trusted-location.png)

Diese Option kann in Richtlinien für bedingten Zugriff einfließen, wenn Sie z. B. eine Registrierung für die mehrstufige Authentifizierung von einem vertrauenswürdigen Netzwerkstandort als erforderlich festlegen. Sie wird auch bei der Risikoberechnung von Azure AD Identity Protection berücksichtigt und senkt das Anmelderisiko eines Benutzers, wenn die Anforderung von einem als vertrauenswürdig gekennzeichneten Standort aus erfolgt.

### <a name="countries-and-regions"></a>Länder und Regionen

Einige Organisationen können sich dafür entscheiden, IP-Grenzen für ganze Länder oder Regionen als benannte Standorte für Richtlinien für bedingten Zugriff zu definieren. Sie können mithilfe dieser Standorte unnötigen Datenverkehr blockieren, wenn sie wissen, dass gültige Benutzer niemals aus einem Standort wie etwa Nordkorea stammen. Diese Zuordnungen von IP-Adressen zu einem Land werden in regelmäßigen Abständen aktualisiert. 

> [!NOTE]
> Länder umfassen keine IPv6-Adressbereiche, nur bekannte IPv4-Adressbereiche.

![Erstellen eines neuen Standorts basierend auf Land oder Region im Azure-Portal](./media/location-condition/new-named-location-country-region.png)

#### <a name="include-unknown-areas"></a>Einschließen unbekannter Bereiche

Einige IP-Adressen sind weder einem bestimmten Land noch einer bestimmten Region zugeordnet. Um diese IP-Standorte zu erfassen, aktivieren Sie beim Definieren eines Standorts die Option **Unbekannte Bereiche einschließen**. Mithilfe dieser Option können Sie auswählen, ob der benannte Standort diese IP-Adressen umfassen soll. Verwenden Sie diese Einstellung, wenn die Richtlinie für den benannten Standort auch für unbekannte Standorte gelten soll.

### <a name="configure-mfa-trusted-ips"></a>Konfigurieren durch MFA bestätigter IP-Adressen

Ferner können Sie in den [Einstellungen für den mehrstufigen Authentifizierungsdienst](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx) IP-Adressbereiche konfigurieren, die das lokale Intranet Ihrer Organisation darstellen. Mithilfe dieser Funktion können Sie bis zu 50 IP-Adressbereiche konfigurieren. Die IP-Adressbereiche müssen im CIDR-Format angegeben werden. Weitere Informationen finden Sie unter [Vertrauenswürdige IP-Adressen](../authentication/howto-mfa-mfasettings.md#trusted-ips).  

Wenn Sie vertrauenswürdige IP-Adressen konfiguriert haben, werden Sie als **Für MFA vertrauenswürdige IPs** in der Liste der Standorte für die Standortbedingung angezeigt.

### <a name="skipping-multi-factor-authentication"></a>Überspringen der mehrstufigen Authentifizierung

Auf der Einstellungsseite für den mehrstufigen Authentifizierungsdienst können Sie Benutzer aus dem Unternehmensintranet identifizieren, indem Sie **Für Anforderungen von Partnerbenutzern in meinem Intranet die mehrstufige Authentifizierung überspringen** aktivieren. Diese Einstellung gibt an, dass der Anspruch innerhalb des Unternehmensnetzwerks, der von AD FS ausgestellt wird, als vertrauenswürdig angesehen und dazu verwendet wird, den Benutzer als innerhalb des Unternehmensnetzwerks ansässig zu erkennen. Weitere Informationen finden Sie unter [Aktivieren des Features vertrauenswürdige IPs beim bedingten Zugriff](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Nach dem Aktivieren wird diese Option, einschließlich des benannten Standorts **Für MFA vertrauenswürdige IPs**, auf alle Richtlinien angewendet, für die diese Option ausgewählt ist.

Für mobile und Desktopanwendungen mit langer Sitzungslebensdauer wird der bedingte Zugriff in regelmäßigen Abständen neu ausgewertet. Die Standardeinstellung ist einmal pro Stunde. Wenn der Anspruch innerhalb des Unternehmensnetzwerks nur zum Zeitpunkt der erstmaligen Authentifizierung ausgegeben wird, verfügt Azure AD möglicherweise nicht über eine Liste der vertrauenswürdigen IP-Bereiche. In diesem Fall ist die Bestimmung, ob sich der Benutzer noch im Unternehmensnetzwerk befindet, schwieriger:

1. Überprüfen Sie, ob die IP-Adresse des Benutzers in einem der vertrauenswürdigen IP-Bereiche liegt.
1. Überprüfen Sie, ob die ersten drei Oktette der IP-Adresse des Benutzers mit den ersten drei Oktetten der IP-Adresse der ersten Authentifizierung übereinstimmen. Die IP-Adresse wird mit der ersten Authentifizierung zu dem Zeitpunkt verglichen, zu dem der Anspruch innerhalb des Unternehmensnetzwerks ursprünglich ausgestellt und der Benutzerstandort überprüft wurde.

Wenn bei beiden Schritten ein Fehler auftritt, wird ein Benutzer nicht mehr als vertrauenswürdige IP angesehen.

## <a name="preview-features"></a>Vorschaufunktionen

Zusätzlich zum allgemein verfügbaren Feature für benannte Standorte gibt es das Feature „Benannte Standorte (Vorschau)“. Sie können auf „Benannte Standorte (Vorschau)“ zugreifen, indem Sie oben auf dem Blatt des aktuellen benannten Standorts auf das Banner klicken.

![Ausprobieren von „Benannte Standorte (Vorschau)“](./media/location-condition/preview-features.png)

„Benannte Standorte (Vorschau)“ bietet folgende Möglichkeiten:

- Konfigurieren von bis zu 195 benannten Standorten
- Konfigurieren von bis zu 2.000 IP-Adressbereichen pro benanntem Standort
- Konfigurieren von IPv6-Adressen

Darüber hinaus wurden einige zusätzliche Überprüfungen eingefügt, um fehlerhafte Konfigurationen zu vermeiden.

- Private IP-Adressbereiche können nicht länger konfiguriert werden.
- Die Anzahl von IP-Adressen, die in einen Bereich eingeschlossen werden können, ist begrenzt. Beim Konfigurieren eines IP-Adressbereichs sind nur CIDR-Masken größer als /8 zulässig.

Mit der Vorschau stehen bei der Erstellung nun zwei Optionen zur Verfügung: 

- **Länder (Standort)**
- **IP-Adressbereiche (Standort)**

> [!NOTE]
> Länder umfassen keine IPv6-Adressbereiche, nur bekannte IPv4-Adressbereiche.

![Schnittstelle „Benannte Standorte (Vorschau)“](./media/location-condition/named-location-preview.png)

## <a name="location-condition-in-policy"></a>Standortbedingung in Richtlinie

Beim Konfigurieren der Standortbedingung können Sie zwischen diesen Optionen wählen:

- Beliebiger Standort
- Alle vertrauenswürdigen Standorte
- Ausgewählte Standorte

### <a name="any-location"></a>Beliebiger Standort

Standardmäßig bewirkt das Aktivieren von **Alle Standorte**, dass eine Richtlinie auf alle IP-Adressen angewendet wird, was jede beliebige Adresse im Internet bedeutet. Diese Einstellung ist nicht auf IP-Adressen beschränkt, die von Ihnen als benannter Standort konfiguriert wurden. Wenn Sie **Alle Standorte** aktivieren, können Sie bestimmte Standorte trotzdem noch von einer Richtlinie ausschließen. Beispielsweise können Sie eine Richtlinie auf alle Standorte mit Ausnahme vertrauenswürdiger Standorte anwenden, um den Geltungsbereich auf alle Standorte mit Ausnahme des Unternehmensnetzwerks festzulegen.

### <a name="all-trusted-locations"></a>Alle vertrauenswürdigen Standorte

Diese Option gilt für:

- Alle Standorte, die als vertrauenswürdiger Standort gekennzeichnet wurden
- **Für MFA vertrauenswürdige IPs** (sofern konfiguriert)

### <a name="selected-locations"></a>Ausgewählte Standorte

Mit dieser Option können Sie einen oder mehrere benannte Standorte auswählen. Damit eine Richtlinie mit dieser Einstellung gilt, muss ein Benutzer von einem der ausgewählten Standorte aus eine Verbindung herstellen. Wenn Sie auf **Auswählen** klicken, wird das Steuerelement für die Auswahl von benannten Netzwerken geöffnet und zeigt die Liste der benannten Netzwerke an. In dieser Liste ist außerdem zu sehen, ob der Netzwerkstandort als vertrauenswürdig gekennzeichnet wurde. Der benannte Standort, der als **Für MFA vertrauenswürdige IPs** bezeichnet ist, wird zum Einschließen der IP-Einstellungen verwendet, die auf der Einstellungsseite des mehrstufigen Authentifizierungsdiensts konfiguriert werden können.

## <a name="what-you-should-know"></a>Wichtige Informationen

### <a name="when-is-a-location-evaluated"></a>Wann wird ein Standort ausgewertet?

Richtlinien für bedingten Zugriff werden in diesen Situationen ausgewertet:

- Ein Benutzer meldet sich zum ersten Mal an einer Web-App, mobilen Anwendung oder Desktopanwendung an.
- Für eine mobile Anwendung oder Desktopanwendung, für die die moderne Authentifizierung verwendet wird, wird ein Aktualisierungstoken zum Beschaffen eines neuen Zugriffstokens genutzt. Standardmäßig erfolgt diese Überprüfung einmal pro Stunde.

Für mobile Anwendungen und Desktopanwendungen mit moderner Authentifizierung bedeutet diese Überprüfung, dass eine Änderung des Netzwerkstandorts innerhalb von einer Stunde erkannt wird. Wenn für mobile Anwendungen und Desktopanwendungen keine moderne Authentifizierung genutzt wird, wird die Richtlinie auf jede Tokenanforderung angewendet. Die Häufigkeit der Anforderung kann basierend auf der Anwendung variieren. Ebenso wird die Richtlinie für Webanwendungen bei der ersten Anmeldung angewendet und gilt für die Lebensdauer der Webanwendungssitzung. Aufgrund von Unterschieden bei den Sitzungslebensdauern von Anwendungen variiert auch die Zeit zwischen den Richtlinienauswertungen. Die Richtlinie wird jedes Mal angewendet, wenn die Anwendung ein neues Anmeldetoken anfordert.

Standardmäßig stellt Azure AD stündlich ein Token aus. Nach dem Verlassen des Unternehmensnetzwerks wird die Richtlinie für Anwendungen mit moderner Authentifizierung innerhalb einer Stunde durchgesetzt.

### <a name="user-ip-address"></a>Benutzer-IP-Adresse

Die IP-Adresse, die in der Auswertung der Richtlinie verwendet wird, ist die öffentliche IP-Adresse des Benutzers. Bei Geräten in einem privaten Netzwerk ist diese IP-Adresse nicht die Client-IP des Geräts des Benutzers im Intranet, sondern die vom Netzwerk für das Herstellen der Verbindung mit dem öffentlichen Internet verwendete Adresse.

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Massenhoch- und -herunterladen von benannten Standorten

Beim Erstellen oder Aktualisieren benannter Standorte können Sie für eine Massenaktualisierung eine CSV-Datei mit den IP-Adressbereichen hoch- oder herunterladen. Bei einem Upload werden die IP-Adressbereiche in der Liste durch die Bereiche aus der Datei ersetzt. Jede Zeile der Datei enthält einen IP-Adressbereich im CIDR-Format.

### <a name="cloud-proxies-and-vpns"></a>Cloud-Proxys und VPNs

Wenn Sie einen in der Cloud gehosteten Proxy oder eine VPN-Lösung verwenden, ist die von Azure AD bei der Auswertung einer Richtlinie verwendete IP-Adresse die IP-Adresse des Proxys. Der XFF-Header (X-Forwarded-For), der die öffentliche IP-Adresse des Benutzers enthält, wird nicht verwendet, da es keine Überprüfung gibt, ob er aus einer vertrauenswürdigen Quelle stammt, sodass er ein Verfahren zum Fälschen einer IP-Adresse darstellen kann.

Wenn ein Cloud-Proxy zum Einsatz kommt, kann eine Richtlinie verwendet werden, mit der die Verwendung von der Domäne beigetretenen Geräten oder des Anspruchs aus dem internen Unternehmensnetzwerk von AD FS vorgeschrieben wird.

### <a name="api-support-and-powershell"></a>API-Unterstützung und PowerShell

API und PowerShell werden für benannte Standorte noch nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie wissen möchten, wie Sie eine Richtlinie für bedingten Zugriff konfigurieren, finden Sie weitere Informationen im Artikel [Erstellen einer Richtlinie für bedingten Zugriff](concept-conditional-access-policies.md).
- Suchen Sie nach einer Beispielrichtlinie mit Verwendung der Standortbedingung? Lesen Sie den Artikel [Bedingter Zugriff: Blockieren des Zugriffs nach Standort](howto-conditional-access-policy-location.md).
