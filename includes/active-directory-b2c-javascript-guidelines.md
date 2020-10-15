---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/11/2020
ms.author: mimart
ms.openlocfilehash: ef08f1adc15475cd5dd38548ed39dc57532c0ef5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78185833"
---
## <a name="guidelines-for-using-javascript"></a>Richtlinien für die Verwendung von JavaScript

Beachten Sie die folgenden Richtlinien, wenn Sie die Benutzeroberfläche Ihrer Anwendung mit JavaScript anpassen:

- Binden Sie kein Click-Ereignis an `<a>`-HTML-Elemente.
- Machen Sie sich nicht von Azure AD B2C-Code oder Kommentaren abhängig.
- Ändern Sie nicht die Reihenfolge oder die Hierarchie von Azure AD B2C-HTML-Elementen. Verwenden Sie eine Azure AD B2C-Richtlinie, um die Reihenfolge der Benutzeroberflächenelemente zu steuern.
- Sie können jeden RESTful-Dienst unter Berücksichtigung der folgenden Überlegungen aufrufen:
    - Möglicherweise müssen Sie die CORS-Einstellung für den RESTful-Dienst so festlegen, dass clientseitige HTTP-Aufrufe zulässig sind.
    - Stellen Sie sicher, dass Ihr RESTful-Dienst sicher ist und nur das Protokoll HTTPS verwendet.
    - Verwenden Sie JavaScript nicht direkt, um Azure AD B2C-Endpunkte aufzurufen.
- Sie können Ihren JavaScript-Code einbetten oder einen Link zu externen JavaScript-Dateien hinzufügen. Stellen Sie bei Verwendung einer externen JavaScript-Datei sicher, dass Sie die absolute URL und keine relative URL verwenden.
- JavaScript-Frameworks:
    - Azure AD B2C verwendet eine bestimmte Version von jQuery. Fügen Sie keine weitere Version von jQuery hinzu. Wenn mehr als eine Version auf derselben Seite verwendet wird, treten Probleme auf.
    - Die Verwendung von RequireJS wird nicht unterstützt.
    - Die meisten JavaScript-Frameworks werden von Azure AD B2C nicht unterstützt.
- Azure AD B2C-Einstellungen (wie die aktuelle Sprache der Benutzeroberfläche) können durch Aufrufen der Objekte `window.SETTINGS` und `window.CONTENT` gelesen werden. Ändern Sie nicht den Wert dieser Objekte.
- Verwenden Sie zum Anpassen der Azure AD B2C-Fehlermeldung Lokalisierung in einer Richtlinie.
- Wenn etwas durch die Verwendung einer Richtlinie erreicht werden kann, ist dies im Allgemeinen die empfohlene Methode.
