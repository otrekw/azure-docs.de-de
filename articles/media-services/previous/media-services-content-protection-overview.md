---
title: Schützen Ihrer Inhalte mit Azure Media Services | Microsoft-Dokumentation
description: In diesem Artikel finden Sie eine Übersicht über den Inhaltsschutz mit Azure Media Services v2.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 08ebda8bdd17cc4d620792934e1299365d0ced71
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89257924"
---
# <a name="content-protection-overview"></a>Übersicht über den Inhaltsschutz

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

> [!NOTE]
> Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](../latest/index.yml) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-from-v2-to-v3.md).

Mit Azure Media Services können Sie Ihre Medien ab dem Zeitpunkt, an dem sie Ihren Computer verlassen, während des gesamten Prozesses der Speicherung, Verarbeitung und Übermittlung sichern. Mit Media Services können Sie Ihre zu übermittelnden Live- und On-Demand-Inhalte dynamisch mit Advanced Encryption Standard (AES-128) oder einem der drei wichtigsten DRM-Systeme verschlüsseln: Microsoft PlayReady, Google Widevine und Apple FairPlay. Media Services bietet auch einen Dienst für die Übermittlung von AES-Schlüsseln und DRM-Lizenzen (PlayReady, Widevine und FairPlay) an autorisierte Clients. 

Die folgende Abbildung veranschaulicht den Media Services-Workflow zum Schutz von Inhalten: 

