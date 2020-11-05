---
title: Erstellen eines VM-Angebots im Azure Marketplace
description: Erfahren Sie, wie Sie ein VM-Angebot im kommerziellen Microsoft-Marketplace erstellen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 10/20/2020
ms.openlocfilehash: bfd67688ce30b62002e26cea9f7be4df1cb6e622
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93126446"
---
# <a name="how-to-create-a-virtual-machine-offer-on-azure-marketplace"></a>Erstellen eines VM-Angebots im Azure Marketplace

In diesem Artikel wird beschrieben, wie Sie ein Azure-VM-Angebot für den [Azure Marketplace](https://azuremarketplace.microsoft.com/) erstellen. Er bezieht sich auf virtuelle Computer auf Windows- und Linux-Basis, die ein Betriebssystem, eine virtuelle Festplatte (VHD) und bis zu 16 Datenträger für Daten enthalten.

Bevor Sie beginnen, [erstellen Sie im Partner Center ein Konto für den kommerziellen Marketplace](partner-center-portal/create-account.md). Stellen Sie sicher, dass Ihr Konto im Programm für den kommerziellen Marketplace registriert ist.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Falls noch nicht geschehen, informieren Sie sich unter [Planen eines VM-Angebots](marketplace-virtual-machines.md). Dort werden die technischen Anforderungen für Ihren virtuellen Computer erläutert. Außerdem finden Sie Informationen und Ressourcen, die Sie beim Erstellen Ihres Angebots benötigen. 

## <a name="create-a-new-offer"></a>Erstellen eines neuen Angebots

1. Melden Sie sich bei [Partner Center](https://partner.microsoft.com/dashboard/home) an.
2. Wählen Sie im linken Bereich **Kommerzieller Marketplace** > **Übersicht** aus.
3. Wählen Sie auf der Seite **Übersicht** die Option **Neues Angebot** > **Virtueller Azure-Computer** aus.

    ![Screenshot: Menüoptionen im linken Bereich und Schaltfläche „Neues Angebot“.](./media/create-vm/new-offer-azure-virtual-machine.png)

> [!NOTE]
> Nachdem Ihr Angebot veröffentlicht wurde, werden Änderungen, die Sie in Partner Center vornehmen, im Azure Marketplace erst angezeigt, nachdem Sie das Angebot erneut veröffentlicht haben. Achten Sie darauf, die Veröffentlichung immer erneut durchzuführen, nachdem Sie Änderungen vorgenommen haben.

Geben Sie eine **Angebots-ID** ein. Dies ist ein eindeutiger Bezeichner für jedes Angebot in Ihrem Konto.

- Diese ID wird Kunden unter der Webadresse für das Azure Marketplace-Angebot und ggf. in Azure PowerShell und der Azure-Befehlszeilenschnittstelle angezeigt.
- Verwenden Sie nur Kleinbuchstaben und Zahlen. Die ID kann Bindestriche und Unterstriche enthalten, jedoch keine Leerzeichen, und ist auf 50 Zeichen beschränkt. Wenn Sie hier z. B. **test-offer-1** eingeben, lautet die Webadresse für das Angebot `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Nachdem Sie **Erstellen** ausgewählt haben, kann die Angebots-ID nicht mehr geändert werden.

Geben Sie einen **Angebotsalias** ein. Der Angebotsalias ist der Name, der für das Angebot in Partner Center verwendet wird.

- Dieser Name wird nicht im Azure Marketplace verwendet. Er unterscheidet sich vom Angebotsnamen und anderen Werten, die Kunden präsentiert werden.

## <a name="enable-a-test-drive-optional"></a>Aktivieren einer Testversion (optional)

Eine Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen für eine festgelegte Anzahl von Stunden Zugang zu einer vorkonfigurierten Umgebung gewähren. Wenn Sie eine Testversion anbieten, können Sie die Konvertierungsrate erhöhen und hochqualifizierte Leads generieren. Weitere Informationen zu Testversionen finden Sie unter [Was ist eine Testversion?](./what-is-test-drive.md)

> [!TIP]
> Eine Testversion unterscheidet sich von einer kostenlosen Testversion. Sie können entweder eine Testversion, eine kostenlose Testversion oder beides anbieten. Mit beiden können Kunden Ihre Lösung für einen festgelegten Zeitraum nutzen. Eine Testversion umfasst jedoch auch eine praktische, selbstgesteuerte Tour durch die wichtigsten Features und Vorteile Ihres Produkts, die in einem praxisnahen Implementierungsszenario veranschaulicht werden.

**So aktivieren Sie eine Testversion**
1.  Aktivieren Sie unter **Testversion** das Kontrollkästchen **Aktivieren einer Testversion**.
1.  Wählen Sie in der eingeblendeten Liste den Typ der Testversion aus.

## <a name="configure-lead-management"></a>Konfigurieren der Leadverwaltung

Wenn Sie Ihr Angebot mithilfe von Partner Center im kommerziellen Marketplace veröffentlichen, sollten Sie es mit Ihrem CRM-System (Customer Relationship Management) verbinden. Dadurch empfangen Sie Kontaktinformationen zum Kunden, sobald jemand sein Interesse an Ihrem Produkt bekundet oder es verwendet. Das Herstellen einer Verbindung mit einem CRM-System ist erforderlich, wenn Sie eine Testversion aktivieren möchten (siehe vorheriger Abschnitt). Andernfalls ist das Herstellen einer Verbindung mit einem CRM optional.

Wählen Sie unter **Kundenleads** den Link **Verbinden** aus.
1. Wählen Sie im Dialogfeld **Verbindungsdetails** eine Leadzielgruppe aus der Liste aus.
1. Füllen Sie alle angezeigten Felder aus. Ausführliche Schritte finden Sie in den folgenden Artikeln:

   - [Konfigurieren des Angebots zum Senden von Leads an die Azure-Tabelle](./partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table.md#configure-your-offer-to-send-leads-to-the-azure-table)
   - [Konfigurieren Ihres Angebots zum Senden von Leads an Dynamics 365 Customer Engagement](./partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics.md#configure-your-offer-to-send-leads-to-dynamics-365-customer-engagement) (früher Dynamics CRM Online)
   - [Konfigurieren Ihres Angebots zum Senden von Leads an den HTTPS-Endpunkt](./partner-center-portal/commercial-marketplace-lead-management-instructions-https.md#configure-your-offer-to-send-leads-to-the-https-endpoint)
   - [Konfigurieren Ihres Angebots zum Senden von Leads zu Marketo](./partner-center-portal/commercial-marketplace-lead-management-instructions-marketo.md#configure-your-offer-to-send-leads-to-marketo)
   - [Konfigurieren Ihres Angebots zum Senden von Leads zu Salesforce](./partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce.md#configure-your-offer-to-send-leads-to-salesforce)

1. Um die von Ihnen angegebene Konfiguration zu überprüfen, wählen Sie den Link **Überprüfen** aus.
1. Wählen Sie **OK** aus, um das Dialogfeld zu schließen.

## <a name="resell-through-csps"></a>Verkaufen über CSPs

Erweitern Sie die Reichweite Ihres Angebots, indem Sie es für Partner im [Cloud Solution Provider](https://azure.microsoft.com/offers/ms-azr-0145p/)-Programm (CSP) verfügbar machen. Alle BYOL-Pläne (Bring-Your-Own-License) werden im Programm automatisch aktiviert. Sie können auch Ihre Nicht-BYOL-Pläne aktivieren.

Wählen Sie **Erstellen** aus, um das Angebot zu generieren und fortzufahren.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von VM-Angebotseigenschaften](azure-vm-create-properties.md)
- [Bewährte Methoden für Angebotslistung](gtm-offer-listing-best-practices.md)