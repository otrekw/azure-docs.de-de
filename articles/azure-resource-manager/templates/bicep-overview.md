---
title: Sprache „Bicep“ für Azure Resource Manager-Vorlagen
description: Hier wird die Sprache „Bicep“ für das Bereitstellen der Infrastruktur in Azure über Azure Resource Manager-Vorlagen beschrieben.
ms.topic: conceptual
ms.date: 03/23/2021
ms.openlocfilehash: 74028c682b48a492c2e8f13bef538d1694370cbd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104955906"
---
# <a name="what-is-bicep-preview"></a>Was ist Bicep (Vorschau)?

Bicep ist eine Sprache für die deklarative Bereitstellung von Azure-Ressourcen. Sie können Bicep anstelle von JSON verwenden, um Azure Resource Manager-Vorlagen (ARM-Vorlagen) zu erstellen. Bicep vereinfacht die Dokumenterstellung durch Bereitstellen einer präzisen Syntax, einer besseren Unterstützung für die Wiederverwendung von Code sowie einer verbesserten Typsicherheit. Bicep ist eine domänenspezifische Sprache (Domain-specific Language, DSL), was bedeutet, dass sie für ein bestimmtes Szenario oder eine bestimmte Domäne entworfen wurde. Sie ist nicht als allgemeine Programmiersprache zum Schreiben von Anwendungen gedacht.

