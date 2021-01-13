---
title: Problembehandlung für häufige Fehler
description: Erfahren Sie, wie Sie Probleme beim Erstellen, Zuweisen und Entfernen von Blaupausen beheben, wie z.B. Richtlinienverletzungen und Blaupausenparameterfunktionen.
ms.date: 10/14/2020
ms.topic: troubleshooting
ms.openlocfilehash: a1689141f95aaac9183391af79edb0cabf5343b6
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058283"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Problembehandlung mit Azure Blueprints

Beim Erstellen, Zuweisen oder Entfernen von Blaupausen können Fehler auftreten. In diesem Artikel wird beschrieben, welche Fehler auftreten und wie diese behoben werden können.

## <a name="finding-error-details"></a>Ermitteln von Fehlerdetails

Viele Fehler sind das Ergebnis der Zuweisung einer Blaupause zu einem Bereich. Wenn bei einer Zuweisung ein Fehler auftritt, finden Sie in der Blaupause Informationen zur fehlerhaften Bereitstellung. Diese Informationen enthalten Hinweise zum Problem, damit Sie es beheben können und die nächste Bereitstellung erfolgreich verläuft.

1. Wählen Sie **Alle Dienste** im linken Bereich aus. Suchen Sie nach **Blaupausen**, und wählen Sie die Option aus.

1. Wählen Sie auf der linken Seite **Zugewiesene Blaupausen** aus, und verwenden Sie das Suchfeld, um die Blaupausenzuweisungen zu filtern und nach der fehlerhaften Zuweisung zu suchen. Sie können die Tabelle der Zuweisungen auch nach der Spalte **Bereitstellungsstatus** sortieren, um alle fehlerhaften Zuweisungen gruppiert anzuzeigen.

1. Wählen Sie die Blaupause mit dem Status _Fehler_ aus, oder klicken Sie mit der rechten Maustaste, und wählen Sie **Zuweisungsdetails anzeigen** aus.

1. In einem roten Banner oben auf der Seite „Blaupausenzuweisung“ wird darauf hingewiesen, dass die Zuweisung fehlerhaft ist. Wählen Sie eine beliebige Stelle des Banners aus, um weitere Details anzuzeigen.

Es ist üblich, dass der Fehler durch ein Artefakt und nicht durch die Blaupause als Ganzes verursacht wird. Wenn ein Artefakt eine Key Vault-Instanz erstellt und Azure Policy die Key Vault-Erstellung verhindert, tritt für die gesamte Zuweisung ein Fehler auf.

## <a name="general-errors"></a>Allgemeine Fehler

### <a name="scenario-policy-violation"></a><a name="policy-violation"></a>Szenario: Richtlinienverletzung

#### <a name="issue"></a>Problem

Bei der Vorlagenbereitstellung ist aufgrund einer Richtlinienverletzung ein Fehler aufgetreten.

#### <a name="cause"></a>Ursache

Eine Richtlinie kann aus verschiedenen Gründen einer Bereitstellung widersprechen:

- Die zu erstellende Ressource ist durch Richtlinien eingeschränkt (üblicherweise SKU- oder Standortbeschränkungen).
- Die Bereitstellung legt Felder fest, die über die Richtlinie konfiguriert werden (häufig mit Tags).

#### <a name="resolution"></a>Lösung

Ändern Sie die Blaupause so, dass sie nicht in Konflikt mit den Richtlinien in den Fehlerdetails steht. Wenn diese Änderung nicht möglich ist, besteht eine alternative Option darin, den Umfang der Richtlinienzuordnung so zu ändern, dass die Blaupause der Richtlinie nicht mehr widerspricht.

### <a name="scenario-blueprint-parameter-is-a-function"></a><a name="escape-function-parameter"></a>Szenario: Blaupausenparameter ist eine Funktion

#### <a name="issue"></a>Problem

Blaupausenparameter, die Funktionen sind, werden vor der Übergabe an Artefakte verarbeitet.

#### <a name="cause"></a>Ursache

Die Übergabe eines Blaupausenparameters, der eine Funktion verwendet, wie z.B. `[resourceGroup().tags.myTag]`, an ein Artefakt resultiert darin, dass das verarbeitete Ergebnis der Funktion auf dem Artefakt anstelle der dynamischen Funktion festgelegt wird.

#### <a name="resolution"></a>Lösung

Um eine Funktion als Parameter zu übergeben, versehen Sie die gesamte Zeichenfolge mit `[` als Escapezeichen, sodass der Blaupausenparameter aussieht wie `[[resourceGroup().tags.myTag]`. Das Escapezeichen bewirkt, dass Blueprints den Wert bei der Verarbeitung der Blaupause als Zeichenfolge behandelt. Dann platziert der Blueprints-Dienst die Funktion auf dem Artefakt, sodass sie so dynamisch wie erwartet sein kann. Weitere Informationen finden Sie unter [Syntax und Ausdrücke in Azure Resource Manager-Vorlagen](../../../azure-resource-manager/templates/template-expressions.md).

## <a name="delete-errors"></a>Löschen von Fehlern

### <a name="scenario-assignment-deletion-timeout"></a><a name="assign-delete-timeout"></a>Szenario: Timeout beim Löschen einer Zuweisung

#### <a name="issue"></a>Problem

Das Löschen einer Blaupausenzuweisung wird nicht abgeschlossen.

#### <a name="cause"></a>Ursache

Wenn eine Blaupausenzuweisung gelöscht wird, kann es vorkommen, dass sie in einem Nicht-Endzustand verbleibt. Dieser Zustand wird erzeugt, wenn das Löschen von Ressourcen, die von der Blaupausenzuweisung erstellt wurden, noch aussteht, oder wenn diese keinen Statuscode an Azure Blueprints zurückgeben.

#### <a name="resolution"></a>Lösung

Blaupausenzuweisungen in einem Nicht-Endzustand werden nach einem Timeout von _sechs Stunden_ automatisch als **Fehlerhaft** markiert. Nachdem der Timeout den Zustand der Blaupausenzuweisung angepasst hat, kann der Löschvorgang wiederholt werden.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

- Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
- Mit [@AzureSupport](https://twitter.com/azuresupport) verbinden – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit durch Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.
- Wenn Sie weitere Hilfe benötigen, können Sie einen Azure-Supportvorgang anlegen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.