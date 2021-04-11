---
title: Rollenschema in Azure Cloud Services (klassisch) | Microsoft-Dokumentation
description: Durch das Rollenelement einer Dienstkonfigurationsdatei wird angegeben, wie viele Rolleninstanzen für die einzelnen Rollen, Konfigurationswerte und Zertifikatfingerabdrücke bereitgestellt werden sollen.
ms.topic: article
ms.service: cloud-services
ms.subservice: deployment-files
ms.date: 10/14/2020
ms.author: tagore
author: tanmaygore
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: dd46a37ada1fb96797faee6c8491359561e065d7
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105934154"
---
# <a name="azure-cloud-services-classic-config-role-schema"></a>Rollenschema der Azure Cloud Services-Konfiguration (klassisch)

> [!IMPORTANT]
> [Azure Cloud Services (erweiterter Support)](../cloud-services-extended-support/overview.md) ist ein neues auf Azure Resource Manager basierendes Bereitstellungsmodell für Azure Cloud Services. Im Zuge dieser Änderung wurden Azure Cloud Services-Instanzen, die unter dem Azure Service Manager-basierten Bereitstellungsmodell ausgeführt werden, in „Cloud Services (klassisch)“ umbenannt. Für alle neuen Bereitstellungen wird [Azure Cloud Services (erweiterter Support)](../cloud-services-extended-support/overview.md) verwendet.

Das `Role`-Element der Konfigurationsdatei gibt die Anzahl der Rolleninstanzen, die für jede Rolle im Dienst bereitgestellt werden, die Werte aller Konfigurationseinstellungen und die Fingerabdrücke für alle einer Rolle zugeordneten Zertifikate an.

Weitere Informationen zum Azure-Dienstkonfigurationsschema finden Sie unter [Azure Cloud Services Config Schema (.cscfg File)](schema-cscfg-file.md) (Azure Cloud Services-Konfigurationsschema (CSCFG-Datei)). Weitere Informationen zum Azure-Dienstdefinitionsschema finden Sie unter [Azure Cloud Services Definition Schema (.csdef File)](schema-csdef-file.md) (Azure Cloud Services-Definitionsschema (CSDEF-Datei)).

##  <a name="role-element"></a><a name="Role"></a> Role-Element
Das folgende Beispiel zeigt das `Role`-Element und seine untergeordneten Elemente.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

In der folgenden Tabelle sind die Attribute des `Role`-Elements beschrieben.

| Attribut | BESCHREIBUNG |
| --------- | ----------- |
| name   | Erforderlich. Gibt den Namen der Rolle an. Der Name muss mit dem in der Dienstdefinitionsdatei angegebenen Namen für die Rolle übereinstimmen.|
| vmName | Optional. Gibt den DNS-Namen für einen virtuellen Computer an. Der Name darf höchstens 10 Zeichen enthalten.|

Die folgende Tabelle beschreibt die untergeordneten Elemente des `Role`-Elements.

| Element | Beschreibung |
| ------- | ----------- |
| Instanzen | Erforderlich. Gibt die Anzahl von Instanzen an, die für die Rolle bereitgestellt werden sollen. Die Anzahl von Instanzen wird durch ein Integer für das `count`-Attribut definiert.|
| Einstellung   | Optional. Gibt einen Einstellungsnamen und -Wert in einer Auflistung von Einstellungen für eine Rolle an. Der Name der Einstellung wird durch eine Zeichenfolge für das `name`-Attribut und der Wert der Einstellung durch eine Zeichenfolge für das `value`-Attribut definiert.|
| Zertifikat | Optional. Gibt den Namen, den Fingerabdruck und den Algorithmus eines Dienstzertifikats an, das der Rolle zugeordnet werden soll. Der Name des Zertifikats wird durch eine Zeichenfolge für das `name`-Attribut definiert. Der Zertifikatfingerabdruck wird durch eine Zeichenfolge von Hexadezimalzahlen ohne Leerzeichen für das `thumbprint`-Attribut definiert. Die Hexadezimalzahlen müssen durch Ziffern und Großbuchstaben dargestellt werden. Der Zertifikatalgorithmus wird durch eine Zeichenfolge für das `thumbprintAlgorithm`-Attribut definiert.|

## <a name="see-also"></a>Weitere Informationen
[Azure Cloud Services Config Schema (.cscfg File)](schema-cscfg-file.md) (Azure Cloud Services-Konfigurationsschema (CSCFG-Datei))