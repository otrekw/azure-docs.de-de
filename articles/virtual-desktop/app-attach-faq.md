---
title: 'Häufig gestellte Fragen zu Windows Virtual Desktop: MSIX-Feature zum Anfügen von Apps – Azure'
description: Häufig gestellte Fragen zum MSIX-Feature zum Anfügen von Apps für Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/17/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a63d7f067665836910b91b2911db522f0a92bbb1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88556148"
---
# <a name="msix-app-attach-faq"></a>Häufig gestellte Fragen zum MSIX-Feature zum Anfügen von Apps

In diesem Artikel erhalten Sie Antworten auf häufig gestellte Fragen zum MSIX-Feature zum Anfügen von Apps für Windows Virtual Desktop.

## <a name="does-msix-app-attach-use-fslogix"></a>Verwendet das MSIX-Feature zum Anfügen von Apps FSLogix?

Das MSIX-Feature zum Anfügen von Apps verwendet FSLogix nicht. Anfügen von Apps und FSLogix sind jedoch so konzipiert, dass Sie zusammen verwendet werden können, um eine einheitliche Benutzererfahrung zu ermöglichen.

## <a name="can-i-use-msix-app-attach-outside-of-windows-virtual-desktop"></a>Kann ich das MSIX-Feature zum Anfügen von Apps außerhalb von Windows Virtual Desktop verwenden?

Ja, das MSIX-Feature zum Anfügen von Apps ist ein Feature, das in Windows 10 Enterprise enthalten ist und außerhalb von Windows Virtual Desktop verwendet werden kann. Es gibt jedoch keine Verwaltungsebene für das MSIX-Feature zum Anfügen von Apps außerhalb von Windows Virtual Desktop.

## <a name="how-do-i-get-an-msix-package"></a>Wie rufe ich ein MSIX-Paket ab?

Sie erhalten ein MSIX-Paket bei Ihrem Softwarehersteller. Sie können auch Nicht-MSIX-Pakete in das MSIX-Format konvertieren. Weitere Informationen finden Sie unter [Verschieben vorhandener Installationsprogramme in MSIX](/windows/msix/packaging-tool/create-an-msix-overview#how-to-move-your-existing-installers-to-msix).

## <a name="which-operating-systems-support-msix-app-attach"></a>Welche Betriebssysteme unterstützen das MSIX-Feature zum Anfügen von Apps?

Windows 10 Enterprise und Windows 10 Enterprise (mehrere Sitzungen).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über das MSIX-Feature zum Anfügen von Apps finden Sie in unserer [Übersicht](what-is-app-attach.md) und im [Glossar](app-attach-glossary.md). Oder legen Sie direkt mit dem [Einrichten des MSIX-Features zum Anfügen von Apps](app-attach.md) los.