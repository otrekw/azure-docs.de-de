---
title: Ändern der SKU für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den SKU-Tarif für eine von Azure AD Domain Services verwaltete Domäne ändern, wenn sich Ihre geschäftlichen Anforderungen ändern.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/31/2020
ms.author: iainfou
ms.openlocfilehash: 18395f2b839aef88491f71aeed660eb2ce011e2c
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77614233"
---
# <a name="change-the-sku-for-an-existing-azure-ad-domain-services-managed-domain"></a>Ändern der SKU für eine vorhandene, von Azure AD Domain Services verwaltete Domäne

In Azure Active Directory Domain Services (Azure AD DS) beruhen die verfügbare Leistung und die Funktionen auf dem SKU-Typ. Zu diesen Funktionsunterschieden zählen die Sicherungshäufigkeit oder die maximale Anzahl von unidirektionalen ausgehenden Gesamtstrukturvertrauensstellungen (derzeit in der Vorschau verfügbar). Sie wählen eine SKU aus, wenn Sie die verwaltete Domäne erstellen, und Sie können die SKU wechseln, wenn sich Ihre Geschäftsanforderungen nach der Bereitstellung der verwalteten Domäne ändern. Änderungen bei den Geschäftsanforderungen können dazu führen, dass häufigere Sicherungen erforderlich sind oder zusätzliche Gesamtstrukturvertrauensstellungen erstellt werden müssen. Weitere Informationen zu den Grenzwerten und Preisen der unterschiedlichen SKUs finden Sie auf den Seiten[SKU-Konzepte von Azure AD DS][concepts-sku] und [Azure AD DS – Preise][pricing].

In diesem Artikel wird gezeigt, wie Sie im Azure-Portal die SKU für eine vorhandene verwaltete Azure AD DS-Domäne ändern.

## <a name="before-you-begin"></a>Voraussetzungen

Für diesen Artikel benötigen Sie die folgenden Ressourcen und Berechtigungen:

* Ein aktives Azure-Abonnement.
    * Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Einen mit Ihrem Abonnement verknüpften Azure Active Directory-Mandanten, der entweder mit einem lokalen Verzeichnis synchronisiert oder ein reines Cloudverzeichnis ist.
    * [Erstellen Sie einen Azure Active Directory-Mandanten][create-azure-ad-tenant], oder [verknüpfen Sie ein Azure-Abonnement mit Ihrem Konto][associate-azure-ad-tenant], sofern erforderlich.
* Eine verwaltete Azure Active Directory Domain Services-Domäne, die in Ihrem Azure AD-Mandanten aktiviert und konfiguriert ist.
    * Führen Sie bei Bedarf das Tutorial zum [Erstellen und Konfigurieren einer Azure Active Directory Domain Services-Instanz][create-azure-ad-ds-instance] aus.

## <a name="sku-change-limitations"></a>Einschränkungen bei SKU-Änderungen

Beim SKU-Änderungsvorgang gibt es einige Einschränkungen, wenn Sie eine Ressourcengesamtstruktur (derzeit in der Vorschau verfügbar) verwenden und eine unidirektionale ausgehende Gesamtstrukturvertrauensstellung von Azure AD DS zu einer lokalen AD DS-Umgebung erstellt haben. Für SKUs der *Premium*- und *Enterprise*-Edition ist ein Grenzwert für die Anzahl der Vertrauensstellungen definiert, die Sie erstellen können. Sie können nicht zu einer SKU mit einem Grenzwert wechseln, der unter dem derzeit konfigurierten Wert liegt.

Wenn Sie z. B. zwei Gesamtstrukturvertrauensstellungen für die *Premium*-SKU erstellt haben, können Sie nicht auf die *Standard*-SKU umsteigen. Die *Standard*-SKU unterstützt keine Gesamtstrukturvertrauensstellungen. Oder wenn Sie z. B. sieben Vertrauensstellungen für die *Premium*-SKU erstellt haben, können Sie nicht auf die *Enterprise*-SKU umsteigen. Die *Enterprise*-SKU unterstützt maximal fünf Vertrauensstellungen.

Weitere Informationen zu diesen Grenzwerten finden Sie unter [Azure AD DS-Funktionen und -Grenzwerte][concepts-sku].

## <a name="select-a-new-sku"></a>Auswählen einer neuen SKU

Führen Sie die folgenden Schritte aus, um die SKU für eine verwaltete Azure AD DS-Domäne im Azure-Portal zu ändern:

1. Suchen Sie oben im Azure-Portal nach dem Eintrag **Azure AD Domain Services**, und wählen Sie ihn aus. Wählen Sie in der Liste Ihre verwaltete Domäne (z. B. *aaddscontoso.com*) aus.
1. Wählen Sie auf der Seite „Azure AD DS“ im Menü auf der linken Seite die Optionen **Einstellungen > SKU** aus.

    ![Auswählen der SKU-Menüoption für Ihre verwaltete Azure AD DS-Domäne im Azure-Portal](media/change-sku/overview-change-sku.png)

1. Wählen Sie im Dropdownmenü die gewünschte SKU für Ihre verwaltete Azure AD DS-Domäne aus. Wenn Sie über eine Ressourcengesamtstruktur verfügen, können Sie *Standard*-SKU nicht auswählen, da Gesamtstrukturvertrauensstellungen nur für *Enterprise*-SKUs oder höher verfügbar sind.

    Wählen Sie im Dropdownmenü die gewünschte SKU und dann **Speichern** aus.

    ![Auswählen der gewünschten SKU im Dropdownmenü im Azure-Portal](media/change-sku/change-sku-selection.png)

Es kann ein oder zwei Minuten dauern, bis der SKU-Typ geändert wird.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie über eine Ressourcengesamtstruktur verfügen und nach der SKU-Änderung weitere Vertrauensstellungen erstellen möchten, lesen Sie [Erstellen einer ausgehenden Gesamtstrukturvertrauensstellung zu einer lokalen Domäne in Azure AD DS (Vorschau)][create-trust].

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[concepts-sku]: administration-concepts.md#azure-ad-ds-skus
[create-trust]: tutorial-create-forest-trust.md

<!-- EXTERNAL LINKS -->
[pricing]: https://azure.microsoft.com/pricing/details/active-directory-ds/
