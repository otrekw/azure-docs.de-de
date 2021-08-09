---
title: Codeintegrität auf der Azure-Plattform – Azure-Sicherheit
description: Erfahren Sie, wie Microsoft sicherstellt, dass nur autorisierte Software ausgeführt wird.
author: yosharm
ms.service: security
ms.subservice: security-fundamentals
ms.topic: article
ms.author: terrylan
manager: rkarlin
ms.date: 06/10/2021
ms.openlocfilehash: e9a58bf83181352adb3894fb159fd212f0f1b7ca
ms.sourcegitcommit: 23040f695dd0785409ab964613fabca1645cef90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112063649"
---
# <a name="platform-code-integrity"></a>Codeintegrität auf der Azure-Plattform

Eine erhebliche Herausforderung beim Betrieb eines komplexen Systems wie Microsoft Azure ist es, sicherzustellen, dass nur autorisierte Software im System ausgeführt wird. Nicht autorisierte Software birgt mehrere Risiken für jedes Unternehmen:

- Sicherheitsrisiken wie dedizierte Angriffstools, benutzerdefinierte Schadsoftware und Drittanbietersoftware mit bekannten Sicherheitsrisiken
- Compliancerisiken, wenn zum Einführen neuer Software nicht der genehmigte Change Management-Prozess verwendet wird
- Qualitätsrisiko durch extern entwickelte Software, die die betrieblichen Anforderungen des Unternehmens möglicherweise nicht erfüllt

In Azure sehen wir uns der gleichen Herausforderung gegenüber, und zwar mit einem erheblichen Komplexitätsgrad. Wir verfügen über Tausende von Servern, auf denen von Tausenden von Technikern entwickelte und gewartete Software ausgeführt wird. Dies ist eine große Angriffsfläche, die nicht allein über Geschäftsprozesse gehandhabt werden kann.

## <a name="adding-an-authorization-gate"></a>Autorisierungsprüfungen

