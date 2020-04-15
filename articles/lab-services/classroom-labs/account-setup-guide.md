---
title: Leitfaden für beschleunigte Lab-Konto-Einrichtung für Azure Lab Services
description: Dieser Leitfaden unterstützt Administratoren beim schnellen Einrichten eines Lab-Kontos für die Verwendung innerhalb ihrer Bildungseinrichtung.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/18/2020
ms.author: spelluru
ms.openlocfilehash: 8fcc46487e7f7c2d075639f10a30cae9950ff31b
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879580"
---
# <a name="lab-account-setup-guide"></a>Einrichtungsleitfaden für Lab-Konten

Als ersten Schritt sollten Administratoren ein Lab-Konto innerhalb Ihres Azure-Abonnements einrichten. Ein Lab-Konto ist ein Container für Ihre Classroom-Labs. Seine Einrichtung nimmt nur einige Minuten in Anspruch.

## <a name="understand-your-schools-lab-account-requirements"></a>Informationen zu den Lab-Kontoanforderungen Ihrer Bildungseinrichtung

Wenn Sie wissen möchten, wie Sie Ihr Lab-Konto auf der Grundlage der Anforderungen Ihrer Bildungseinrichtung konfigurieren, sollten Sie die folgenden Fragen berücksichtigen.

### <a name="do-i-have-access-to-an-azure-subscription"></a>Besitze ich Zugriff auf ein Azure-Abonnement?

Um ein Lab-Konto zu erstellen, benötigen Sie Zugriff auf ein Azure-Abonnement, das für Ihre Bildungseinrichtung konfiguriert ist. Ihre Bildungseinrichtung kann über ein Abonnement oder mehrere Abonnements verfügen. Mit einem Abonnement verwalten Sie Abrechnung und Sicherheit für Ihre sämtlichen Azure-Ressourcen und -Dienste. Hierzu zählen auch Lab-Konten.

### <a name="how-many-lab-accounts-need-to-be-created"></a>Wie viele Lab-Konten müssen erstellt werden?

Erstellen Sie für einen schnellen Einstieg ein einzelnes Lab-Konto, und legen Sie später nach Bedarf weitere Lab-Konten an. Beispielsweise können Sie als Ziel ein Lab-Konto pro Abteilung anstreben.

### <a name="who-should-be-owners-and-contributors-of-the-lab-account"></a>Wer sollte Besitzer und Mitwirkender für das Lab-Konto sein?

Ihre Administratoren sind in der Regel Besitzer und Mitwirkende für ein Lab-Konto. Sie sind dafür verantwortlich, die Richtlinien zu verwalten, die für alle Labs im Lab-Konto gelten. Die Person, die das Lab-Konto erstellt, ist automatisch Besitzer des Kontos. Sie können weitere Besitzer und Mitwirkende hinzufügen, im Allgemeinen aus dem Azure Active Directory (Azure AD)-Mandanten, der mit Ihrem Abonnement verknüpft ist. Dies kann beim Verwalten eines Lab-Kontos hilfreich sein, indem Sie entweder die Rolle „Besitzer“ oder „Mitwirkender“ auf der Lab-Kontoebene zuweisen.

### <a name="who-will-be-allowed-to-create-and-manage-labs"></a>Wer ist berechtigt, Labs zu erstellen und zu verwalten?

Sie können festlegen, dass Ihre Administratoren und Lehrkräfte Labs erstellen und verwalten können. Diese Benutzer (in der Regel vom Azure AD Mandanten, der Ihrem Abonnement zugeordnet ist) werden der Rolle „Lab-Ersteller“ im Lab-Konto zugewiesen.

### <a name="do-you-want-to-give-lab-creators-the-ability-to-save-images-that-can-be-shared-across-labs"></a>Möchten Sie Lab-Entwicklern die Möglichkeit geben, Images zu speichern, die von Labs gemeinsam genutzt werden können?

Eine Shared Image Gallery ist ein Repository, das Sie zum Speichern und Freigeben von Images verwenden können. Wenn Sie über mehrere Kurse verfügen, die die gleichen Images benötigen, können Lab-Ersteller das Image einmal erstellen und es dann für verschiedene Labs freigeben. Für den Einstieg benötigen Sie jedoch nicht unbedingt eine Shared Image Gallery, da Sie diese später jederzeit hinzufügen können.

Wenn Sie diese Frage mit „Ja“ beantwortet haben, müssen Sie eine Shared Image Gallery erstellen oder eine vorhandene an Ihr Lab-Konto anfügen. Wenn Sie mit „Ich weiß nicht“ geantwortet haben, können Sie diese Entscheidung auf einen späteren Zeitpunkt verschieben.

### <a name="which-images-in-azure-marketplace-will-your-classroom-labs-use"></a>Welche Images in Azure Marketplace werden von den Classroom-Labs verwendet?

Azure Marketplace bietet Hunderte von Images, die Sie aktivieren können, damit Lab-Ersteller das Image zum Erstellen des Labs verwenden können. Einige Images enthalten möglicherweise alles, was für ein Lab benötigt wird. In anderen Fällen können Sie ein Image als Ausgangspunkt verwenden, das der Ersteller des Labs durch die Installation zusätzlicher Anwendungen oder Tools anpassen kann.

Wenn Sie noch nicht wissen, welche Images Sie verwenden werden, können Sie diese später jederzeit aktivieren. Sie können am besten feststellen, welche Images verfügbar sind, indem Sie zuerst ein Lab-Konto erstellen. Dadurch erhalten Sie Zugriff, sodass Sie die Liste der verfügbaren Images und deren Inhalte überprüfen können.
  
