---
title: Protokollieren von Fehlern und Ausnahmen in MSAL für Python
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen in MSAL für Python protokollieren
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/25/2021
ms.author: marsma
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ms.openlocfilehash: f660bd17954afb4ae02da77e93d4d9cc36f3d355
ms.sourcegitcommit: 3c3ec8cd21f2b0671bcd2230fc22e4b4adb11ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2021
ms.locfileid: "98763281"
---
# <a name="logging-in-msal-for-python"></a>Protokollierung in MSAL für Python

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="msal-for-python-logging"></a>MSAL für Python-Protokollierung

Bei der Protokollierung in MSAL Python wird der standardmäßige Python-Protokollierungsmechanismus verwendet (beispielsweise `logging.info("msg")`). Die MSAL-Protokollierung kann wie folgt konfiguriert werden. (Ein Praxisbeispiel finden Sie unter [username_password_sample](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.0.0/sample/username_password_sample.py#L31L32).)

### <a name="enable-debug-logging-for-all-modules"></a>Aktivieren der Debugprotokollierung für alle Module

Standardmäßig ist die Protokollierung in jedem Python-Skript deaktiviert. Wenn Sie die Debugprotokollierung für alle Module in Ihrem gesamten Python-Skript aktivieren möchten, verwenden Sie Folgendes:

```python
logging.basicConfig(level=logging.DEBUG)
```

### <a name="silence-only-msal-logging"></a>Nur Deaktivieren der MSAL-Protokollierung

Wenn Sie nur die MSAL-Protokollierung deaktivieren möchten, die Debugprotokollierung in allen anderen Modulen in Ihrem Python-Skript aber aktiviert sein soll, deaktivieren Sie die von MSAL Python verwendete Protokollierung:

```Python
logging.getLogger("msal").setLevel(logging.WARN)
```

### <a name="personal-and-organizational-data-in-python"></a>Personen- und organisationsbezogene Daten in Python

Von MSAL für Python werden keine personen- oder organisationsbezogenen Daten protokolliert. Es gibt keine Eigenschaft zum Aktivieren oder Deaktivieren der Protokollierung personen- oder organisationsbezogener Daten.

Sie können die Python-Standardprotokollierung verwenden, um die gewünschten Inhalte zu protokollieren. Sie sind jedoch für den sicheren Umgang mit sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen verantwortlich.

Weitere Informationen zur Protokollierung in Python finden Sie in der [Protokollierungsanleitung](https://docs.python.org/3/howto/logging.html#logging-basic-tutorial) von Python.

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele finden Sie unter [Microsoft Identity Platform-Codebeispiele](sample-v2-code.md).

---