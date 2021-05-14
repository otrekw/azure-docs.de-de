---
title: Erstellen eines VM-Angebots im Azure Marketplace
description: Erfahren Sie, wie Sie ein VM-Angebot im kommerziellen Microsoft-Marketplace erstellen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 04/08/2021
ms.openlocfilehash: f0c1d9d528ed4fbf61786042fb6fb34f05fec5d5
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107812591"
---
# <a name="how-to-create-a-virtual-machine-offer-on-azure-marketplace"></a>Erstellen eines VM-Angebots im Azure Marketplace

In diesem Artikel wird beschrieben, wie Sie ein Azure-VM-Angebot für den [Azure Marketplace](https://azuremarketplace.microsoft.com/) erstellen. Er bezieht sich auf virtuelle Computer auf Windows- und Linux-Basis, die ein Betriebssystem, eine virtuelle Festplatte (VHD) und bis zu 16 Datenträger für Daten enthalten.

Bevor Sie beginnen, [erstellen Sie im Partner Center ein Konto für den kommerziellen Marketplace](create-account.md). Stellen Sie sicher, dass Ihr Konto im Programm für den kommerziellen Marketplace registriert ist.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Falls noch nicht geschehen, informieren Sie sich unter [Planen eines VM-Angebots](marketplace-virtual-machines.md). Dort werden die technischen Anforderungen für Ihren virtuellen Computer erläutert. Außerdem finden Sie Informationen und Ressourcen, die Sie beim Erstellen Ihres Angebots benötigen.

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Bereich **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie auf der Seite **Übersicht** die Option **+ Neues Angebot** > **Virtueller Azure-Computer** aus.

    ![Screenshot: Menüoptionen im linken Bereich und Schaltfläche „Neues Angebot“.](./media/create-vm/new-offer-azure-virtual-machine.png)

> [!NOTE]
> Nachdem Ihr Angebot veröffentlicht wurde, werden Änderungen, die Sie in Partner Center vornehmen, im Azure Marketplace erst angezeigt, nachdem Sie das Angebot erneut veröffentlicht haben. Achten Sie darauf, die Veröffentlichung immer erneut durchzuführen, nachdem Sie Änderungen vorgenommen haben.

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird Kunden unter der Webadresse für das Azure Marketplace-Angebot und ggf. in Azure PowerShell und der Azure-Befehlszeilenschnittstelle angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Die ID kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Der Angebotsalias ist der Name, der für das Angebot in Partner Center verwendet wird.

- Dieser Name wird nicht im Azure Marketplace verwendet. Er unterscheidet sich vom Angebotsnamen und anderen Werten, die Kunden präsentiert werden.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren. In Partner Center wird die Seite **Angebotseinrichtung** geöffnet.

## <a name="enable-a-test-drive-optional"></a>Aktivieren einer Testversion (optional)

Eine Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen für eine festgelegte Anzahl von Stunden Zugang zu einer vorkonfigurierten Umgebung gewähren. Wenn Sie eine Testversion anbieten, können Sie die Konvertierungsrate erhöhen und hochqualifizierte Leads generieren. Weitere Informationen zu Testversionen finden Sie unter [Was ist eine Testversion?](./what-is-test-drive.md)

> [!TIP]
> Eine Testversion unterscheidet sich von einer kostenlosen Testversion. Sie können entweder eine Testversion, eine kostenlose Testversion oder beides anbieten. Mit beiden können Kunden Ihre Lösung für einen festgelegten Zeitraum nutzen. Eine Testversion umfasst jedoch auch eine praktische, selbstgesteuerte Tour durch die wichtigsten Features und Vorteile Ihres Produkts, die in einem praxisnahen Implementierungsszenario veranschaulicht werden.

Aktivieren Sie das Kontrollkästchen **Aktivieren einer Testversion**, um eine Testversion zu aktivieren. Sie konfigurieren die Testversion später. Eine Testversion erfordert das Konfigurieren eines CRM-Systems (siehe nächster Abschnitt).

## <a name="configure-customer-leads-management"></a>Konfigurieren der Verwaltung von Kundenleads

[!INCLUDE [Customer leads](includes/customer-leads.md)] 

Wählen Sie **Entwurf speichern** aus, bevor Sie mit der nächsten Registerkarte im linken Navigationsmenü (**Eigenschaften**) fortfahren.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von VM-Angebotseigenschaften](azure-vm-create-properties.md)
- [Bewährte Methoden für Angebotslistung](gtm-offer-listing-best-practices.md)