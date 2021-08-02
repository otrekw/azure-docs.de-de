---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/11/2021
ms.author: mimart
ms.openlocfilehash: cd6bddb6988555b336cb920c481fe675e9f7a276
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112073002"
---
1. Wählen Sie die von Ihnen erstellte Anwendung *my-api1* aus, um ihre **Übersicht** zu öffnen.
1. Wählen Sie unter **Verwalten** die Option **Eine API verfügbar machen** aus.
1. Wählen Sie neben **Anwendungs-ID-URI** den Link **Festlegen** aus. Ersetzen Sie den Standardwert (eine GUID) durch einen eindeutigen Namen, z. B. `tasks-api`, und wählen Sie dann **Speichern** aus. Wenn Ihre Webanwendung ein Zugriffstoken für die Web-API anfordert, sollte sie diesen URI als Präfix für jeden Bereich hinzufügen, den Sie für die API definieren.
1. Wählen Sie unter **Durch diese API definierte Bereiche** die Option **Bereich hinzufügen** aus.
1. Geben Sie die folgenden Werte ein, um einen Bereich zu erstellen, der Lesezugriff auf die API definiert, und wählen Sie dann **Bereich hinzufügen** aus:
    1. **Bereichsname**: `tasks.read`
    1. **Anzeigename der Administratoreinwilligung**: `Read access to tasks API`
    1. **Beschreibung der Administratoreinwilligung**: `Allows read access to the tasks API`
1. Wählen Sie **Bereich hinzufügen** aus, geben Sie die folgenden Werte ein, um einen Bereich hinzuzufügen, der Schreibzugriff auf die API definiert, und wählen Sie dann **Bereich hinzufügen** aus:
    1. **Bereichsname**: `tasks.write`
    1. **Anzeigename der Administratoreinwilligung**: `Write access to tasks API`
    1. **Beschreibung der Administratoreinwilligung**: `Allows write access to the tasks API`