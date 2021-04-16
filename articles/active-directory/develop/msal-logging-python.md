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
ms.openlocfilehash: 1d52b017f94785f5fb25a25f127ae52d96e97d8b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104578752"
---
# <a name="logging-in-msal-for-python"></a>Protokollierung in MSAL für Python

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="msal-for-python-logging"></a>MSAL für Python-Protokollierung

Die Protokollierung in MSAL für Python nutzt das [logging-Modul in der Python-Standardbibliothek](https://docs.python.org/3/library/logging.html). Die MSAL-Protokollierung kann wie folgt konfiguriert werden. (Ein Praxisbeispiel finden Sie unter [username_password_sample](https://github.com/AzureAD/microsoft-authentication-library-for-python/blob/1.0.0/sample/username_password_sample.py#L31L32).)

### <a name="enable-debug-logging-for-all-modules"></a>Aktivieren der Debugprotokollierung für alle Module

Standardmäßig ist die Protokollierung in jedem Python-Skript deaktiviert. Wenn Sie die ausführliche Protokollierung für **alle** Python-Module im Skript aktivieren möchten, verwenden Sie `logging.basicConfig` mit dem Grad `logging.DEBUG`:

```python
import logging

logging.basicConfig(level=logging.DEBUG)
```

Dadurch werden alle Protokollmeldungen, die an das logging-Modul übergeben werden, an der Standardausgabe ausgegeben.

### <a name="configure-msal-logging-level"></a>Konfigurieren des MSAL-Protokolliergrads

Sie können den Protokolliergrad für den Protokollanbieter von MSAL für Python konfigurieren, indem Sie die `logging.getLogger()`-Methode mit `"msal"` als Namen der Protokollierung verwenden:

```python
import logging

logging.getLogger("msal").setLevel(logging.WARN)
```

### <a name="configure-msal-logging-with-azure-app-insights"></a>Konfigurieren der MSAL-Protokollierung mit Azure Application Insights

Python-Protokolle werden einen Protokollhandler übergeben. Standardmäßig ist dies `StreamHandler`. Um MSAL-Protokolle an eine Application Insights-Instanz mit einem Instrumentierungsschlüssel zu senden, verwenden Sie den von der Bibliothek `opencensus-ext-azure` bereitgestellten `AzureLogHandler`.

Fügen Sie zum Installieren von `opencensus-ext-azure` das Paket `opencensus-ext-azure` aus PyPI Ihren Abhängigkeiten hinzu, oder verwenden Sie pip install:

```console
pip install opencensus-ext-azure
```

Ändern Sie dann den Standardhandler des `"msal"`-Protokollanbieters in eine Instanz von `AzureLogHandler`, deren Instrumentierungsschlüssel in der Umgebungsvariable `APP_INSIGHTS_KEY` festgelegt ist:

```python
import logging
import os

from opencensus.ext.azure.log_exporter import AzureLogHandler

APP_INSIGHTS_KEY = os.getenv('APP_INSIGHTS_KEY')

logging.getLogger("msal").addHandler(AzureLogHandler(connection_string='InstrumentationKey={0}'.format(APP_INSIGHTS_KEY))
```

### <a name="personal-and-organizational-data-in-python"></a>Personen- und organisationsbezogene Daten in Python

Von MSAL für Python werden keine personen- oder organisationsbezogenen Daten protokolliert. Es gibt keine Eigenschaft zum Aktivieren oder Deaktivieren der Protokollierung personen- oder organisationsbezogener Daten.

Sie können die Python-Standardprotokollierung verwenden, um die gewünschten Inhalte zu protokollieren. Sie sind jedoch für den sicheren Umgang mit sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen verantwortlich.

Weitere Informationen zur Protokollierung in Python finden Sie in der [Protokollierungsanleitung](https://docs.python.org/3/howto/logging.html#logging-basic-tutorial) von Python.

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele finden Sie unter [Microsoft Identity Platform-Codebeispiele](sample-v2-code.md).
