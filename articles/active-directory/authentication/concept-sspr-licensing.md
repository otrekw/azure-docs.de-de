---
title: Lizenzieren der Self-Service-Kennwortzurücksetzung – Azure Active Directory
description: Erfahren Sie mehr über die verschiedenen Lizenzierungsanforderungen für die Self-Service-Kennwortzurücksetzung in Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/03/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70c441cc2bae0c318e312ccfa64b5161937c2457
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111963805"
---
# <a name="licensing-requirements-for-azure-active-directory-self-service-password-reset"></a>Lizenzierungsanforderungen für die Self-Service-Kennwortzurücksetzung in Azure Active Directory

Um Helpdeskanrufe und Produktivitätsverluste zu minimieren, wenn sich ein Benutzer nicht bei seinem Gerät oder einer Anwendung anmelden kann, können Benutzerkonten in Azure Active Directory (Azure AD) für die Self-Service-Kennwortzurücksetzung (SSPR) aktiviert werden. Zu den SSPR-Features zählen das Ändern, Zurücksetzen und Entsperren von Kennwörtern sowie das Rückschreiben in ein lokales Verzeichnis. Grundlegende SSPR-Funktionen sind in Microsoft 365 Business Standard oder höher und allen Azure AD Premium-SKUs kostenlos verfügbar.

In diesem Artikel werden die verschiedenen Möglichkeiten für die Lizenzierung und Verwendung der Self-Service-Kennwortzurücksetzung erläutert. Ausführliche Informationen zur Preisgestaltung und Abrechnung finden Sie auf der Seite [Azure AD – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Vergleich von Editionen und Features

In der folgenden Tabelle werden die verschiedenen SSPR-Szenarien für das Ändern, Zurücksetzen oder lokale Rückschreiben von Kennwörtern erläutert sowie welche SKUs die jeweilige Funktion bereitstellen.

| Funktion | Azure AD Free | Microsoft 365 Business Standard | Microsoft 365 Business Premium | Azure AD Premium P1 oder P2 |
| --- |:---:|:---:|:---:|:---:|
| **Ändern des Benutzerkennworts nur in der Cloud**<br />Wenn ein Benutzer in Azure AD sein Kennwort kennt und es ändern möchte. | ● | ● | ● | ● |
| **Zurücksetzen des Benutzerkennworts nur in der Cloud**<br />Wenn ein Benutzer in Azure AD sein Kennwort vergessen hat und es zurücksetzen muss. | | ● | ● | ● |
| **Ändern oder Zurücksetzen eines Hybridbenutzerkennworts mit lokalem Rückschreiben**<br />Wenn ein Benutzer in Azure AD, der mit Azure AD Connect von einem lokalen Verzeichnis aus synchronisiert wird, sein Kennwort ändern oder zurücksetzen und das neue Kennwort auch in das lokale Verzeichnis zurückschreiben möchte. | | | ● | ● |

> [!WARNING]
> Die Lizenzierungspläne für eigenständige Versionen von Microsoft 365 Basic und Standard unterstützen SSPR mit lokalem Rückschreiben nicht. Die Funktion für das lokale Rückschreiben setzt Azure AD Premium P1, Premium P2 oder Microsoft 365 Business Premium voraus.

Weitere Informationen zur Lizenzierung, einschließlich der Kosten, finden Sie auf den folgenden Seiten:


* [Microsoft 365 Lizenzierungsleitfaden für Sicherheit und Compliance](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-tenantlevel-services-licensing-guidance/microsoft-365-security-compliance-licensing-guidance)
* [Azure Active Directory-Preise](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-Features und -Funktionen](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 Business](/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="next-steps"></a>Nächste Schritte

Nutzen Sie das folgende Tutorial, um sich mit SSPR vertraut zu machen:

> [!div class="nextstepaction"]
> [Tutorial: Aktivieren der Self-Service-Kennwortzurücksetzung (SSPR)](tutorial-enable-sspr.md)