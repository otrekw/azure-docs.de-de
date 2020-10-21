---
title: include file
description: Includedatei mit Text für den kommerziellen Microsoft-Marketplace
documentationcenter: partner-center-commercial-marketplace
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
ms.date: 01/22/2020
ms.custom: include file
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: e1119cf5c40cdcb1fd598b85410ea9149a7cc146
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91400994"
---
Microsoft stellt eine Vorlage mit dem Standardvertrag für den kommerziellen Marketplace bereit.

- **Möchten Sie den Standardvertrag für den kommerziellen Microsoft-Marketplace verwenden?**

Zur Vereinfachung des Beschaffungsprozesses für Kunden und zur Reduzierung der rechtlichen Komplexität für Softwareanbieter stellt Microsoft eine Vorlage für Standardverträge für den kommerziellen Microsoft-Marketplace bereit, mit der Transaktionen im Marketplace erleichtert werden. Statt benutzerdefinierte Geschäftsbedingungen zu erstellen, können Herausgeber im kommerziellen Marketplace ihre Software wahlweise unter dem Standardvertrag anbieten, den Kunden nur einmal überprüfen und akzeptieren müssen. Der Standardvertrag ist hier zu finden: https://go.microsoft.com/fwlink/?linkid=2041178.

Sie können auswählen, ob Sie den Standardvertrag statt Ihrer eigenen benutzerdefinierten Geschäftsbedingungen verwenden möchten, indem Sie das Kontrollkästchen „Standardvertrag für den kommerziellen Marketplace verwenden“ aktivieren.

![Kontrollkästchen „Standardvertrag verwenden“](./media/use-standard-contract.png)

> [!NOTE]
> Nachdem Sie ein Angebot mit dem Standardvertrag für den kommerziellen Microsoft-Marketplace veröffentlicht haben, können Sie Ihre eigenen benutzerdefinierten Geschäftsbedingungen nicht mehr angeben. In diesem Szenario gilt „entweder oder“. Entweder bieten Sie Ihre Lösung im Rahmen des Standardvertrags **oder** Ihrer eigenen Geschäftsbedingungen an. Wenn Sie die Bedingungen des Standardvertrags ändern möchten, können Sie dies über Zusatzvereinbarungen für den Standardvertrag erreichen.

**Zusatzvereinbarungen für den Standardvertrag**

Mithilfe von Zusatzvereinbarungen für den Standardvertrag können Herausgeber der Einfachheit halber den Standardvertrag auswählen und die Bedingungen für ihr Produkt oder Unternehmen anpassen. Kunden, die bereits den Microsoft-Standardvertrag geprüft und akzeptiert haben, müssen lediglich die Zusatzvereinbarungen zum Vertrag prüfen.

Für kommerzielle Marketplace-Herausgeber stehen zwei Arten von Zusatzvereinbarungen zur Verfügung:

- Universelle Zusatzvereinbarungen: Diese Zusatzvereinbarungen werden universell auf den Standardvertrag für alle Kunden angewendet. Universelle Zusatzvereinbarungen werden jedem Kunden des Angebots im Rahmen der Kaufabwicklung angezeigt. Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarungen akzeptieren, bevor sie Ihr Angebot nutzen können.
- Benutzerdefinierte Zusatzvereinbarungen: Diese Zusatzvereinbarungen sind spezielle Zusatzvereinbarungen für den Standardvertrag, die über Azure-Mandanten-IDs nur bestimmte Kunden betreffen. Herausgeber können den gewünschten Mandanten auswählen. Nur den Kunden des Mandanten werden die Bedingungen der Zusatzvereinbarungen im Rahmen der Kaufabwicklung für das Angebot präsentiert.  Kunden müssen die Bedingungen des Standardvertrags und die Zusatzvereinbarung(en) akzeptieren, bevor sie Ihr Angebot nutzen können.

>[!NOTE]
> Diese beiden Arten von Zusatzvereinbarungen sind übereinander angeordnet. Kunden, für die benutzerdefinierte Zusatzvereinbarungen gelten, erhalten beim Kauf auch die universellen Zusatzvereinbarung für den Standardvertrag.

**Bestimmungen der universellen Zusatzvereinbarung zum Standardvertrag für den kommerziellen Microsoft-Marketplace:** Geben Sie in diesem Feld die universellen Zusatzbestimmungen ein. Sie können eine universelle Zusatzvereinbarung pro Angebot angeben. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Die Bestimmungen werden Kunden während des Ermittlungs- und Kaufprozesses in AppSource, dem Azure Marketplace und dem Azure-Portal angezeigt.

**Benutzerdefinierte Bestimmungen der Zusatzvereinbarung zum Standardvertrag für den kommerziellen Microsoft-Marketplace:** Wählen Sie zunächst **Benutzerdefinierte Zusatzbestimmungen hinzufügen** aus. Sie können bis zu 10 benutzerdefinierte Zusatzbestimmungen pro Angebot angeben.

- **Benutzerdefinierte Zusatzbestimmungen**: Geben Sie Ihre Zusatzbestimmungen in das Feld für benutzerdefinierte Bestimmungen der Zusatzvereinbarung ein. Sie können eine unbegrenzte Anzahl von Zeichen in dieses Feld eingeben. Diese benutzerdefinierten Zusatzbestimmungen werden im Azure-Portal im Rahmen der Kaufabwicklung für das Angebot nur für Kunden mit den Mandanten-IDs angezeigt, die Sie für diese benutzerdefinierten Bestimmungen angeben.  
- **Mandanten-IDs** (erforderlich): Jede benutzerdefinierte Zusatzvereinbarung kann für bis zu 20 Mandanten-IDs gelten. Wenn Sie eine benutzerdefinierte Zusatzvereinbarung hinzufügen, müssen Sie mindestens eine Mandanten-ID angeben. Die Mandanten-ID identifiziert Ihren Kunden in Azure. Sie können Ihren Kunden bitten, Ihnen diese ID mitzuteilen. Kunden finden ihre ID unter „portal.azure.com“ > „Azure Active Directory“ > „Eigenschaften“. Der Wert „Verzeichnis-ID“ ist die Mandanten-ID (z. B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können die von Ihrer Organisation zugeteilte Mandanten-ID Ihres Kunden auch nachschlagen, indem Sie unter [Wie lautet meine Mandanten-ID für Microsoft Azure und Microsoft 365?](https://www.whatismytenantid.com) die Domänennamen-URL des Kunden verwenden.
- **Beschreibung** (optional): Sie können optional eine aussagekräftige Beschreibung für die Mandanten-ID hinzufügen, mit der Sie den Kunden, für den die Zusatzvereinbarung gelten soll, einfacher identifizieren können.

**Geschäftsbedingungen**

Wenn Sie Ihre eigenen Geschäftsbedingungen einschließen möchten, können Sie diese in das Feld für die Geschäftsbedingungen eingeben. Sie können bis zu 10.000 Zeichen in dieses Feld eingeben. Wenn Ihre Geschäftsbedingungen eine längere Beschreibung erfordern, geben Sie in diesem Feld einen einzelnen URL-Link ein, über den Ihre Geschäftsbedingungen aufgerufen werden können. Er wird für Kunden als aktiver Link angezeigt.

Kunden müssen diese Bestimmungen akzeptieren, bevor sie Ihr Angebot testen können.

Wählen Sie **Entwurf speichern** aus, bevor Sie fortfahren.
