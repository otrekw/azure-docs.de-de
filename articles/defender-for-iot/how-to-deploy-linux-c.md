---
title: Installieren und Bereitstellen des C-basierten Linux-Agents
description: Erfahren Sie, wie Sie den C-basierten Sicherheits-Agent von Defender für IoT unter Linux installieren und bereitstellen.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 6d3f96ed60ca784402b6d24eea7234f37c4fb959
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449780"
---
# <a name="deploy-defender-for-iot-c-based-security-agent-for-linux"></a>Bereitstellen des C-basierten Sicherheits-Agents von Defender für IoT unter Linux

Erfahren Sie, wie Sie den C-basierten Sicherheits-Agent von Defender für IoT unter Linux installieren und bereitstellen.

- Installieren
- Überprüfen der Bereitstellung
- Deinstallieren des Agents
- Problembehandlung

## <a name="prerequisites"></a>Voraussetzungen

Informationen zu anderen Plattformen und Agent-Varianten finden Sie unter [Choose the right security agent](how-to-deploy-agent.md) (Auswählen des richtigen Sicherheits-Agents).

1. Um den Sicherheits-Agent bereitstellen zu können, müssen Sie auf dem Computer, auf dem Sie ihn installieren möchten, über lokale Administratorrechte verfügen (sudo).

1. [Erstellen Sie ein Sicherheitsmodul](quickstart-create-security-twin.md) für das Gerät.

## <a name="installation"></a>Installation

Verwenden Sie zum Installieren und Bereitstellen des Sicherheits-Agents den folgenden Workflow:

1. Laden Sie von [GitHub](https://aka.ms/iot-security-github-c) die neueste Version auf Ihren Computer herunter.

1. Extrahieren Sie den Inhalt des Pakets, und navigieren Sie zum Ordner _/src/installation_.

1. Führen Sie den folgenden Befehl aus, um dem Skript **InstallSecurityAgent** Ausführungsberechtigungen hinzuzufügen:

   ```
   chmod +x InstallSecurityAgent.sh
   ```

1. Führen Sie anschließend Folgendes aus:

   ```
   ./InstallSecurityAgent.sh -aui <authentication identity> -aum <authentication method> -f <file path> -hn <host name> -di <device id> -i
   ```

   Weitere Informationen zu Authentifizierungsparametern finden Sie unter [Security agent authentication methods](concept-security-agent-authentication-methods.md) (Authentifizierungsmethoden des Sicherheits-Agents).

Dieses Skript führt die folgende Funktion aus:

1. Installieren der erforderlichen Komponenten

1. Hinzufügen eines Dienstbenutzers (mit deaktivierter interaktiver Anmeldung)

1. Installieren des Agents als **Daemon** (wobei vorausgesetzt wird, dass das Gerät **systemd** für die Dienstverwaltung verwendet)

1. Konfigurieren des Agents mit den angegebenen Authentifizierungsparametern

Sollten Sie weitere Hilfe benötigen, führen Sie das Skript mit dem Parameter „–help“ aus:

```./InstallSecurityAgent.sh --help```

### <a name="uninstall-the-agent"></a>Deinstallieren des Agents

Wenn Sie den Agent deinstallieren möchten, führen Sie das Skript mit dem Parameter „–uninstall“ aus:

```./InstallSecurityAgent.sh -–uninstall```

## <a name="troubleshooting"></a>Problembehandlung

Führen Sie Folgendes aus, um den Bereitstellungsstatus zu überprüfen:

```systemctl status ASCIoTAgent.service```

## <a name="next-steps"></a>Nächste Schritte

- Lesen der [Übersicht](overview.md) über den Defender für IoT-Dienst
- Weitere Informationen zur Defender für IoT-[Architektur](architecture.md).
- Aktivieren Sie den [Dienst](quickstart-onboard-iot-hub.md).
- Lesen Sie die [häufig gestellten Fragen](resources-frequently-asked-questions.md).
- Machen Sie sich mit [Sicherheitswarnungen](concept-security-alerts.md) vertraut.
