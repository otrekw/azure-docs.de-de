---
title: Verknüpfen Ihrer Partner-ID, um vom Partner erworbenes Guthaben (Partner Earned Credit, PEC) für delegierte Ressourcen zu aktivieren
description: Hier erfahren Sie, wie Sie Ihre Partner-ID zuordnen, um vom Partner erworbenes Guthaben (Partner Earned Credit, PEC) für Kundenressourcen zu erhalten, die Sie über Azure Lighthouse verwalten.
ms.date: 09/04/2020
ms.topic: how-to
ms.openlocfilehash: 0a9e7f51e90b38bad24eada5a665ca60bf43452f
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89493277"
---
# <a name="link-your-partner-id-to-enable-partner-earned-credit-on-delegated-resources"></a>Verknüpfen Ihrer Partner-ID, um vom Partner erworbenes Guthaben (Partner Earned Credit, PEC) für delegierte Ressourcen zu aktivieren

Als Mitglied des [Microsoft Partner Network](https://partner.microsoft.com/) können Sie Ihre Partner-ID mit den Anmeldeinformationen verknüpfen, die für die Verwaltung delegierter Kundenressourcen verwendet werden. Dadurch können Sie Ihren Beitrag zu Kundeninteraktionen nachverfolgen und [vom Partner erworbenes Guthaben (Partner Earned Credit, PEC) für verwaltete Dienste](/partner-center/partner-earned-credit) erhalten.

Wenn Sie das [Onboarding von Kunden mit Angeboten für verwaltete Dienste im Azure Marketplace durchführen](publish-managed-services-offers.md), erfolgt diese Verknüpfung automatisch unter Verwendung der MPN-ID des Partner Center-Kontos, mit dem die Angebote veröffentlicht wurden. In diesem Fall sind keine weiteren Schritte erforderlich, um vom Partner erworbenes Guthaben für diese Kunden zu erhalten.

Wenn Sie das [Onboarding von Kunden mit Azure Resource Manager-Vorlagen durchführen](onboard-customer.md), müssen Sie diese Verknüpfung selbst erstellen. Dazu ist eine [Verknüpfung Ihrer MPN-ID](../../cost-management-billing/manage/link-partner-id.md) mit mindestens einem Benutzerkonto in Ihrem Verwaltungsmandanten erforderlich, das Zugriff auf alle Abonnements hat, für die Sie das Onboarding durchgeführt haben.

## <a name="associate-your-partner-id-when-you-onboard-new-customers"></a>Zuordnen Ihrer Partner-ID beim Durchführen des Onboardings für neue Kunden

Wenn Sie das Onboarding von Kunden über Azure Resource Manager-Vorlagen durchführen, müssen Sie wie folgt vorgehen, um Ihre Partner-ID zu verknüpfen und vom Partner erworbenes Guthaben zu aktivieren. Für diese Schritte benötigen Sie Ihre [MPN-Partner-ID](/partner-center/partner-center-account-setup#locate-your-mpn-id).

Der Einfachheit halber empfiehlt es sich, ein Dienstprinzipalkonto in Ihrem Mandanten zu erstellen, es mit Ihrer MPN-ID zu verknüpfen und dem Konto dann Zugriff auf jeden Kunden zu gewähren, für den Sie mit einer [integrierten Azure-Rolle, die für PEC berechtigt ist](https://docs.microsoft.com/partner-center/azure-roles-perms-pec), ein Onboarding durchführen.

1. [Erstellen Sie ein Dienstprinzipalkonto](../../active-directory/develop/howto-authenticate-service-principal-powershell.md) in Ihrem Verwaltungsmandanten. In diesem Beispiel wird ein Dienstprinzipalkonto mit dem Namen „PEC Automation Account“ verwendet.
1. Verwenden Sie dieses Dienstprinzipalkonto um in Ihrem Verwaltungsmandanten eine [Verknüpfung mit Ihrer Partner-ID](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) zu erstellen. Dies ist nur einmal erforderlich.
1. Schließen Sie beim [Durchführen des Onboardings eines Kunden mit Azure Resource Manager-Vorlagen](onboard-customer.md) eine Autorisierung ein, die „PEC Automation Account“ als Benutzer mit einer [integrierten Azure-Rolle, die für PEC berechtigt ist](https://docs.microsoft.com/partner-center/azure-roles-perms-pec), enthält.

Durch diese Schritte wird jeder von Ihnen verwaltete Kundenmandant mit Ihrer Partner-ID verknüpft, sodass Sie PEC für diese Kunden erhalten können. „PEC Automation Account“ muss sich nicht authentifizieren oder Aktionen im Kundenmandanten ausführen.

## <a name="add-your-partner-id-to-previously-onboarded-customers"></a>Hinzufügen Ihrer Partner-ID zu Kunden, für die bereits ein Onboarding durchgeführt wurde

Wenn Sie bereits ein Onboarding für einen Kunden durchgeführt haben, möchten Sie unter Umständen keine weitere Bereitstellung vornehmen, um den Dienstprinzipal „PEC Automation Account“ hinzuzufügen. Stattdessen können Sie die MPN-ID einem Benutzerkonto zuordnen, das bereits Zugriff auf die Arbeit im Mandanten dieses Kunden hat. Achten Sie darauf, dass dem Konto eine [integrierte Azure-Rolle, die für PEC berechtigt ist](https://docs.microsoft.com/partner-center/azure-roles-perms-pec), zugewiesen wurde.

Nachdem das Konto in Ihrem Verwaltungsmandanten [mit Ihrer Partner-ID verknüpft](../../cost-management-billing/manage/link-partner-id.md#link-to-a-partner-id) wurde, können Sie für diesen Kunden PEC erhalten.

## <a name="confirm-partner-earned-credit"></a>Bestätigen des vom Partner erworbenen Guthabens

Sie können [PEC-Details im Azure-Portal anzeigen](/partner-center/partner-earned-credit-explanation#azure-cost-management) und überprüfen, auf welche Kosten der PEC-Vorteil angewendet wurde.

Falls Sie die obigen Schritte ausgeführt haben und die Zuordnung nicht angezeigt wird, erstellen Sie eine Supportanfrage im Azure-Portal.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die Teilnahme am [Microsoft Partner Network](/partner-center/mpn-overview).
- Informieren Sie sich über die [Berechnung und Zahlung von PEC](/partner-center/partner-earned-credit-explanation).
- Führen Sie das [Onboarding von Kunden](onboard-customer.md) in Azure Lighthouse durch.
