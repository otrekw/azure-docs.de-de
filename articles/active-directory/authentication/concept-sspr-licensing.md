---
title: Lizenzieren der Self-Service-Kennwortzurücksetzung – Azure Active Directory
description: Erfahren Sie mehr über die verschiedenen Lizenzierungsanforderungen für die Self-Service-Kennwortzurücksetzung in Azure Active Directory.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45ca11af061e37cf4f804ce2d7ceed72a9448294
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393077"
---
# <a name="licensing-requirements-for-azure-active-directory-self-service-password-reset"></a>Lizenzierungsanforderungen für die Self-Service-Kennwortzurücksetzung in Azure Active Directory

Um Helpdeskanrufe und Produktivitätsverluste zu minimieren, wenn sich ein Benutzer nicht bei seinem Gerät oder einer Anwendung anmelden kann, können Benutzerkonten in Azure Active Directory (Azure AD) für die Self-Service-Kennwortzurücksetzung (SSPR) aktiviert werden. Zu den SSPR-Features zählen das Ändern, Zurücksetzen und Entsperren von Kennwörtern sowie das Rückschreiben in ein lokales Verzeichnis. Grundlegende SSPR-Features stehen für Office 365- und alle Azure AD-Benutzer kostenlos zur Verfügung.

In diesem Artikel werden die verschiedenen Möglichkeiten für die Lizenzierung und Verwendung der Self-Service-Kennwortzurücksetzung erläutert. Ausführliche Informationen zur Preisgestaltung und Abrechnung finden Sie auf der Seite [Azure AD – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="compare-editions-and-features"></a>Vergleich von Editionen und Features

SSPR wird pro Benutzer lizenziert. Aus Gründen der Compliance müssen Organisationen ihren Benutzern die entsprechende Lizenz zuweisen.

In der folgenden Tabelle werden die verschiedenen SSPR-Szenarien für das Ändern, Zurücksetzen oder lokale Rückschreiben von Kennwörtern erläutert sowie welche SKUs die jeweilige Funktion bereitstellen.

| Funktion | Azure AD Free | Office 365 Business Premium | Microsoft 365 Business | Azure AD Premium P1 oder P2 |
| --- |:---:|:---:|:---:|:---:|
| **Ändern des Benutzerkennworts nur in der Cloud**<br />Wenn ein Benutzer in Azure AD sein Kennwort kennt und es ändern möchte. | ● | ● | ● | ● |
| **Zurücksetzen des Benutzerkennworts nur in der Cloud**<br />Wenn ein Benutzer in Azure AD sein Kennwort vergessen hat und es zurücksetzen muss. | | ● | ● | ● |
| **Ändern oder Zurücksetzen eines Hybridbenutzerkennworts mit lokalem Rückschreiben**<br />Wenn ein Benutzer in Azure AD, der mit Azure AD Connect von einem lokalen Verzeichnis aus synchronisiert wird, sein Kennwort ändern oder zurücksetzen und das neue Kennwort auch in das lokale Verzeichnis zurückschreiben möchte. | | | ● | ● |

> [!WARNING]
> Eigenständige Office 365-Lizenzierungspläne unterstützen kein SSPR mit lokalem Rückschreiben. Diese Office 365-Lizenzierungspläne erfordern Azure AD Premium P1, Premium P2 oder Microsoft 365 Business, damit diese Funktion verwendet werden kann.

Weitere Informationen zur Lizenzierung, einschließlich der Kosten, finden Sie auf den folgenden Seiten:

* [Azure Active Directory-Preise](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure Active Directory-Features und -Funktionen](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)
* [Microsoft 365 Business](https://docs.microsoft.com/office365/servicedescriptions/microsoft-365-service-descriptions/microsoft-365-business-service-description)

## <a name="enable-group-or-user-based-licensing"></a>Aktivieren der gruppen- oder benutzerbasierten Lizenzierung

Azure AD unterstützt die gruppenbasierte Lizenzierung. Administratoren können einer Gruppe von Benutzern Lizenzen in einem Massenvorgang zuweisen, anstatt sie einzeln zuzuweisen. Weitere Informationen hierzu finden Sie unter [Zuweisen, Prüfen und Beheben von Problemen mit Lizenzen](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Einige Microsoft-Dienste sind nicht an allen Standorten verfügbar. Bevor einem Benutzer eine Lizenz zugewiesen werden kann, muss der Administrator die Eigenschaft **Nutzungsspeicherort** für den Benutzer angeben. Die Zuweisung von Lizenzen erfolgt im Azure-Portal im Abschnitt **Benutzer** > **Profil** > **Einstellungen**. *Wenn Sie eine Gruppenlizenzzuweisung verwenden, erben alle Benutzer ohne Nutzungsspeicherort den Speicherort des Verzeichnisses.*

## <a name="next-steps"></a>Nächste Schritte

Nutzen Sie das folgende Tutorial, um sich mit SSPR vertraut zu machen:

> [!div class="nextstepaction"]
> [Tutorial: Aktivieren der Self-Service-Kennwortzurücksetzung (SSPR)](tutorial-enable-sspr.md)
