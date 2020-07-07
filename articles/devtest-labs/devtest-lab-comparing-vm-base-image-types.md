---
title: Vergleich zwischen benutzerdefinierten Images und Formeln in DevTest Labs | Microsoft Docs
description: Erfahren Sie mehr zu den Unterschieden zwischen benutzerdefinierten Images und Formeln als Basis für virtuelle Computer, sodass Sie entscheiden können, welche Methode sich für Ihre Umgebung am besten eignet.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: dac820d0600745906e4b1aeef95806ed5679488a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85484025"
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Vergleich zwischen benutzerdefinierten Images und Formeln in DevTest Labs
Sowohl [benutzerdefinierte Images](devtest-lab-create-template.md) als auch [Formeln](devtest-lab-manage-formulas.md) können als Basis für [neu erstellte virtuelle Computer](devtest-lab-add-vm.md) verwendet werden. Der grundlegende Unterschied zwischen benutzerdefinierten Images und Formeln ist jedoch, dass ein benutzerdefiniertes Image einfach ein auf einer virtuellen Festplatte basierendes Image ist. Eine Formel ist ein Image, das ebenfalls auf einer virtuellen Festplatte, aber *zusätzlich* auch auf vorkonfigurierten Einstellungen basiert, wie beispielsweise VM-Größe, virtuelles Netzwerk, Subnetz und Artefakten. Diese vorkonfigurierten Einstellungen werden mit Standardwerten eingerichtet, die zum Zeitpunkt der Erstellung des virtuellen Computers überschrieben werden können. In diesem Artikel werden einige Vor- und Nachteile der Verwendung von benutzerdefinierten Images im Vergleich zur Verwendung von Formeln erläutert.

## <a name="custom-image-pros-and-cons"></a>Benutzerdefinierte Images – Vor- und Nachteile
Benutzerdefinierte Images stellen eine statische, unveränderliche Möglichkeit zum Erstellen virtueller Computer aus einer bestimmten Umgebung bereit. 

**Vorteile**

* Die VM-Bereitstellung über ein benutzerdefiniertes Image ist schnell, da sich nichts mehr ändert, nachdem der virtuelle Computer aus dem Image erstellt wurde. Anders gesagt: Es müssen keine Einstellungen angewendet werden, da ein benutzerdefiniertes Image ganz einfach ein Image ohne Einstellungen ist. 
* Virtuelle Computer, die aus einem einzigen benutzerdefinierten Image erstellt wurden, sind identisch.

**Nachteile**

* Wenn Sie einen Aspekt des benutzerdefinierten Images aktualisieren müssen, muss das Image neu erstellt werden.  

## <a name="formula-pros-and-cons"></a>Formeln – Vor- und Nachteile
Formeln stellen eine dynamische Möglichkeit bereit, um virtuelle Computer mithilfe der gewünschten Konfigurationseinstellungen zu erstellen.

**Vorteile**

* Änderungen der Umgebung können über Artefakte dynamisch erfasst werden. Wenn Sie z.B. einen virtuellen Computer mit den neuesten Aktualisierungen aus Ihrer Releasepipeline installieren oder den neuesten Code aus Ihrem Repository eintragen möchten, können Sie einfach ein Artefakt angeben, das diese Aktualisierungen bereitstellt bzw. den neuesten Code in der Formel zusammen mit dem Basiszielimage einträgt. Wann immer diese Formel zum Erstellen eines virtuellen Computers verwendet wird, werden die neuesten Aktualisierungen bzw. der neueste Code in diesem virtuellen Computer bereitgestellt bzw. eingetragen. 
* Formeln können Standardeinstellungen definieren, die benutzerdefinierte Images nicht bereitstellen können, wie z.B. VM-Größen und Einstellungen des virtuellen Netzwerks. 
* Die in einer Formel gespeicherten Einstellungen werden als Standardwerte angezeigt, können jedoch bei der Erstellung des virtuellen Computers geändert werden. 

**Nachteile**

* Die Erstellung eines virtuellen Computers aus einer Formel dauert länger als die Erstellung eines virtuellen Computers aus einem benutzerdefinierten Image.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge
* [Custom images or formulas? (Benutzerdefinierte Images oder Formeln?)](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Nächste Schritte
- [Häufig gestellte Fragen zu DevTest Labs](devtest-lab-faq.md)