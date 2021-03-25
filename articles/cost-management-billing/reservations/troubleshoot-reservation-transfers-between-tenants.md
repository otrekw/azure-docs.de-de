---
title: Problembehandlung bei Azure-Reservierungsübertragungen zwischen Mandanten
description: Dieser Artikel hilft Reservierungsbesitzern, einen Reservierungsauftrag von einem Azure Active Directory-Mandanten (Verzeichnis) auf einen anderen zu übertragen.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 02/24/2021
ms.openlocfilehash: 79473d57cc7504e7e6ef4ef68ba0cee74203f62b
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102055801"
---
# <a name="troubleshoot-reservation-transfers-between-tenants"></a>Problembehandlung bei Reservierungsübertragungen zwischen Mandanten

Dieser Artikel hilft Reservierungsbesitzern, einen Reservierungsauftrag von einem Azure Active Directory-Mandanten(-Verzeichnis) auf einen anderen (ein anderes) zu übertragen. Wenn Sie das Verzeichnis eines Reservierungsauftrags ändern, werden alle Azure RBAC-Zugriffe auf den Reservierungsauftrag und abhängige Reservierungen entfernt. Nur Sie haben nach der Änderung Zugriff. Durch das Ändern des Verzeichnisses wird der Abrechnungsbesitz für den Reservierungsauftrag nicht geändert. Das Verzeichnis wird für den übergeordneten Reservierungsauftrag und abhängige Reservierungen geändert.

Ein Reservierungstausch und eine Stornierung ist für die Übertragung zwischen Mandanten nicht erforderlich.

Nachdem Sie eine Reservierung auf einen anderen Mandanten übertragen haben, können Sie der Reservierung weitere Besitzer hinzufügen. Weitere Informationen finden Sie unter [Wer kann eine Reservierung standardmäßig verwalten?](view-reservations.md#who-can-manage-a-reservation-by-default).

Wenn Sie einen Reservierungsauftrag übertragen, werden alle Reservierungen des Auftrags mit übertragen.

## <a name="transfer-a-reservation"></a>Übertragen einer Reservierung

Führen Sie die folgenden Schritte aus, um einen Reservierungsauftrag und seine abhängigen Reservierungen auf einen anderen Mandanten zu übertragen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Sie kein Abrechnungsadministrator, aber ein Reservierungsbesitzer sind, navigieren Sie zu **Reservierungen**, und fahren Sie dann mit Schritt 6 fort.
1. Navigieren Sie zu **Kostenverwaltung und Abrechnung**.
    - Wenn Sie EA-Administrator sind, wählen Sie im Menü auf der linken Seite die Option **Abrechnungsbereiche** und dann in der Liste der Abrechnungsbereiche einen Bereich aus.
    - Wenn Sie Besitzer eines Abrechnungsprofils für eine Microsoft-Kundenvereinbarung sind, wählen Sie im linken Menü **Abrechnungsprofile** aus. Wählen Sie in der Liste der Abrechnungsprofile ein Profil aus.
1. Wählen Sie im Menü auf der linken Seite die Option **Reservierungstransaktionen** aus. Die Liste der Reservierungstransaktionen wird angezeigt.
1. Am oberen Seitenrand wird ein Banner mit folgendem Hinweis angezeigt: *Abrechnungsadministratoren können jetzt Reservierungen verwalten. Klicken Sie hier, um Reservierungen anzuzeigen und zu verwalten.* Wählen Sie das Banner aus.
1. Die vollständige Liste der Reservierungen für Ihr EA-Registrierungsprofil oder -Abrechnungsprofil wird angezeigt.
1. Wählen Sie die zu übertragende Reservierung aus.
1. Wählen Sie in den Reservierungsdetails die Reservierungsauftrags-ID aus.
1. Wählen Sie im Reservierungsauftrag die Option **Verzeichnis ändern** aus.
1. Wählen Sie im Bereich „Verzeichnis ändern“ das Azure AD-Verzeichnis aus, auf das Sie die Reservierung übertragen möchten. Wählen Sie dann **Bestätigen** aus.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Reservierungen finden Sie unter [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md).