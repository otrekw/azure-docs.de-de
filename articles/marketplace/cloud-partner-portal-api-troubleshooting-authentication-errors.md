---
title: Problembehandlung von häufigen Authentifizierungsfehlern | Azure Marketplace
description: Erfahren Sie, wie Sie häufig auftretende Authentifizierungsfehler beheben, wenn Sie Cloud-Partnerportal-APIs verwenden.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 7684e097118fce4dcd471257ec0d9d476ce342be
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964922"
---
# <a name="troubleshooting-common-authentication-errors"></a>Problembehandlung von häufigen Authentifizierungsfehlern

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und werden auch nach der Migration Ihrer Angebote zu Partner Center weiterhin funktionieren. Die Integration führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach der Migration zu Partner Center weiterhin funktioniert.

Dieser Artikel erläutert, wie Sie häufig auftretende Authentifizierungsfehler beheben, wenn Sie Cloud-Partnerportal-APIs verwenden.

## <a name="unauthorized-error"></a>Fehler „Nicht autorisiert“

Wenn Sie regelmäßig `401 unauthorized`-Fehler erhalten, überprüfen Sie, ob Sie über ein gültiges Zugriffstoken verfügen.  Sofern noch nicht geschehen, erstellen Sie eine Azure Active Directory-Basisanwendung (Azure AD) und einen Dienstprinzipal wie unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md) beschrieben. Überprüfen Sie Ihren Zugriff dann mithilfe der Anwendung oder einer einfachen HTTP POST-Anforderung.  Geben Sie den geheimen Schlüssel sowie die Mandanten-, Anwendungs- und Objekt-ID an, um den Zugriffstoken wie in der folgenden Abbildung abzurufen:

![Beheben von Fehler 401](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Fehler „Verboten“

Wenn Sie einen `403 forbidden`-Fehler erhalten, überprüfen Sie, ob Ihrem Herausgeberkonto im Cloud-Partnerportal der richtige Dienstprinzipal hinzugefügt wurde.
Befolgen Sie die Schritte auf der Seite [Voraussetzungen](./cloud-partner-portal-api-prerequisites.md), um Ihren Dienstprinzipal dem Portal hinzuzufügen.

Wenn der richtige Dienstprinzipal hinzugefügt wurde, überprüfen Sie alle anderen Informationen. Achten Sie besonders auf die im Portal eingegebene Objekt-ID. Auf der Seite zum Registrieren der Azure Active Directory-App werden zwei Objekt-IDs angezeigt – verwenden Sie die lokale Objekt-ID. Klicken Sie auf der Seite **App-Registrierungen** für Ihre App unter **Verwaltete Anwendung in lokalem Verzeichnis** auf den App-Namen, um den richtigen Wert abzurufen. Daraufhin gelangen Sie zu den lokalen Eigenschaften der App. Dort finden Sie auf der Seite **Eigenschaften** die richtige Objekt-ID, wie in der folgenden Abbildung dargestellt. Überprüfen Sie außerdem, ob Sie die richtige Herausgeber-ID verwenden, wenn Sie den Dienstprinzipal hinzufügen und den API-Aufruf ausführen.

![Beheben von Fehler 403](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
