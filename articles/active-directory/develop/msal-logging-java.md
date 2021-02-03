---
title: Protokollieren von Fehlern und Ausnahmen in MSAL für Java
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie Fehler und Ausnahmen in MSAL für Java protokollieren
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
ms.openlocfilehash: d3fa13a6751b2d8acf1fc99aecbf174f1823fb0b
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98954917"
---
# <a name="logging-in-msal-for-java"></a>Protokollierung in MSAL für Java

[!INCLUDE [MSAL logging introduction](../../../includes/active-directory-develop-error-logging-introduction.md)]

## <a name="msal-for-java-logging"></a>MSAL für Java-Protokollierung

Mit MSAL für Java können Sie die Protokollierungsbibliothek nutzen, die Sie bereits mit Ihrer App verwenden, sofern sie mit SLF4J kompatibel ist. MSAL für Java verwendet [Simple Logging Facade for Java](http://www.slf4j.org/) (SLF4J) als einfache Fassade oder Abstraktion für verschiedene Protokollierungsframeworks (beispielsweise [java.util.logging](https://docs.oracle.com/javase/7/docs/api/java/util/logging/package-summary.html), [Logback](http://logback.qos.ch/) und [Log4j](https://logging.apache.org/log4j/2.x/)). Mit SLF4J kann der Benutzer zur Bereitstellungszeit das gewünschte Protokollierungsframework einbinden.

Wenn Sie also beispielsweise Logback als Protokollierungsframework in Ihrer Anwendung verwenden möchten, fügen Sie der Maven-POM-Datei für Ihre Anwendung die Logback-Abhängigkeit hinzu:

```xml
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

Fügen Sie anschließend die Logback-Konfigurationsdatei hinzu:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">

</configuration>
```

SLF4J wird zur Bereitstellungszeit automatisch an Logback gebunden. MSAL-Protokolle werden in die Konsole geschrieben.

Eine Anleitung für die Bindung an andere Protokollierungsframeworks finden Sie im [SLF4J-Handbuch](http://www.slf4j.org/manual.html).

### <a name="personal-and-organization-information"></a>Personen- oder organisationsbezogene Informationen

Von der MSAL-Protokollierung werden standardmäßig keine personen- oder organisationsbezogenen Daten erfasst oder protokolliert. Im folgenden Beispiel ist die Protokollierung personen- oder organisationsbezogener Daten standardmäßig deaktiviert:

```java
    PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
            .authority(AUTHORITY)
            .build();
```

Aktivieren Sie die Protokollierung personen- oder organisationsbezogener Daten, indem Sie `logPii()` im Clientanwendungsersteller festlegen. Wenn Sie die Protokollierung personen- oder organisationsbezogener Daten aktivieren, muss Ihre App die Verantwortung für den sicheren Umgang mit hochgradig sensiblen Daten sowie für die Einhaltung gesetzlicher Anforderungen übernehmen.

Im folgenden Beispiel ist die Protokollierung personen- oder organisationsbezogener Daten aktiviert:

```java
PublicClientApplication app2 = PublicClientApplication.builder(PUBLIC_CLIENT_ID)
        .authority(AUTHORITY)
        .logPii(true)
        .build();
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Codebeispiele finden Sie unter [Microsoft Identity Platform-Codebeispiele](sample-v2-code.md).