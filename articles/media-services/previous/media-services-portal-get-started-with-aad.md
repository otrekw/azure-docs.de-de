---
title: Erste Schritte mit der Azure AD-Authentifizierung mithilfe des Azure-Portals| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das Azure-Portal für den Zugriff auf die Azure Active Directory (Azure AD)-Authentifizierung verwenden, um die Azure Media Services-API zu nutzen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: d9ad439fe3f41d9e2634fbf9a76cfd21114a5dbc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103013056"
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a>Erste Schritte mit der Azure AD-Authentifizierung mithilfe des Azure-Portals

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](../latest/index.yml) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-v-2-v-3-migration-introduction.md).

Erfahren Sie, wie Sie das Azure-Portal für den Zugriff auf die Azure Active Directory (Azure AD)-Authentifizierung verwenden, um auf die Azure Media Services-API zuzugreifen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto. Beginnen Sie mit einer [kostenlosen Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/), falls Sie kein Konto haben. 
- Media Services-Konto. Weitere Informationen finden Sie unter [Erstellen eines Azure Media Services-Kontos mithilfe des Azure-Portals](media-services-portal-create-account.md).

Wenn Sie die Azure AD-Authentifizierung mit Azure Media Services nutzen, stehen Ihnen zwei Authentifizierungsoptionen zur Verfügung:

- **Dienstprinzipalauthentifizierung**. Dient zum Authentifizieren eines Diensts. Anwendungen, für die diese Authentifizierungsmethode normalerweise verwendet wird, sind Apps, die Daemondienste, Dienste der mittleren Ebene oder geplante Aufträge ausführen: Web-Apps, Funktions-Apps, Logic Apps, APIs oder Microservices.
- **Benutzerauthentifizierung**. Dient zum Authentifizieren einer Person, die die App für die Interaktion mit Media Services-Ressourcen verwendet. Die interaktive Anwendung soll den Benutzer zuerst zur Eingabe seiner Anmeldeinformationen auffordern. Ein Beispiel hierfür ist eine Verwaltungskonsolen-App, die von autorisierten Benutzern zum Überwachen von Codierungsaufträgen oder Livestreaming verwendet wird. 

## <a name="access-the-media-services-api"></a>Zugreifen auf die Media Services-API

Auf dieser Seite können Sie die Authentifizierungsmethode auswählen, die Sie für die Verbindung mit der API verwenden möchten. Die Seite bietet auch die Werte, die Sie zum Herstellen einer Verbindung mit der API benötigen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihr Media Services-Konto aus.
2. Wählen Sie, wie Sie eine Verbindung mit der Media Services-API herstellen möchten.
3. Wählen Sie unter **Verbindung mit Media Services-API herstellen** die Version der Media Services-API aus, mit der Sie eine Verbindung herstellen möchten.

## <a name="service-principal-authentication--recommended"></a>Dienstprinzipalauthentifizierung (empfohlen)

Authentifiziert einen Dienst mit einer App und einem Geheimnis von Azure Active Directory (Azure AD). Dies wird für alle Dienste der mittleren Ebene empfohlen, die die Media Services-API aufrufen. Beispiele sind Web-Apps, Funktionen, Logik-Apps, APIs und Microservices. Dies ist die empfohlene Authentifizierungsmethode.

### <a name="manage-your-azure-ad-app-and-secret"></a>Verwalten von Azure AD-App und -Geheimnis

Im Abschnitt **AAD-App und Geheimnis verwalten** können Sie eine neue Azure AD-App auswählen oder erstellen und ein Geheimnis generieren. Aus Sicherheitsgründen kann das Geheimnis nicht mehr angezeigt werden, nachdem das Blatt geschlossen wurde. Die Anwendung verwendet die Anwendungs-ID und das Geheimnis für die Authentifizierung, um ein gültiges Token für Media Services zu erhalten.

Stellen Sie sicher, dass Sie über ausreichende Berechtigungen verfügen, um eine Anwendung bei Ihrem Azure AD-Mandanten zu registrieren und die Anwendung einer Rolle in Ihrem Azure-Abonnement zuzuweisen. Weitere Informationen finden Sie unter [Erforderliche Berechtigungen](../../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app).

### <a name="connect-to-media-services-api"></a>Herstellen einer Verbindung mit der Media Services-API

Über **Verbindung mit Media Services-API herstellen** erhalten Sie Werte, mit denen Sie Ihre Dienstprinzipalanwendung verbinden können. Sie können Textwerte abrufen oder die JSON- oder XML-Blöcke kopieren.

## <a name="user-authentication"></a>Benutzerauthentifizierung

Mithilfe dieser Option kann ein Mitarbeiter oder ein Mitglied einer Azure Active Directory-Instanz authentifiziert werden, der für die Interaktion mit Media Services-Ressourcen eine App verwendet. Die interaktive Anwendung sollte den Benutzer zuerst zur Eingabe seiner Anmeldeinformationen auffordern. Diese Authentifizierungsmethode sollte nur für Managementanwendungen verwendet werden.

### <a name="connect-to-media-services-api"></a>Herstellen einer Verbindung mit der Media Services-API

Kopieren Sie Ihre Anmeldeinformationen, um Ihre Benutzeranwendung über den Abschnitt **Verbindung mit Media Services-API herstellen** zu verbinden. Sie können Textwerte abrufen oder die JSON- oder XML-Blöcke kopieren.

## <a name="next-steps"></a>Nächste Schritte

Erste Schritte mit dem [Hochladen von Dateien in Ihr Konto](media-services-portal-upload-files.md).
