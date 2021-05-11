---
title: Einrichtung von Dynamics 365 for Customer Engagement & PowerApps bietet technische Konfiguration auf Microsoft AppSource (Azure Marketplace)
description: Einrichtung von Dynamics 365 for Customer Engagement & PowerApps bietet technische Konfiguration auf Microsoft AppSource (Azure Marketplace).
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.custom: references_regions
author: vamahtan
ms.author: vamahtan
ms.date: 04/20/2021
ms.openlocfilehash: 99e7edfb66639f3b2fcc1596ede4e36fd14100bb
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108144977"
---
# <a name="set-up-dynamics-365-for-customer-engagement--power-apps-offer-technical-configuration"></a>Einrichtung von Dynamics 365 for Customer Engagement & PowerApps bietet technische Konfiguration

Auf dieser Seite werden die technischen Informationen definiert, die für das Herstellen einer Verbindung mit Ihrem Angebot verwendet werden. Durch diese Verbindung können wir Ihr Angebot für den Endkunden bereitstellen, wenn er es kaufen möchte.

## <a name="offer-information"></a>Angebotsinformationen

Das **Basislizenzmodell** bestimmt, wie Kunden im CRM Admin Center Ihrer Anwendung zugewiesen werden. Wählen Sie **Ressource** für instanzbasierte Lizenzierung bzw. **Benutzer** aus, wenn Lizenzen pro Mandant zugewiesen werden.

Das Kontrollkästchen **Erfordert eine ausgehende S2S-Verbindung und CRM-Secure Store-Zugriff** ermöglicht die Konfiguration von ausgehendem CRM Secure Store- oder Server-zu-Server-Zugriff (S2S). Dieses Feature muss während der Zertifizierungsphase besonders vom Dynamics 365-Team berücksichtigt werden. Microsoft kontaktiert Sie, um zusätzliche Schritte ausführen, die dieses Feature unterstützen.

Lassen Sie **Anwendungskonfigurations-URL leer**. Dies ist für die zukünftige Verwendung vorgesehen.

## <a name="crm-package"></a>CRM-Paket

Geben Sie für **URL Ihres Paketspeicherorts** die URL eines Azure Blob Storage-Kontos ein, das die hochgeladene ZIP-Datei des CRM-Pakets enthält. Schließen Sie einen schreibgeschützten SAS-Schlüssel in die URL ein, damit Microsoft Ihr Paket zur Überprüfung öffnen kann.

> [!IMPORTANT]
> Um eine Veröffentlichungssperre zu vermeiden, stellen Sie sicher, dass das Ablaufdatum in der URL Ihres Blobspeichers nicht abgelaufen ist. Sie können das Datum überarbeiten, indem Sie auf Ihre Richtlinie zugreifen. Es wird empfohlen, den **Ablaufzeitpunkt** mindestens einen Monat in der Zukunft anzusetzen.

Aktivieren Sie ggf. das Feld **In der Paketdatei sind mehrere CRM-Pakete enthalten**. Wenn dies der Fall ist, stellen Sie sicher, dass Sie alle Pakete in die ZIP-Datei einschließen.

Ausführliche Informationen zum Erstellen des Pakets und zum Aktualisieren seiner Struktur finden Sie in [Schritt 3: Erstellen eines AppSource-Pakets für Ihre App](/powerapps/developer/common-data-service/create-package-app-appsource).

## <a name="crm-package-availability"></a>Verfügbarkeit von CRM-Paketen

Wählen Sie **+ Region hinzufügen** aus, um die geografischen Regionen anzugeben, in denen Ihr CRM-Paket für Kunden verfügbar sein wird. Die Bereitstellung in den folgenden unabhängigen Regionen erfordert eine spezielle Berechtigung und Überprüfung während des Zertifizierungsprozesses: [Deutschland](../germany/index.yml), [Cloud for US Government](../azure-government/documentation-government-welcome.md) und TIP.

Standardmäßig wird die zuvor eingegebene **Anwendungskonfigurations-URL** für jede Region verwendet. Wenn Sie möchten, können Sie eine separate Anwendungskonfigurations-URL für eine oder mehrere bestimmte Regionen eingeben.

Wählen Sie **Entwurf speichern**, und fahren Sie dann mit der nächsten Registerkarte im linken Navigationsmenü fort, **Co-Sell mit Microsoft**. Informationen zur Einrichtung von Co-Sell mit Microsoft (optional) finden Sie unter [Co-Sell Partner Engagement](./co-sell-overview.md). Wenn Sie kein Co-Sell einrichten oder fertig sind, fahren Sie mit **Weitere Schritte** unten fort.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren zusätzlicher Inhalte](dynamics-365-customer-engage-supplemental-content.md)