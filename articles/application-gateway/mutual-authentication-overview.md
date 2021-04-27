---
title: Übersicht über die gegenseitige Authentifizierung in Azure Application Gateway
description: Dieser Artikel stellt eine Übersicht über die gegenseitige Authentifizierung in Application Gateway dar.
services: application-gateway
author: mscatyao
ms.service: application-gateway
ms.date: 03/30/2021
ms.topic: conceptual
ms.author: caya
ms.openlocfilehash: a63697c2c9c32dfb48e77248a5e7a601677b8b3e
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106230867"
---
# <a name="overview-of-mutual-authentication-with-application-gateway-preview"></a>Übersicht über die gegenseitige Authentifizierung in Application Gateway (Vorschau)

Gegenseitige Authentifizierung oder Clientauthentifizierung ermöglicht es dem Application Gateway, die vom Client gesendeten Anforderungen zu authentifizieren. In der Regel authentifiziert nur der Client den Application Gateway. Bei der gegenseitigen Authentifizierung können sowohl der Client als auch der Application Gateway einander authentifizieren. 

> [!NOTE]
> Wir empfehlen Ihnen die Nutzung von TLS 1.2 mit gegenseitiger Authentifizierung, da TLS 1.2 in Zukunft vorgeschrieben sein wird. 

## <a name="mutual-authentication"></a>Gegenseitige Authentifizierung

Application Gateway unterstützt die zertifikatbasierte gegenseitige Authentifizierung, bei der Sie ein oder mehrere vertrauenswürdige Client-ZS-Zertifikate in den Application Gateway hochladen können. Das Gateway verwendet dieses Zertifikat zum Authentifizieren des Clients, der eine Anforderung an das Gateway sendet. Angesichts des vermehrten Einsatz des IoT und erweiterter Sicherheitsanforderungen in allen Branchen bietet die gegenseitige Authentifizierung Ihnen die Möglichkeit, zu verwalten und zu steuern, welche Clients mit ihrer Application Gateway kommunizieren können. 

Zum Konfigurieren der gegenseitigen Authentifizierung muss ein vertrauenswürdiges Client-ZS-Zertifikat als Teil der Clientauthentifizierung eines SSL-Profils hochgeladen werden. Das SSL-Profil muss dann einem Listener zugeordnet werden, um die Konfiguration der gegenseitigen Authentifizierung abzuschließen. Das Clientzertifikat, das Sie hochladen, muss immer ein Stammzertifikat einer Zertifizierungsstelle (ZS) enthalten. Sie können auch eine Zertifikatkette hochladen, die beliebig viele ZS-Zwischenzertifikate enthalten kann. Sie muss aber auch ein ZS-Stammzertifikat enthalten. 

Wenn das Clientzertifikat beispielsweise ein ZS-Stammzertifikat, mehrere ZS-Zwischenzertifikate und ein Blattzertifikat enthält, sollten Sie darauf achten, dass das ZS-Stammzertifikat und das ZS-Zwischenzertifikat in einer Datei in Application Gateway hochgeladen werden. Weitere Informationen zum Extrahieren von vertrauenswürdigen ZS-Clientzertifikaten finden Sie unter [Extrahieren von vertrauenswürdigen ZS-Clientzertifikaten](./mutual-authentication-certificate-management.md).

Wenn Sie eine Zertifikatkette mit ZS-Stamm- und ZS-Zwischenzertifikaten hochladen, muss die Zertifikatkette als PEM- oder CER-Datei in das Gateway hochgeladen werden. 

> [!NOTE] 
> Die gegenseitige Authentifizierung ist nur für Standard_v2-und WAF_v2-SKUs verfügbar. 

### <a name="certificates-supported-for-mutual-authentication"></a>Für die gegenseitige Authentifizierung unterstützte Zertifikate

Application Gateway unterstützt folgende Typen von Zertifikaten:

- CA-Zertifikat: Ein CA-Zertifikat ist ein digitales Zertifikat, das von einer Zertifizierungsstelle (Certificate Authority, CA) ausgestellt wurde.
- Selbstsignierte Zertifikate: Clientbrowser vertrauen diesen Zertifikaten nicht und warnen den Benutzer, dass das virtuelle Zertifikat des Dienstes nicht Teil einer vertrauenswürdigen Kette ist. Selbstsignierte Zertifikate eignen sich gut für Tests oder Umgebungen, in denen Administratoren die Clients kontrollieren und Sicherheitswarnungen des Browsers sicher umgehen können. Produktionsworkloads sollten niemals selbstsignierte ZS-Zertifikate verwenden.

