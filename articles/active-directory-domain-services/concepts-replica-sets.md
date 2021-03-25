---
title: Konzepte zu Replikatgruppen für Azure AD Domain Services | Microsoft-Dokumentation
description: Erfahren Sie, was Replikatgruppen in Azure Active Directory Domain Services sind und wie sie Redundanz für Anwendungen bereitstellen, die Identitätsdienste erfordern.
services: active-directory-ds
author: justinha
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2021
ms.author: justinha
ms.openlocfilehash: 8eb1560887c08c3f64fa599c39e5577242d2a1e8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101689061"
---
# <a name="replica-sets-concepts-and-features-for-azure-active-directory-domain-services"></a>Konzepte und Features zu Replikatgruppen für Azure Active Directory Domain Services

Beim Erstellen einer verwalteten Azure AD DS-Domäne (Azure Active Directory Domain Services) definieren Sie einen eindeutigen Namespace. Dieser Namespace ist der Domänenname, etwa *aaddscontoso.com*. Anschließend werden zwei Domänencontroller (DCs) in der ausgewählten Azure-Region bereitgestellt. Diese Bereitstellung von Domänencontrollern wird als Replikatgruppe bezeichnet.

Sie können eine verwaltete Domäne erweitern, sodass mehrere Replikatgruppen pro Azure AD-Mandant festgelegt sind. Replikatgruppen können einem beliebigen virtuellen Netzwerk mit Peering in einer beliebigen Azure-Region mit Azure AD DS-Unterstützung hinzugefügt werden. Zusätzliche Replikatgruppen in verschiedenen Azure-Regionen bieten eine geografische Notfallwiederherstellung für Legacyanwendungen, wenn eine Azure-Region offline geschaltet wird.

> [!NOTE]
> Sie können mit Replikatgruppen nicht mehrere eindeutige verwaltete Domänen in einem einzelnen Azure-Mandanten bereitstellen. Jede Replikatgruppe enthält dieselben Daten.

## <a name="how-replica-sets-work"></a>Funktionsweise von Replikatgruppen

Wenn Sie eine verwaltete Domäne erstellen (beispielsweise *aaddscontoso.com*), wird eine anfängliche Replikatgruppe erstellt. Von weiteren Replikatgruppen werden der gleiche Namespace und die gleiche Konfiguration verwendet. Änderungen an Azure AD DS (einschließlich Konfiguration, Benutzeridentität und Anmeldeinformationen, Gruppen, Gruppenrichtlinienobjekte, Computerobjekte und andere Änderungen) werden mittels AD DS-Replikation auf alle Replikatgruppen in der verwalteten Domäne angewendet.

Sie erstellen jede Replikatgruppe in einem virtuellen Netzwerk. Jedes virtuelle Netzwerk muss per Peering mit jedem anderen virtuellen Netzwerk verbunden werden, das die Replikatgruppe einer verwalteten Domäne hostet. Bei dieser Konfiguration wird eine Peermesh-Netzwerktopologie erstellt, die die Verzeichnisreplikation unterstützt. Ein virtuelles Netzwerk kann mehrere Replikatgruppen unterstützen, sofern sich jede Replikatgruppe in einem anderen virtuellen Subnetz befindet.

Alle Replikatgruppen werden am selben Active Directory-Standort angeordnet. Durch die schnelle Weitergabe aller Änderungen mithilfe der standortinternen Replikation wird schnell Konvergenz erreicht.

> [!NOTE]
> Es ist nicht möglich, separate Standorte zu definieren und Replikationseinstellungen zwischen Replikatgruppen zu definieren.

Das folgende Diagramm zeigt eine verwaltete Domäne mit zwei Replikatgruppen. Die erste Replikatgruppe wird mit dem Domänennamespace erstellt. Anschließend wird eine zweite Replikatgruppe erstellt:

![Diagramm der verwalteten Beispieldomäne mit zwei Replikatgruppen](./media/concepts-replica-sets/two-replica-set-example.png)

> [!NOTE]
> Replikatgruppen stellen sicher, dass in den Regionen, in denen eine Replikatgruppe konfiguriert ist, auch Authentifizierungsdienste verfügbar sind. Damit eine Anwendung bei einem regionalen Ausfall geografische Redundanz behält, muss sich die Anwendungsplattform, die von der verwalteten Domäne abhängt, auch in der anderen Region befinden.
>
> Die Resilienz anderer Dienste, die für die Funktion der Anwendung erforderlich sind, z. B. virtuelle Azure-Computer oder Azure App Service, wird nicht durch Replikatgruppen bereitgestellt. Sie sollten beim Entwerfen der Verfügbarkeit anderer Anwendungskomponenten die Resilienzfeatures für alle Dienste der Anwendung berücksichtigen.