In Azure sind als Teil eines umfassenden Entwicklungsprozesses Prüfungen der Sicherheit, Compliance und Qualität der von uns eingesetzten Software implementiert. Dieser Prozess beinhaltet die Steuerung des Zugriffs auf Quellcode, Peer Code Reviews, statische Analysen zur Suche nach Sicherheitsrisiken, die Einhaltung des Microsoft [Security Development Lifecycle](https://www.microsoft.com/securityengineering/sdl/) (SDL) sowie Funktions- und Qualitätstests. Wir müssen garantieren, dass die von uns bereitgestellte Software diesen Prozess durchlaufen hat. Codeintegrität hilft uns dabei, diesen Anspruch umzusetzen.

## <a name="code-integrity-as-an-authorization-gate"></a>Codeintegrität als Autorisierungsprüfung

Codeintegrität ist ein Dienst auf Kernelebene, der ab Windows Server 2016 verfügbar wurde. Die Codeintegrität kann eine strenge Ausführungssteuerungsrichtlinie anwenden, wenn ein Treiber oder eine dynamisch verknüpfte Bibliothek (DLL) geladen, eine ausführbare Binärdatei oder ein Skript ausgeführt wird. Ähnliche Systeme wie [DM-Verity](https://www.kernel.org/doc/html/latest/admin-guide/device-mapper/verity.html) sind auf Linux verfügbar. Eine Codeintegritätsrichtlinie besteht aus einem Satz von Autorisierungsindikatoren, entweder Codesignaturzertifikate oder [SHA256](https://en.wikipedia.org/wiki/Secure_Hash_Algorithms)-Dateihashes, mit denen der Kernel vor dem Laden oder Ausführen einer Binärdatei oder eines Skripts einen Abgleich durchführt.

Mit Codeintegrität können Systemadministratoren eine Richtlinie definieren, die nur Binärdateien und Skripts autorisiert, die von bestimmten Zertifikaten signiert wurden oder mit angegebenen SHA256-Hashes übereinstimmen. Der Kernel setzt diese Richtlinie durch, indem er die Ausführung aller Daten blockiert, die nicht der festgelegten Richtlinie entsprechen.

Ein Problem bei einer Codeintegritätsrichtlinie ist, dass kritische Software in der Produktion blockiert und so ein Ausfall verursacht werden könnte, wenn die Richtlinie nicht hundertprozentig korrekt ist. Angesichts dieser Bedenken kann man sich fragen, warum es nicht ausreicht, per Sicherheitsüberwachung zu erkennen, wenn nicht autorisierte Software ausgeführt wird. Die Codeintegrität verfügt über einen Überwachungsmodus, der nicht die Ausführung verhindert, sondern warnt, wenn nicht autorisierte Software ausgeführt wird. Warnungen können bei der Behandlung von Compliancerisiken zwar wertvoll sein, aber bei Sicherheitsrisiken wie Ransomware oder benutzerdefinierter Schadsoftware kann eine Verzögerung der Antwort um einige Sekunden den Unterschied zwischen Sicherheit und einer dauerhaften Kompromittierung durch Angreifer ausmachen. Wir haben viel investiert, um in Azure das Risiko zu bekämpfen, dass Codeintegrität zu einem Ausfall bei Kunden beitragen könnte.

## <a name="build-process"></a>Buildprozess

Wie bereits erwähnt, verfügt das Azure-Buildsystem über eine umfangreiche Reihe von Tests, um sicherzustellen, dass Softwareänderungen sicher und konform sind. Sobald ein Build die Validierung abschließt, signiert das Buildsystem ihn mithilfe eines Azure-Buildzertifikats. Das Zertifikat gibt an, dass der Build den gesamten Change Management-Prozess bestanden hat. Der letzte Test, den der Build durchläuft, heißt Codesignaturvalidierung (CSV). Die CSV bestätigt, dass die neu erstellten Binärdateien die Codeintegritätsrichtlinie erfüllen, bevor sie in der Produktion bereitgestellt werden. So haben wir großes Vertrauen, dass wir bei Kunden keinen Ausfall aufgrund falsch signierter Binärdateien verursachen werden. Wenn die CSV ein Problem findet, wird der Build unterbrochen und die entsprechenden Techniker benachrichtigt, um das Problem zu untersuchen und zu beheben.

## <a name="safety-during-deployment"></a>Sicherheit während der Bereitstellung

Obwohl wir die CSV für jeden Build ausführen, besteht immer noch die Möglichkeit, dass eine Änderung oder Inkonsistenz in der Produktion zu einem durch die Codeintegrität bedingten Ausfall kommen kann. Beispielsweise kann auf einem Computer eine alte Version der Codeintegritätsrichtlinie ausgeführt werden, oder er kann sich in einem fehlerhaften Zustand befinden, der zu False Positives der Codeintegrität führt. (Aufgrund des großen Umfangs von Azure haben wir solche Sonderfälle schon beobachtet.) Daher müssen wir uns weiterhin vor dem Risiko eines Ausfalls während der Bereitstellung schützen.

Alle Änderungen in Azure müssen in einer Reihe von Phasen bereitgestellt werden. Die erste davon sind interne Azure-Testinstanzen. Die nächste Phase wird nur für andere Microsoft-Produktteams verwendet. Die letzte Phase ist für Drittkunden vorgesehen. Wenn eine Änderung bereitgestellt wird, durchläuft sie nacheinander jede dieser Phasen und hält an, um die Integrität der Phase zu messen. Wenn festgestellt wird, dass die Änderung keine negativen Auswirkungen hat, wechselt sie zur nächsten Phase. Wenn wir eine fehlerhafte Änderung an einer Codeintegritätsrichtlinie vornehmen, wird die Änderung während dieser stufenweisen Bereitstellung erkannt und ein Rollback ausgeführt.

## <a name="incident-response"></a>Reaktion auf Incidents

Selbst bei diesem mehrschichtigen Schutz ist es immer noch möglich, dass ein Server ordnungsgemäß autorisierte Software blockiert und ein Kundenproblem verursacht, was für uns ein Worst Case-Szenario darstellt. Unsere letzte Schutzschicht ist die Untersuchung durch Menschen. Jedes Mal, wenn die Codeintegrität eine Datei blockiert, löst sie eine Warnung aus, die von den abrufbereiten Technikern untersucht wird. Anhand der Warnung können wir Sicherheitsuntersuchungen durchführen und eingreifen, unabhängig davon, ob das Problem ein Hinweis auf einen echten Angriff, ein False Positive oder eine andere Situation mit potenziellen Auswirkungen auf den Kunden ist. Dadurch wird der Zeitaufwand für die Problembehebung bei der Codeintegrität minimiert.  

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie [Windows 10](/windows/security/threat-protection/device-guard/introduction-to-device-guard-virtualization-based-security-and-windows-defender-application-control) konfigurierbare Codeintegrität verwendet.

Weitere Informationen zu unseren Anstrengungen, die Integrität und Sicherheit von Plattformen zu erhöhen, finden Sie unter:

- [Firmwaresicherheit](firmware.md)
- [Sicherer Start](secure-boot.md)
- [Gemessener Start- und Hostnachweis](measured-boot-host-attestation.md)
- [Project Cerberus](project-cerberus.md)
- [Verschlüsselung ruhender Daten](encryption-atrest.md)
- [Hypervisor-Sicherheit](hypervisor.md)