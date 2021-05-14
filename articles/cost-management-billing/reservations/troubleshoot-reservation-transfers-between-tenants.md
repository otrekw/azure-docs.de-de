---
title: Ändern eines Azure-Reservierungsverzeichnisses
description: Dieser Artikel hilft Reservierungsbesitzern, einen Reservierungsauftrag von einem Azure Active Directory-Mandanten(-Verzeichnis) auf einen anderen (ein anderes) zu übertragen.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: reservations
ms.author: banders
ms.reviewer: yashar
ms.topic: troubleshooting
ms.date: 04/27/2021
ms.openlocfilehash: 183071454cbe6c9185ecb1868abfe1ac217e9519
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108143663"
---
# <a name="change-an-azure-reservation-directory-between-tenants"></a>Ändern eines Azure-Reservierungsverzeichnisses zwischen Mandanten

In diesem Artikel wird erläutert, wie Reservierungsbesitzer das Verzeichnis des Reservierungsauftrags von einem Azure Active Directory-Mandanten (Verzeichnis) in einen anderen ändern. Wenn Sie das Verzeichnis eines Reservierungsauftrags ändern, werden alle Azure RBAC-Zugriffe auf den Reservierungsauftrag und abhängige Reservierungen entfernt. Nur Sie haben nach der Änderung Zugriff. Durch das Ändern des Verzeichnisses wird der Abrechnungsbesitz für den Reservierungsauftrag nicht geändert. Das Verzeichnis wird für den übergeordneten Reservierungsauftrag und abhängige Reservierungen geändert.

Ein Reservierungsaustausch und eine Stornierung sind nicht erforderlich, um das Verzeichnis eines Reservierungsauftrags zu ändern.

Nachdem Sie das Verzeichnis einer Reservierung in einen anderen Mandanten geändert haben, können Sie der Reservierung weitere Besitzer hinzufügen. Weitere Informationen finden Sie unter [Wer kann eine Reservierung standardmäßig verwalten?](view-reservations.md#who-can-manage-a-reservation-by-default).

Wenn Sie das Verzeichnis eines Reservierungsauftrags ändern, werden alle Reservierungen des Auftrags mit übertragen.

## <a name="change-a-reservation-orders-directory"></a>Ändern des Verzeichnisses eines Reservierungsauftrags

Führen Sie die folgenden Schritte aus, um das Verzeichnis eines Reservierungsauftrags und seine abhängigen Reservierungen in einen anderen Mandanten zu ändern.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wenn Sie kein Abrechnungsadministrator, aber ein Reservierungsbesitzer sind, navigieren Sie zu **Reservierungen**, und fahren Sie dann mit Schritt 5 fort.
1. Navigieren Sie zu **Kostenverwaltung und Abrechnung**.
    - Wenn Sie EA-Administrator sind, wählen Sie im Menü auf der linken Seite die Option **Abrechnungsbereiche** und dann in der Liste der Abrechnungsbereiche einen Bereich aus.
    - Wenn Sie Besitzer eines Abrechnungsprofils für eine Microsoft-Kundenvereinbarung sind, wählen Sie im linken Menü **Abrechnungsprofile** aus. Wählen Sie in der Liste der Abrechnungsprofile ein Profil aus.
1. Die vollständige Liste der Reservierungen für Ihr EA-Registrierungsprofil oder -Abrechnungsprofil wird angezeigt.
1. Wählen Sie die zu übertragende Reservierung aus.
1. Wählen Sie in den Reservierungsdetails die Reservierungsauftrags-ID aus.
1. Wählen Sie im Reservierungsauftrag die Option **Verzeichnis ändern** aus.
1. Wählen Sie im Bereich „Verzeichnis ändern“ das Azure AD-Verzeichnis aus, auf das Sie die Reservierung übertragen möchten. Wählen Sie dann **Bestätigen** aus.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Reservierungen finden Sie unter [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md).