![Schützen mit PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Dieser Artikel erläutert die relevanten Konzepte und Begriffe für den Inhaltsschutz mit Media Services. Der Artikel enthält auch Links zu Artikeln, in denen es um den Schutz von Inhalten geht. 

## <a name="dynamic-encryption"></a>Dynamische Verschlüsselung

Mit Media Services können Sie Ihre zu übermittelnden Inhalte dynamisch mit unverschlüsselten AES-Schlüsseln oder per DRM-Verschlüsselung über Microsoft PlayReady, Google Widevine oder FairPlay verschlüsseln. Inhalt, der mit einem unverschlüsselten AES-Schlüssel verschlüsselt und per HTTPS gesendet wird, bleibt verschlüsselt, bis er den Client erreicht. 

Jedes Verschlüsselungsverfahren unterstützt die folgenden Streamingprotokolle:
 
- AES: MPEG-DASH, Smooth Streaming und HLS
- PlayReady: MPEG-DASH, Smooth Streaming und HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Die Verschlüsselung für progressive Downloads wird nicht unterstützt. 

Wenn Sie ein Medienobjekt verschlüsseln möchten, müssen Sie dem Medienobjekt einen Inhaltsschlüssel für die Verschlüsselung zuordnen und außerdem eine Autorisierungsrichtlinie für den Schlüssel konfigurieren. Inhaltsschlüssel können angegeben oder von Media Services automatisch generiert werden.

Außerdem müssen Sie die Übermittlungsrichtlinie des Medienobjekts konfigurieren. Wenn Sie ein speicherverschlüsseltes Medienobjekt streamen möchten, geben Sie an, wie die Übermittlung erfolgen soll, indem Sie die Richtlinie für die Übermittlung von Medienobjekten konfigurieren.

Wenn ein Player einen Stream anfordert, verwendet Media Services den angegebenen Schlüssel, um den Inhalt dynamisch mit einem unverschlüsselten AES-Schlüssel oder per DRM-Verschlüsselung zu verschlüsseln. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel vom Media Services-Schlüsselübermittlungsdienst an. Um zu entscheiden, ob der Benutzer berechtigt ist, den Schlüssel zu erhalten, wertet der Dienst die Autorisierungsrichtlinien aus, die Sie für den Schlüssel angegeben haben.

## <a name="aes-128-clear-key-vs-drm"></a>Unverschlüsselter AES-128-Schlüssel im Vergleich zu DRM
Kunden fragen sich oft, ob sie die AES-Verschlüsselung oder ein DRM-System verwenden sollten. Der Hauptunterschied zwischen den beiden Systemen besteht darin, dass bei der AES-Verschlüsselung der Inhaltsschlüssel in unverschlüsseltem Format (als Klartext) an den Client übertragen wird. So kann der Schlüssel zur Verschlüsselung des Inhalts in einer Netzwerkablaufverfolgung auf dem Client als unverschlüsselter Text angezeigt werden. Die Verschlüsselung mit einem unverschlüsselten AES-128-Schlüssel eignet sich für Anwendungsfälle, in denen der Betrachter vertrauenswürdig ist (z.B. bei Unternehmensvideos, die innerhalb eines Unternehmens verteilt und von Mitarbeitern angesehen werden).

PlayReady, Widevine und FairPlay bieten alle eine höhere Verschlüsselungsstufe als die Verschlüsselung mit dem unverschlüsselten AES-128-Schlüssel. Der Inhaltsschlüssel wird in einem verschlüsselten Format übertragen. Zusätzlich erfolgt die Entschlüsselung in einer sicheren Umgebung auf Betriebssystemebene, wo ein Angriff durch einen böswilligen Benutzer schwieriger auszuführen ist. DRM wird für Anwendungsfälle empfohlen, in denen der Betrachter möglicherweise nicht vertrauenswürdig ist und Sie ein Höchstmaß an Sicherheit benötigen.

## <a name="storage-encryption"></a>Speicherverschlüsselung
Sie können die Speicherverschlüsselung verwenden, um Ihre unverschlüsselten Inhalte lokal per AES-Verschlüsselung mit 256 Bit zu verschlüsseln. Danach können Sie die Inhalte in Azure Storage hochladen, wo sie im Ruhezustand verschlüsselt gespeichert werden. Medienobjekte, die durch die Speicherverschlüsselung geschützt sind, werden vor der Codierung automatisch entschlüsselt und in einem verschlüsselten Dateisystem platziert. Vor dem Hochladen als neue Ausgabemedienobjekte können sie optional erneut verschlüsselt werden. Der primäre Anwendungsfall für Storage Encryption ist, wenn Sie Ihre qualitativ hochwertigen Eingabemediendateien mit starker Verschlüsselung beim Speichern im Ruhezustand auf dem Datenträger sichern möchten.

Um ein speicherverschlüsseltes Medienobjekt zu übermitteln, müssen Sie die Übermittlungsrichtlinie des Medienobjekts konfigurieren und Media Services so mitteilen, wie die Inhalte bereitgestellt werden sollen. Bevor das Medienobjekt gestreamt werden kann, entschlüsselt und streamt der Streamingserver den Inhalt mithilfe der angegebenen Übermittlungsrichtlinie (z.B. AES, Common Encryption oder unverschlüsselt).

## <a name="types-of-encryption"></a>Arten der Verschlüsselung
PlayReady und Widevine nutzen Common Encryption (AES-CTR-Modus). FairPlay nutzt die Verschlüsselung im AES-CBC-Modus. Die Verschlüsselung mit unverschlüsseltem AES-128-Schlüssel verwendet die Umschlagverschlüsselung.

## <a name="licenses-and-keys-delivery-service"></a>Dienst für die Übermittlung von Lizenzen und Schlüsseln
Media Services bietet einen Schlüsselübermittlungsdienst zum Übermitteln von DRM-Lizenzen (PlayReady, Widevine und FairPlay) und unverschlüsselten AES-Schlüsseln an autorisierte Clients. Zum Konfigurieren von Autorisierungs- und Authentifizierungsrichtlinien für Ihre Lizenzen und Schlüssel können Sie das [Azure-Portal](media-services-portal-protect-content.md), die REST-API oder das Media Services SDK für .NET verwenden.

## <a name="control-content-access"></a>Steuern des Zugriffs auf Inhalte
Sie können steuern, wer Zugriff auf Ihre Inhalte hat, indem Sie die Autorisierungsrichtlinie für Inhaltsschlüssel konfigurieren. Die Autorisierungsrichtlinie für Inhaltsschlüssel unterstützt entweder eine offene oder eine Tokenbeschränkung.

### <a name="open-authorization"></a>Offene Autorisierung
Bei einer offenen Autorisierungsrichtlinie wird der Inhaltsschlüssel an jeden Client gesendet (keine Einschränkung).

### <a name="token-authorization"></a>Tokenautorisierung
Bei einer Autorisierungsrichtlinie mit Tokeneinschränkung wird der Inhaltsschlüssel nur an einen Client gesendet, der in der Schlüssel-/Lizenzanforderung ein gültiges JWT (JSON Web Token) oder SWT (Simple Web Token) präsentiert. Dieses Token muss von einem Sicherheitstokendienst ausgestellt werden. Sie können Azure Active Directory als Sicherheitstokendienst verwenden oder einen benutzerdefinierten Sicherheitstokendienst bereitstellen. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Der Schlüsselübermittlungsdienst von Media Services gibt den angeforderten Schlüssel bzw. die Lizenz an den Client zurück, wenn das Token gültig ist und die Ansprüche im Token mit den für den Schlüssel bzw. die Lizenz konfigurierten Ansprüchen übereinstimmen.

Bei der Konfiguration der Richtlinie mit Tokeneinschränkung müssen die Parameter für den primären Verifizierungsschlüssel (primary verification key), den Aussteller (issuer) und die Zielgruppe (audience) angegeben werden. Der primäre Verifizierungsschlüssel enthält den Schlüssel, mit dem das Token signiert wurde. Der Aussteller ist der Sicherheitstokendienst, der das Token ausstellt. „Audience“ (manchmal auch „Scope“) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

### <a name="token-replay-prevention"></a>Verhindern der Tokenwiedergabe

Mit dem Feature zum *Verhindern der Tokenwiedergabe* können Media Services-Kunden einen Grenzwert festlegen, der bestimmt, wie oft ein Token zum Anfordern eines Schlüssels oder einer Lizenz verwendet werden kann. Der Kunde kann einen Anspruch vom Typ `urn:microsoft:azure:mediaservices:maxuses` im Token hinzufügen, wobei der Wert die Häufigkeit ist, mit der das Token für den Erwerb einer Lizenz oder eines Schlüssels verwendet werden kann. Bei allen nachfolgenden Anforderungen mit demselben Token für die Schlüsselbereitstellung wird eine nicht autorisierte Antwort zurückgegeben. Informationen zum Hinzufügen des Anspruchs finden Sie im [DRM-Beispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601).
 
#### <a name="considerations"></a>Überlegungen

* Kunden müssen die Kontrolle über die Tokengenerierung haben. Der Anspruch muss in das Token selbst eingefügt werden.
* Bei Verwendung dieser Funktion werden Anforderungen mit Token, deren Ablaufzeit mehr als eine Stunde vor dem Zeitpunkt liegt, zu dem die Anforderung empfangen wird, werden mit der Antwort „nicht autorisiert“ abgelehnt.
* Token werden durch ihre Signatur eindeutig identifiziert. Jede Änderung an der Nutzlast (z. B. eine Aktualisierung der Ablaufzeit oder des Anspruchs) ändert die Signatur des Tokens, und es zählt als ein neues Token, das bei der Schlüsselbereitstellung zuvor noch nicht aufgetreten ist.
* Die Wiedergabe schlägt fehl, wenn das Token den vom Kunden festgelegten `maxuses`-Wert überschritten hat.
* Diese Funktion kann für alle vorhandenen geschützten Inhalte verwendet werden (nur das ausgegebene Token muss geändert werden).
* Diese Funktion kann sowohl mit JWT als auch mit SWT verwendet werden.

## <a name="streaming-urls"></a>Streaming-URLs
Falls Ihr Medienobjekt mit mehreren DRM-Systemen verschlüsselt wurde, verwenden Sie in der Streaming-URL ein Verschlüsselungstag: (format='m3u8-aapl', encryption='xxx').

Es gelten die folgenden Bedingungen:

* Es kann höchstens ein Verschlüsselungstyp angegeben werden.
* Der Verschlüsselungstyp muss nicht in der URL angegeben werden, wenn auf das Medienobjekt nur eine einzelne Verschlüsselung angewendet wurde.
* Beim Verschlüsselungstyp wird die Groß-/Kleinschreibung nicht beachtet.
* Folgende Verschlüsselungstypen können angegeben werden:

  * **cenc:** Für PlayReady oder Widevine (Common Encryption)
  * **cbcs-aapl:** Für FairPlay (AES-CBC-Verschlüsselung)
  * **cbc:** Für AES-Umschlagverschlüsselung

## <a name="additional-notes"></a>Zusätzliche Hinweise

* Widevine ist ein von Google Inc. bereitgestellter Dienst, der den Vertragsbedingungen und der Datenschutzrichtlinie von Google, Inc. unterliegt.

## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln werden die nächsten Schritte für den Einstieg in den Inhaltsschutz beschrieben:

* [Schutz durch Speicherverschlüsselung](media-services-rest-storage-encryption.md)
* [Schutz durch AES-Verschlüsselung](media-services-protect-with-aes128.md)
* [Schutz mit PlayReady und/oder Widevine](media-services-protect-with-playready-widevine.md)
* [Schutz mit FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Verwandte Links

* [JWT-Tokenauthentifizierung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)
* [Integrate Azure Media Services OWIN MVC-based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/) (Integrieren einer Azure Media Services-OWIN MVC-basierten App in Azure Active Directory und Einschränken der Übermittlung von Inhaltsschlüsseln auf Grundlage von JWT-Ansprüchen)

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