### <a name="do-the-labs-virtual-machines-need-to-have-access-to-other-azure-or-on-premises-resources"></a>Benötigen die virtuellen Computer des Labs Zugriff auf andere Azure- oder lokale Ressourcen?

Wenn Sie ein Lab-Konto einrichten, haben Sie auch die Möglichkeit, ein Peering mit einem virtuellen Netzwerk durchzuführen. Um zu entscheiden, ob dies erforderlich ist, sollten Sie die folgenden Fragen berücksichtigen:

- **Müssen Sie Zugriff auf einen Lizenzierungsserver bereitstellen?**
  
   Wenn Sie Azure Marketplace-Images verwenden möchten, werden die Kosten für die Betriebssystemlizenz in den Preisen für Lab-Dienste gebündelt. Auf diese Weise müssen Sie die Lizenzen für das Betriebssystem selbst nicht bereitstellen. Bei Installation zusätzlicher Software und Anwendungen müssen Sie jedoch ggf. eine entsprechende Lizenz bereitstellen.

- **Benötigen die Lab-VMs Zugriff auf andere lokale Ressourcen, z. B. auf eine Dateifreigabe oder eine Datenbank?**

   Sie erstellen ein virtuelles Netzwerk, um den Zugriff auf lokale Ressourcen zu ermöglichen, in der Regel über ein virtuelles Site-to-Site-Netzwerkgateway. Wenn Sie kein virtuelles Netzwerk konfiguriert haben, müssen Sie hierfür zusätzliche Zeit aufwenden.

- **Benötigen die Lab-VMs Zugriff auf andere Azure-Ressourcen, die sich in einem virtuellen Netzwerk befinden?**

   Wenn Sie Zugriff auf Azure-Ressourcen benötigen, die *nicht* innerhalb eines virtuellen Netzwerks gesichert sind, können Sie über das öffentliche Internet auf diese Ressourcen zugreifen, ohne ein Peering durchzuführen.

Wenn Sie auf mindestens eine Frage mit „Ja“ geantwortet haben, müssen Sie für das Lab-Konto ein Peering mit einem virtuellen Netzwerk durchführen. Wenn Sie mit „Ich weiß nicht“ geantwortet haben, können Sie diese Entscheidung auf einem späteren Zeitpunkt verschieben. Nach dem Erstellen des Lab-Kontos können Sie sich jederzeit für das Peering mit einem virtuellen Netzwerk entscheiden.

## <a name="set-up-your-lab-account"></a>Einrichten Ihres Lab-Kontos

Nachdem Sie nun die Anforderungen für das Lab-Konto verstanden haben, können Sie es einrichten.

1. **Erstellen Ihres Lab-Kontos** Im Tutorial zum [Erstellen eines Lab-Kontos](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#create-a-lab-account) finden Sie weitere Anleitungen.

   Beim Erstellen eines Lab-Kontos empfiehlt es sich möglicherweise, sich mit den beteiligten Azure-Ressourcen vertraut zu machen. Weitere Informationen finden Sie in den folgenden Artikeln:

   - [Abonnement](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#subscription)
   - [Ressourcengruppe](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#resource-group)
   - [Lab-Konto](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#lab-account)
   - [Classroom-Lab](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#classroom-lab)
   - [Auswählen einer Region und eines Standorts](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations)
   - [Benennungsrichtlinien für Ressourcen](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#naming)

2. **Hinzufügen von Benutzern zur Rolle „Lab-Ersteller“** . Anweisungen hierzu finden Sie unter [Hinzufügen von Benutzern zur Rolle „Ersteller des Labs“](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role).

   Weitere Informationen zu den verschiedenen Rollen, die Benutzern zugewiesen werden können, die Lab-Konten und Labs verwalten, finden Sie im [Leitfaden zum Verwalten von Identität](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#manage-identity).

3. **Herstellen einer Verbindung mit einem virtuellen Peernetzwerk** Eine Anleitung finden Sie unter [Verbinden des Netzwerks Ihres Labs mit einem virtuellen Peernetzwerk](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network).

   Möglicherweise benötigen Sie auch die Anweisungen unter [Konfigurieren des Adressbereichs der Lab-VMs](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-lab-accounts#specify-an-address-range-for-vms-in-the-lab).

4. **Aktivieren und Überprüfen von Images**. Eine Anleitung finden Sie unter [Aktivieren von Azure Marketplace-Images für Lab-Ersteller](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images).

   Um die Inhalte der einzelnen Azure Marketplace-Images zu überprüfen, wählen Sie den Imagenamen aus. Im folgenden Screenshot werden beispielsweise die Details für das Image „Ubuntu Data Science VM“ gezeigt:

   ![Screenshot zum Überprüfen von Azure Marketplace-Images](../media/setup-guide/review-marketplace-images.png)

   Wenn Sie eine Shared Image Gallery an Ihr Lab-Konto angefügt haben und benutzerdefinierte Images für Lab-Ersteller freigeben möchten, führen Sie ähnliche Schritte wie im folgenden Screenshot gezeigt aus:

   ![Screenshot zum Aktivieren benutzerdefinierter Images in einer Shared Image Gallery](../media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Lab-Konten](how-to-manage-lab-accounts.md)

- [Einrichtungsleitfaden für Classroom-Labs](setup-guide.md)
