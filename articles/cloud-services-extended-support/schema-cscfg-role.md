---
title: 'Azure Cloud Services (erweiterter Support): Rollenschema | Microsoft-Dokumentation'
description: Informationen zum Rollenschema für Cloud Services (erweiterter Support)
ms.topic: article
ms.service: cloud-services-extended-support
ms.date: 10/14/2020
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 2567f5bb817a34f6274d5e265a266d67a9c81413
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743982"
---
# <a name="azure-cloud-services-extended-support-config-role-schema"></a>Azure Cloud Services (erweiterter Support): Konfigurationsrollenschema

Das `Role`-Element der Konfigurationsdatei gibt die Anzahl der Rolleninstanzen, die für jede Rolle im Dienst bereitgestellt werden, die Werte aller Konfigurationseinstellungen und die Fingerabdrücke für alle einer Rolle zugeordneten Zertifikate an.

Weitere Informationen zum Azure-Dienstkonfigurationsschema finden Sie unter [Azure Cloud Services (erweiterter Support): Konfigurationsschema (CSCFG-Datei)](schema-cscfg-file.md). Weitere Informationen zum Azure-Dienstdefinitionsschema finden Sie unter [Azure Cloud Services (erweiterter Support): Definitionsschema (CSDEF-Datei)](schema-csdef-file.md).

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

| attribute | BESCHREIBUNG |
| --------- | ----------- |
| name   | Erforderlich. Gibt den Namen der Rolle an. Der Name muss mit dem in der Dienstdefinitionsdatei angegebenen Namen für die Rolle übereinstimmen.|
| vmName | Optional. Gibt den DNS-Namen für einen virtuellen Computer an. Der Name darf höchstens 10 Zeichen enthalten.|

Die folgende Tabelle beschreibt die untergeordneten Elemente des `Role`-Elements.

| Element | BESCHREIBUNG |
| ------- | ----------- |
| Instanzen | Erforderlich. Gibt die Anzahl von Instanzen an, die für die Rolle bereitgestellt werden sollen. Die Anzahl von Instanzen wird durch ein Integer für das `count`-Attribut definiert.|
| Einstellung   | Optional. Gibt einen Einstellungsnamen und -Wert in einer Auflistung von Einstellungen für eine Rolle an. Der Name der Einstellung wird durch eine Zeichenfolge für das `name`-Attribut und der Wert der Einstellung durch eine Zeichenfolge für das `value`-Attribut definiert.|
| Zertifikat | Optional. Gibt den Namen, den Fingerabdruck und den Algorithmus eines Dienstzertifikats an, das der Rolle zugeordnet werden soll. Der Name des Zertifikats wird durch eine Zeichenfolge für das `name`-Attribut definiert. Der Zertifikatfingerabdruck wird durch eine Zeichenfolge von Hexadezimalzahlen ohne Leerzeichen für das `thumbprint`-Attribut definiert. Die Hexadezimalzahlen müssen durch Ziffern und Großbuchstaben dargestellt werden. Der Zertifikatalgorithmus wird durch eine Zeichenfolge für das `thumbprintAlgorithm`-Attribut definiert.|

## <a name="see-also"></a>Siehe auch
[Azure Cloud Services (erweiterter Support): Konfigurationsschema (CSCFG-Datei)](schema-cscfg-file.md)