Die JSON-Syntax zum Erstellen einer Vorlage kann ausführlich sein und einen komplizierten Ausdruck erfordern. Bicep verbessert diese Funktionalität, ohne dass die Fähigkeiten einer JSON-Vorlage verloren gehen. Es handelt sich um eine transparente Abstraktion der JSON-für-ARM-Vorlagen. Jede Bicep-Datei wird in eine ARM-Standardvorlage kompiliert. In einer ARM-Vorlage gültige Ressourcentypen, API-Versionen und Eigenschaften sind auch in einer Bicep-Datei gültig. Das aktuelle Release enthält eine Reihe von [bekannten Einschränkungen](#known-limitations).

Bicep ist zurzeit als Vorschau verfügbar. Informationen zur Überwachung des Status der Arbeit finden Sie im [Bicep-Projektrepository](https://github.com/Azure/bicep).

Weitere Informationen zu Bicep finden Sie im folgenden Video:

> [!VIDEO https://www.youtube.com/embed/sc1kJfcRQgY]

## <a name="get-started"></a>Erste Schritte

[Installieren Sie die Tools](bicep-install.md), um Bicep verwenden zu können.

Nachdem Sie die Tools installiert haben, können Sie das [Bicep-Tutorial](./bicep-tutorial-create-first-bicep.md) testen. In der Tutorialreihe werden die Struktur und die Funktionen von Bicep erläutert. Sie stellen Bicep-Dateien bereit und konvertieren eine ARM-Vorlage in die entsprechende Bicep-Datei.

Mit dem [Bicep Playground](https://aka.ms/bicepdemo) können Sie die entsprechenden JSON- und Bicep-Dateien zum Vergleich nebeneinander anzeigen.

Informationen zum Konvertieren von vorhandenen ARM-Vorlagen in Bicep finden Sie unter [Konvertieren von ARM-Vorlagen zwischen JSON und Bicep](bicep-decompile.md).

## <a name="bicep-improvements"></a>Bicep-Verbesserungen

Bicep bietet im Vergleich zum entsprechenden JSON-Code eine einfachere und präzisere Syntax. Sie verwenden keine `[...]`-Ausdrücke. Stattdessen rufen Sie Funktionen direkt auf und erhalten Werte von Parametern und Variablen. Sie weisen jeder bereitgestellten Ressource einen symbolischen Namen zu, sodass Sie einfach in Ihrer Vorlage auf diese Ressource verweisen können.

Der folgende JSON-Code gibt z. B. einen Ausgabewert aus einer Ressourceneigenschaft zurück.

```json
"outputs": {
  "hostname": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses', variables('publicIPAddressName'))).dnsSettings.fqdn]"
    },
}
```

Der entsprechende Ausgabeausdruck in Bicep ist einfacher zu schreiben. Im folgenden Beispiel wird dieselbe Eigenschaft mit dem symbolischen Namen **publicIP** für eine Ressource zurückgegeben, die in der Vorlage definiert ist:

```bicep
output hostname string = publicIP.properties.dnsSettings.fqdn
```

Einen vollständigen Vergleich der Syntax finden Sie unter [Vergleichen von JSON und Bicep für Vorlagen](compare-template-syntax.md).

Bicep verwaltet Abhängigkeiten zwischen Ressourcen automatisch. Sie können die Einstellung `dependsOn` vermeiden, wenn der symbolische Name einer Ressource in einer anderen Ressourcendeklaration verwendet wird.

Mit Bicep können Sie Ihr Projekt in mehrere Module aufteilen.

Die Struktur der Bicep-Datei ist flexibler als die JSON-Vorlage. Sie können an beliebiger Stelle in der Datei Parameter, Variablen und Ausgaben deklarieren. In JSON müssen Sie alle Parameter, Variablen und Ausgaben in den entsprechenden Abschnitten der Vorlage deklarieren.

Die VS Code-Erweiterung für Bicep bietet eine umfassende Überprüfung sowie IntelliSense. Mit der Funktion IntelliSense können Sie beispielsweise die Eigenschaften einer Ressource abrufen.

## <a name="known-limitations"></a>Bekannte Einschränkungen

Aktuell gelten die folgenden Einschränkungen:

* Für Kopierschleifen kann weder Modus noch Batchgröße festgelegt werden.
* Schleifen und Bedingungen können nicht miteinander kombiniert werden.
* Einzeilige Objekte und Arrays wie `['a', 'b', 'c']` werden nicht unterstützt.

## <a name="faq"></a>Häufig gestellte Fragen

**Warum wird eine neue Sprache entwickelt, anstatt eine vorhandene zu verwenden?**

Sie können sich Bicep eher als Revision der vorhandenen ARM-Vorlagensprache als eine neue Sprache vorstellen. Die Syntax wurde geändert, aber die Kernfunktionen und die Runtime bleiben unverändert.

Vor der Entwicklung von Bicep wurde die Verwendung einer vorhandenen Programmiersprache in Betracht gezogen. Wir sind der Ansicht, dass unserer Zielgruppe das Erlernen der Bicep-Sprache leichter fällt als das Erlernen einer völlig anderen Sprache.

**Warum liegt der Fokus nicht auf Terraform oder anderen Infrastructure-as-Code-Angeboten von Drittanbietern?**

Verschiedene Benutzer bevorzugen unterschiedliche Konfigurationssprachen und -tools. Wir möchten sicherstellen, dass alle diese Tools eine optimale Nutzung in Azure bieten. Bicep ist Teil dieser Lösung.

Wenn Sie mit Terraform zufrieden sind, gibt es keinen Grund für einen Wechsel. Microsoft ist bestrebt, die optimale Nutzung von Terraform in Azure zu bieten.

Für Kunden, die ARM-Vorlagen ausgewählt haben, verbessert Bicep die Dokumenterstellung. Außerdem hilft Bicep Kunden beim Produktübergang, wenn diese keine Infrastructure-as-Code-Lösung verwendet haben.

**Kann Bicep nur in Azure verwendet werden?**

Bicep ist eine domänenspezifische Sprache mit dem Fokus auf der Bereitstellung von Komplettlösungen in Azure. Damit dieses Ziel erreicht werden kann, ist die Verwendung einiger APIs außerhalb von Azure nötig. Erwartungsgemäß werden Erweiterbarkeitspunkte für diese Szenarios bereitgestellt.

**Was geschieht mit meinen vorhandenen ARM-Vorlagen?**

Diese funktionieren weiterhin genau so wie zuvor. Sie müssen keine Änderungen vornehmen. Die JSON-Sprache wird für zugrunde liegende ARM-Vorlagen weiterhin unterstütz. Bicep-Dateien werden in JSON kompiliert, und dieser JSON-Code wird für die Bereitstellung an Azure gesendet.

Sie können die [JSON-Dateien umgehend in Bicep-Dateien konvertieren](bicep-decompile.md).

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem [Bicep-Tutorial](./bicep-tutorial-create-first-bicep.md).
