---
title: Problembehandlung von häufigen Authentifizierungsfehlern | Azure Marketplace
description: Erfahren Sie, wie Sie häufig auftretende Authentifizierungsfehler beheben, wenn Sie Cloud-Partnerportal-APIs verwenden.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: dsindona
ms.author: dsindona
ms.date: 07/14/2020
ms.openlocfilehash: bfcee6a6d9a8448315d59009ea8a6ef62fa71da0
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535993"
---
# <a name="troubleshooting-common-authentication-errors"></a>Problembehandlung von häufigen Authentifizierungsfehlern

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Dieser Artikel erläutert, wie Sie häufig auftretende Authentifizierungsfehler beheben, wenn Sie Cloud-Partnerportal-APIs verwenden.

## <a name="unauthorized-error"></a>Fehler „Nicht autorisiert“

Wenn Sie regelmäßig `401 unauthorized`-Fehler erhalten, überprüfen Sie, ob Sie über ein gültiges Zugriffstoken verfügen.  Sofern noch nicht geschehen, erstellen Sie eine Azure Active Directory-Basisanwendung (Azure AD) und einen Dienstprinzipal wie unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md) beschrieben. Überprüfen Sie Ihren Zugriff dann mithilfe der Anwendung oder einer einfachen HTTP POST-Anforderung.  Geben Sie den geheimen Schlüssel sowie die Mandanten-, Anwendungs- und Objekt-ID an, um den Zugriffstoken wie in der folgenden Abbildung abzurufen:

![Beheben von Fehler 401](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)

## <a name="forbidden-error"></a>Fehler „Verboten“

Wenn Sie einen `403 forbidden`-Fehler erhalten, überprüfen Sie, ob Ihrem Herausgeberkonto im Cloud-Partnerportal der richtige Dienstprinzipal hinzugefügt wurde.
Befolgen Sie die Schritte auf der Seite [Voraussetzungen](./cloud-partner-portal-api-prerequisites.md), um Ihren Dienstprinzipal dem Portal hinzuzufügen.

Wenn der richtige Dienstprinzipal hinzugefügt wurde, überprüfen Sie alle anderen Informationen. Achten Sie besonders auf die im Portal eingegebene Objekt-ID. Auf der Seite zum Registrieren der Azure Active Directory-App werden zwei Objekt-IDs angezeigt – verwenden Sie die lokale Objekt-ID. Klicken Sie auf der Seite **App-Registrierungen** für Ihre App unter **Verwaltete Anwendung in lokalem Verzeichnis** auf den App-Namen, um den richtigen Wert abzurufen. Daraufhin gelangen Sie zu den lokalen Eigenschaften der App. Dort finden Sie auf der Seite **Eigenschaften** die richtige Objekt-ID, wie in der folgenden Abbildung dargestellt. Überprüfen Sie außerdem, ob Sie die richtige Herausgeber-ID verwenden, wenn Sie den Dienstprinzipal hinzufügen und den API-Aufruf ausführen.

![Beheben von Fehler 403](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
