---
title: Erste Schritte mit der Azure AD-Authentifizierung
description: Erfahren Sie, wie Sie auf die Azure Active Directory-Authentifizierung (Azure AD) zugreifen, um die Azure Media Services-API zu nutzen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/21/2020
ms.author: juliako
ms.openlocfilehash: 9788c4663908497b51fbaaf7f824125d857e7c81
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83774411"
---
# <a name="get-credentials-to-access-media-services-api"></a>Abrufen von Anmeldeinformationen für den Zugriff auf die Media Services-API  

Wenn Sie für den Zugriff auf die Azure Media Services-API die Azure AD-Authentifizierung verwenden, stehen Ihnen zwei Authentifizierungsoptionen zur Verfügung:

- **Dienstprinzipalauthentifizierung** (empfohlen)

    Dient zum Authentifizieren eines Diensts. Anwendungen, für die diese Authentifizierungsmethode normalerweise verwendet wird, sind Apps, die Daemondienste, Dienste der mittleren Ebene oder geplante Aufträge ausführen: Web-Apps, Funktions-Apps, Logic Apps, APIs oder Microservices.
- **Benutzerauthentifizierung**

    Dient zum Authentifizieren einer Person, die die App für die Interaktion mit Media Services-Ressourcen verwendet. Die interaktive Anwendung soll den Benutzer zuerst zur Eingabe seiner Anmeldeinformationen auffordern. Ein Beispiel hierfür ist eine Verwaltungskonsolen-App, die von autorisierten Benutzern zum Überwachen von Codierungsaufträgen oder Livestreaming verwendet wird. 

In diesem Artikel werden Schritte zum Abrufen von Anmeldeinformationen für den Zugriff auf Media Services API beschrieben. Sie können aus folgenden Registerkarten auswählen:

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto. Beginnen Sie mit einer [kostenlosen Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/), falls Sie kein Konto haben. 
- Media Services-Konto. Weitere Informationen finden Sie unter [Erstellen eines Azure Media Services-Kontos mithilfe des Azure-Portals](create-account-howto.md).

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

### <a name="api-access"></a>API-Zugriff 

Auf der Seite **API-Zugriff** können Sie die Authentifizierungsmethode auswählen, die Sie für die Verbindung mit der API verwenden möchten. Auf der Seite befinden sich auch die Werte, die Sie zum Herstellen einer Verbindung mit der API benötigen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Media Services-Konto aus.
2. Wählen Sie, wie Sie eine Verbindung mit der Media Services-API herstellen möchten.
3. Wählen Sie unter **Verbindung mit Media Services-API herstellen** die Version der Media Services-API aus, mit der Sie eine Verbindung herstellen möchten. (V3 ist die aktuelle Version des Diensts.)

### <a name="service-principal-authentication--recommended"></a>Dienstprinzipalauthentifizierung (empfohlen)

Authentifiziert einen Dienst mit einer App und einem Geheimnis von Azure Active Directory (Azure AD). Dies wird für alle Dienste der mittleren Ebene empfohlen, die die Media Services-API aufrufen. Beispiele sind Web-Apps, Funktionen, Logik-Apps, APIs und Microservices. Dies ist die empfohlene Authentifizierungsmethode.

#### <a name="manage-your-azure-ad-app-and-secret"></a>Verwalten von Azure AD-App und -Geheimnis

Im Abschnitt **AAD-App und Geheimnis verwalten** können Sie eine neue Azure AD-App auswählen oder erstellen und ein Geheimnis generieren. Aus Sicherheitsgründen kann das Geheimnis nicht mehr angezeigt werden, nachdem das Blatt geschlossen wurde. Die Anwendung verwendet die Anwendungs-ID und das Geheimnis für die Authentifizierung, um ein gültiges Token für Media Services zu erhalten.

Stellen Sie sicher, dass Sie über ausreichende Berechtigungen verfügen, um eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren und die Anwendung einer Rolle in Ihrem Azure-Abonnement zuzuweisen. Weitere Informationen finden Sie unter [Erforderliche Berechtigungen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions).

#### <a name="connect-to-media-services-api"></a>Herstellen einer Verbindung mit der Media Services-API

Über **Verbindung mit Media Services-API herstellen** erhalten Sie Werte, mit denen Sie Ihre Dienstprinzipalanwendung verbinden können. Sie können Textwerte abrufen oder die JSON- oder XML-Blöcke kopieren.

### <a name="user-authentication"></a>Benutzerauthentifizierung

Mithilfe dieser Option kann ein Mitarbeiter oder ein Mitglied einer Azure Active Directory-Instanz authentifiziert werden, der für die Interaktion mit Media Services-Ressourcen eine App verwendet. Die interaktive Anwendung sollte den Benutzer zuerst zur Eingabe seiner Anmeldeinformationen auffordern. Diese Authentifizierungsmethode sollte nur für Managementanwendungen verwendet werden.

#### <a name="connect-to-media-services-api"></a>Herstellen einer Verbindung mit der Media Services-API

Kopieren Sie Ihre Anmeldeinformationen, um Ihre Benutzeranwendung über den Abschnitt **Verbindung mit Media Services-API herstellen** zu verbinden. Sie können Textwerte abrufen oder die JSON- oder XML-Blöcke kopieren.

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

---

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Hochladen, Codieren und Streamen von Videos mit Media Services v3](stream-files-tutorial-with-api.md)
