---
title: Löschen von Azure Active Directory Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine von Azure Active Directory Domain Services verwaltete Domäne im Azure-Portal deaktivieren oder löschen.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: iainfou
ms.openlocfilehash: ee4c41ca3179eead0e4fd470a02a5cdfa4a6d43a
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74705234"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Löschen einer von Azure Active Directory Domain Services verwalteten Domäne im Azure-Portal

Wenn Sie eine verwaltete Domäne nicht mehr benötigen, können Sie eine Azure Active Directory Domain Services-Instanz (Azure AD DS) löschen. Es gibt keine Möglichkeit, eine verwaltete Azure AD DS-Domäne abzuschalten oder vorübergehend zu deaktivieren. Durch das Löschen der verwalteten Azure AD DS-Domäne wird der Azure AD-Mandant weder gelöscht noch anderweitig beeinträchtigt. In diesem Artikel erfahren Sie, wie Sie im Azure-Portal eine verwaltete Azure AD DS-Domäne löschen.

> [!WARNING]
> **Die Löschung ist dauerhaft und kann nicht rückgängig gemacht werden.**
> Beim Löschen einer verwalteten Azure AD DS-Domäne werden die folgenden Schritte ausgeführt:
>   * Die Bereitstellung der Domänencontroller für die verwaltete Domäne wird aufgehoben, und die Domänencontroller werden aus dem virtuellen Netzwerk entfernt.
>   * Die Daten in der verwalteten Domäne werden endgültig gelöscht. Dazu gehören u.a. benutzerdefinierte Organisationseinheiten, GPOs, benutzerdefinierte DNS-Einträge, Dienstprinzipale und gruppenverwaltete Dienstkonten (GMSAs), die Sie erstellt haben.
>   * Computer, die in die verwaltete Domäne eingebunden sind, verlieren ihre Vertrauensstellung mit der Domäne und müssen aus der Domäne entfernt werden.
>       * Bei diesen Computern können Sie sich nicht mit AD-Unternehmensanmeldeinformationen anmelden. Stattdessen müssen Sie die Anmeldeinformationen des lokalen Administrators für den Computer verwenden.

## <a name="delete-the-managed-domain"></a>Löschen der verwalteten Domäne

Führen Sie die folgenden Schritte aus, um eine verwaltete Azure AD DS-Domäne zu löschen:

1. Suchen Sie im Azure-Portal nach dem Eintrag **Azure AD Domain Services**, und wählen Sie ihn aus.
1. Wählen Sie den Namen der verwalteten Azure AD DS-Domäne aus, z. B. *aadds.contoso.com*.
1. Wählen Sie auf der Seite **Übersicht** die Option **Löschen** aus. Geben Sie zum Bestätigen des Löschvorgangs erneut den Domänennamen der verwalteten Domäne ein, und wählen Sie dann **Löschen** aus.

Das Löschen der verwalteten Azure AD DS-Domäne kann 15 bis 20 Minuten (oder länger) dauern.

## <a name="next-steps"></a>Nächste Schritte

Ziehen Sie [Feedback][feedback] in Betracht, um uns mitzuteilen, welche Funktionen Sie sich in Azure AD DS wünschen.

Wenn Sie erneut mit Azure AD DS beginnen möchten, lesen Sie [Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz][create-instance].

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md
