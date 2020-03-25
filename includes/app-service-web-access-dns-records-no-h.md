---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: 66d3397fae24ee2546dae4eb5d7c9d188f9ede99
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "78944110"
---
> [!NOTE]
> Mithilfe von Azure DNS können Sie einen benutzerdefinierten DNS-Namen für Azure App Service erstellen. Weitere Informationen finden Sie unter [Bereitstellen von benutzerdefinierten Domäneneinstellungen für einen Azure-Dienst mit Azure DNS](../articles/dns/dns-custom-domain.md#app-service-web-apps).
>
>

Melden Sie sich bei der Website Ihres Domänenanbieters an.

Suchen Sie die Seite für die Verwaltung von DNS-Einträgen. Da jeder Domänenanbieter eine eigene Benutzeroberfläche für DNS-Einträge hat, finden Sie Informationen dazu jeweils in der Dokumentation des Anbieters. Suchen Sie nach Bereichen der Website, die mit **Domänenname**, **DNS** oder **Namenserververwaltung** gekennzeichnet sind. 

Die Seite für DNS-Einträge erreichen Sie häufig in Ihren Kontoinformationen über einen Link wie **Eigene Domänen**. Navigieren Sie auf diese Seite, und suchen Sie nach einem Link, der beispielsweise den Namen **Zonendatei**, **DNS-Einträge** oder **Erweiterte Konfiguration** hat.

Der folgende Screenshot zeigt ein Beispiel für eine Seite mit DNS-Einträgen:

![Beispielseite mit DNS-Einträgen](./media/app-service-web-access-dns-records-no-h/example-record-ui.png)

Im Beispielscreenshot würden Sie die Option **Add** (Hinzufügen) wählen, um einen Eintrag zu erstellen. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen. Informationen hierzu finden Sie ebenfalls in der Dokumentation des Anbieters.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie einen gesonderten Link **Änderungen speichern** wählen. 