Weitere Informationen zum Einrichten der gegenseitigen Authentifizierung finden Sie unter [Konfigurieren der gegenseitigen Authentifizierung mit Application Gateway](./mutual-authentication-portal.md).

> [!IMPORTANT]
> Stellen Sie sicher, dass Sie bei der gegenseitigen Authentifizierung die gesamte vertrauenswürdige ZS-Clientzertifikatkette in die Application Gateway hochladen. 

## <a name="additional-client-authentication-validation"></a>Zusätzliche Überprüfung der Clientauthentifizierung

### <a name="verify-client-certificate-dn"></a>Clientzertifikat-DN überprüfen

Sie können den unmittelbaren Aussteller des Clientzertifikats überprüfen und nur dem Application Gateway gestatten, diesem Aussteller zu vertrauen. Diese Option ist standardmäßig deaktiviert, aber Sie können sie über das Portal, PowerShell oder Azure CLI aktivieren. 

Wenn Sie die Application Gateway den sofortigen Aussteller des Clientzertifikats überprüfen lassen möchten, können Sie anhand dieser Anleitung ermitteln, wie der Aussteller-DN des Clientzertifikats aus den hochgeladenen Zertifikaten extrahiert wird. 
* **Szenario 1:** Die Zertifikatkette umfasst: Stammzertifikat - Zwischenzertifikat - Blattzertifikat 
    * Application Gateway wird den Namen des Antragstellers des *Zwischenzertifikats* als Aussteller-DN des Clientzertifikats extrahieren und dahingehend überprüfen. 
* **Szenario 2:** Die Zertifikatkette umfasst: Stammzertifikat - Zwischenzertifikat1 - Zwischenzertifikat2 - Blattzertifikat
    * Der Name des Antragstellers des *Zwischenzertifikat2s* wird als Aussteller-DN des Clientzertifikats extrahiert und dahingehend überprüft. 
* **Szenario 3:** Die Zertifikatkette umfasst: Stammzertifikat - Blattzertifikat 
    * Der Antragstellername des *Stammzertifikats* wird extrahiert und als Aussteller-DN des Client Zertifikats verwendet.
* **Szenario 4:** Mehrere Zertifikatketten derselben Länge in derselben Datei. Kette 1 umfasst: Stammzertifikat - Zwischenzertifikat1 - Blattzertifikat Kette 2 umfasst: Stammzertifikat - Zwischenzertifikat2 - Blattzertifikat. 
    * Der Antragstellername des *Zwischenzertifikats1* wird als Aussteller-DN des Client Zertifikats extrahiert.  
* **Szenario 5:** Mehrere Zertifikatketten mit unterschiedlichen Längen in derselben Datei. Kette 1 umfasst: Stammzertifikat - Zwischenzertifikat1 - Blattzertifikat Kette 2 umfasst: Stammzertifikat - Zwischenzertifikat2 - Zwischenzertifikat3 - Blattzertifikat 
    * Der Antragstellername des *Zwischenzertifikats3* wird extrahiert und als Aussteller-DN des Client Zertifikats verwendet. 

> [!IMPORTANT]
> Es wird empfohlen, nur eine Zertifikatkette pro Datei hochzuladen. Dies ist besonders wichtig, wenn Sie das Überprüfen von Client Zertifikat-DN aktivieren. Wenn Sie mehrere Zertifikatketten in einer Datei hochladen, werden Sie auf Szenario 4 oder 5 treffen und haben möglicherweise später Probleme, wenn das Clientzertifikat nicht mit der Aussteller-DN des Clientzertifikats übereinstimmt, das von Application Gateway aus den Ketten extrahiert wurde. 

Weitere Informationen zum Extrahieren von vertrauenswürdigen ZS-Client-Zertifikatketten für die Nutzung finden Sie unter [Exportieren von vertrauenswürdigen ZS-Client-Zertifikatketten](./mutual-authentication-certificate-management.md).

## <a name="server-variables"></a>Servervariablen 

Bei der gegenseitigen Authentifizierung gibt es zusätzliche Servervariablen, mit denen Sie Informationen über das Clientzertifikat an die Back-End-Server hinter dem Application Gateway übergeben können. Weitere Informationen darüber, welche Servervariablen verfügbar sind und wie sie verwendet werden, finden Sie unter [Servervariablen](./rewrite-http-headers-url.md#mutual-authentication-server-variables-preview).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit der gegenseitigen Authentifizierung vertraut gemacht haben, wechseln Sie zu[Konfigurieren der Anwendungs-Gateway mit gegenseitiger Authentifizierung in PowerShell](./mutual-authentication-powershell.md), um eine Anwendungs-Gateway mithilfe der gegenseitigen Authentifizierung zu erstellen. 