Das folgende Beispiel zeigt eine verwaltete Domäne, für die mit drei Replikatgruppen zusätzliche Resilienz gewährleistet und die Verfügbarkeit der Authentifizierungsdienste sichergestellt wird. In beiden Beispielen befinden sich Anwendungsworkloads in derselben Region wie die Replikatgruppe der verwalteten Domäne:

![Diagramm der verwalteten Beispieldomäne mit drei Replikatgruppen](./media/concepts-replica-sets/three-replica-set-example.png)

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

Die Standard-SKU für eine verwaltete Domäne ist die *Enterprise*-SKU, die mehrere Replikatgruppen unterstützt. Um bei einem Wechsel von der *Standard*-SKU zusätzliche Replikatgruppen zu erstellen, [führen Sie ein Upgrade der verwalteten Domänen](change-sku.md) auf *Enterprise* oder *Premium* durch.

Die unterstützte maximale Anzahl von Replikatgruppen beträgt vier. Dies schließt auch das erste Replikat ein, das beim Erstellen der verwalteten Domäne erstellt wird.

Die Abrechnung für die einzelnen Replikatgruppe basiert auf der für die Domäne konfigurierten SKU. Wenn Sie z. B. eine verwaltete Domäne mit der *Enterprise*-SKU verwenden und über drei Replikatgruppen verfügen, erfolgt die Abrechnung in Ihrem Abonnement pro Stunde für jede der drei Replikatgruppen.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="can-i-create-a-replica-set-in-subscription-different-from-my-managed-domain"></a>Kann ich eine Replikatgruppe in einem anderen Abonnement als meine verwaltete Domäne erstellen?

Nein. Replikatgruppen müssen sich im selben Abonnement befinden wie die verwaltete Domäne.

### <a name="how-many-replica-sets-can-i-create"></a>Wie viele Replikatgruppen kann ich erstellen?

Sie können maximal vier Replikatgruppen erstellen (die anfängliche Replikatgruppe für die verwaltete Domäne und drei zusätzliche Replikatgruppen).

### <a name="how-does-user-and-group-information-get-synchronized-to-my-replica-sets"></a>Wie werden Benutzer- und Gruppeninformationen mit meinen Replikatgruppen synchronisiert?

Alle Replikatgruppen sind über ein Peermesh per Peering virtueller Netzwerke miteinander verbunden. Eine Replikatgruppe empfängt Benutzer- und Gruppenaktualisierungen von Azure AD. Diese Änderungen werden dann mithilfe der standortinternen AD DS-Replikation über das mittels Peering verknüpfte Netzwerk in die anderen Replikatgruppen repliziert.

Ebenso wie bei den lokalen AD DS kann ein erweiterter, nicht verbundener Zustand Unterbrechungen bei der Replikation verursachen. Da mittels Peering verbundene virtuelle Netzwerke nicht transitiv sind, muss bereits beim Entwurf für Replikatgruppen eine vollständige Peermesh-Netzwerktopologie eingeplant werden.

### <a name="how-do-i-make-changes-in-my-managed-domain-after-i-have-replica-sets"></a>Wie nehme ich Änderungen an der verwalteten Domäne vor, nachdem ich Replikatgruppen hinzugefügt habe?

Änderungen innerhalb der verwalteten Domäne funktionieren wie zuvor. Sie [erstellen und verwenden eine Verwaltungs-VM mit den RSAT-Tools, die in die verwaltete Domäne eingebunden ist](tutorial-create-management-vm.md). Sie können der verwalteten Domäne beliebig viele Verwaltungs-VMs hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

Für einen schnellen Einstieg in Replikatgruppen [erstellen und konfigurieren Sie eine verwaltete Azure AD DS-Domäne][tutorial-create-advanced]. Nach der Bereitstellung können Sie [zusätzliche Replikatgruppen erstellen und verwenden][create-replica-set].

<!-- LINKS - INTERNAL -->
[tutorial-create-advanced]: tutorial-create-instance-advanced.md
[create-replica-set]: tutorial-create-replica-set.md
