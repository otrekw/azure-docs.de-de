---
title: Problembehandlung bei allgemeinen Authentifizierungsfehlern | Azure Marketplace
description: Bietet Unterstützung bei allgemeinen Authentifizierungsfehlern, die bei Verwendung der Cloud-Partnerportal-APIs in Azure Marketplace auftreten können.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: troubleshooting
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 01af5357e4ae2f4dfb317a0931a8d0bc2b2d54e1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89487316"
---
# <a name="troubleshooting-common-authentication-errors"></a>Problembehandlung von häufigen Authentifizierungsfehlern

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Dieser Artikel erläutert, wie Sie häufig auftretende Authentifizierungsfehler beheben, wenn Sie Cloud-Partnerportal-APIs verwenden.

## <a name="unauthorized-error"></a>Fehler „Nicht autorisiert“

Wenn Sie regelmäßig `401 unauthorized`-Fehler erhalten, überprüfen Sie, ob Sie über ein gültiges Zugriffstoken verfügen.  Sofern noch nicht geschehen, erstellen Sie eine Azure Active Directory-Basisanwendung (Azure AD) und einen Dienstprinzipal wie unter [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../active-directory/develop/howto-create-service-principal-portal.md) beschrieben. Überprüfen Sie Ihren Zugriff dann mithilfe der Anwendung oder einer einfachen HTTP POST-Anforderung.  Geben Sie die Mandanten-, Anwendungs- und Objekt-ID und den geheimen Schlüssel an, um das Zugriffstoken abzurufen.

## <a name="forbidden-error"></a>Fehler „Verboten“

Wenn Sie einen `403 forbidden`-Fehler erhalten, überprüfen Sie, ob Ihrem Herausgeberkonto im Cloud-Partnerportal der richtige Dienstprinzipal hinzugefügt wurde. Befolgen Sie die Schritte auf der Seite [Voraussetzungen](./cloud-partner-portal-api-prerequisites.md), um Ihren Dienstprinzipal dem Portal hinzuzufügen.

Wenn der richtige Dienstprinzipal hinzugefügt wurde, überprüfen Sie alle anderen Informationen. Achten Sie besonders auf die im Portal eingegebene Objekt-ID. Auf der Seite zum Registrieren der Azure Active Directory-App werden zwei Objekt-IDs angezeigt – verwenden Sie die lokale Objekt-ID. Klicken Sie auf der Seite **App-Registrierungen** für Ihre App unter **Verwaltete Anwendung in lokalem Verzeichnis** auf den App-Namen, um den richtigen Wert abzurufen. Daraufhin gelangen Sie zu den lokalen Eigenschaften der App. Dort finden Sie auf der Seite **Eigenschaften** die richtige Objekt-ID, wie in der folgenden Abbildung dargestellt. Überprüfen Sie außerdem, ob Sie die richtige Herausgeber-ID verwenden, wenn Sie den Dienstprinzipal hinzufügen und den API-Aufruf ausführen.
