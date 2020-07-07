---
title: Verbinden von Linux-Computern mit Azure Monitor | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Linux-Computer, die in anderen Clouds oder lokal gehostet werden, über den Log Analytics-Agent für Linux mit Azure Monitor verbunden werden können.
ms.subservice: logs
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 01/21/2020
ms.openlocfilehash: 9807d6eeb07b953ab75b328ce64c5166ca52dd2a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80637519"
---
# <a name="connect-linux-computers-to-azure-monitor"></a>Verbinden von Linux-Computern mit Azure Monitor

Zum Überwachen und Verwalten virtueller oder physischer Computer in Ihrem lokalen Rechenzentrum oder einer anderen Cloudumgebung mit Azure Monitor müssen Sie den Log Analytics-Agent bereitstellen und so konfigurieren, dass Berichte an einen Log Analytics-Arbeitsbereich übermittelt werden. Der Agent unterstützt auch die Rolle „Hybrid Runbook Worker“ für Azure Automation.

Der Log Analytics-Agent für Linux kann mit einer der folgenden Methoden installiert werden. Details zur Verwendung der einzelnen Methoden finden Sie weiter unten im Artikel.

* [Laden Sie den Agent manuell herunter, und installieren Sie ihn](#install-the-agent-manually). Dies ist erforderlich, wenn der Linux-Computer keinen Zugriff auf das Internet hat und über das [Log Analytics-Gateway](gateway.md) mit Azure Monitor oder Azure Automation kommuniziert. 
* [Installieren Sie den Agent für Linux mithilfe eines Wrapperskripts](#install-the-agent-using-wrapper-script), das auf GitHub gehostet wird. Dies ist die empfohlene Methode für Installation und Upgrade des Agents, wenn der Computer direkt oder über einen Proxyserver mit dem Internet verbunden ist.

Informationen zur unterstützten Konfiguration finden Sie in den Abschnitten zu [unterstützten Linux-Betriebssystemen](log-analytics-agent.md#supported-linux-operating-systems) und zur [Netzwerkfirewallkonfiguration](log-analytics-agent.md#network-requirements).

>[!NOTE]
>Der Log Analytics-Agent für Linux kann nicht für die Berichterstattung für mehrere Log Analytics-Arbeitsbereiche konfiguriert werden. Er kann nur so konfiguriert werden, dass Berichte sowohl an eine System Center Operations Manager-Verwaltungsgruppe als auch einen Log Analytics Arbeitsbereich gleichzeitig oder jeweils einzeln übermittelt werden.

## <a name="agent-install-package"></a>Agent-Installationspaket

Der Log Analytics-Agent für Linux besteht aus mehreren Paketen. Die Releasedatei enthält die folgenden Pakete, die durch Ausführen des Shell-Pakets mit dem Parameter `--extract` verfügbar sind:

**Paket** | **Version** | **Beschreibung**
----------- | ----------- | --------------
omsagent | 1.12.15 | Der Log Analytics-Agent für Linux
omsconfig | 1.1.1 | Konfigurations-Agent für den Log Analytics-Agent
omi | 1.6.3 | Open Management Infrastructure (OMI) – ein kompakter CIM-Server. *Beachten Sie, dass OMI Stammzugriff erfordert, um einen Cron-Auftrag auszuführen, der für das Funktionieren des Diensts erforderlich ist.*
scx | 1.6.3 | OMI-CIM-Anbieter für Leistungsmetriken für das Betriebssystem
apache-cimprov | 1.0.1 | Apache HTTP Server-Anbieter für die Leistungsüberwachung für OMI Nur installiert, wenn Apache HTTP Server erkannt wird.
mysql-cimprov | 1.0.1 | MySQL Server-Anbieter für die Leistungsüberwachung für OMI Nur installiert, wenn MySQL/MariaDB-Server erkannt wird.
docker-cimprov | 1.0.0 | Docker-Anbieter für OMI. Nur installiert, wenn Docker erkannt wird.

### <a name="agent-installation-details"></a>Agent-Installationsdetails

Nach der Installation der Pakete für den Log Analytics-Agent für Linux werden die folgenden zusätzlichen systemweiten Konfigurationsänderungen angewendet. Diese Artefakte werden entfernt, wenn das „omsagent“-Paket deinstalliert wird.

* Ein Benutzer ohne Berechtigungen mit dem Namen `omsagent` wird erstellt. Der Daemon wird unter diesen Anmeldeinformationen ausgeführt. 
* Eine sudoers-*include*-Datei wird in `/etc/sudoers.d/omsagent` erstellt. Diese autorisiert `omsagent` dazu, die syslog- und omsagent-Daemons neu zu starten. Falls sudo-*include*-Direktiven in der installierten sudo-Version nicht unterstützt werden, werden diese Einträge in `/etc/sudoers` geschrieben.
* Die syslog-Konfiguration wird geändert, um eine Teilmenge von Ereignissen an den Agent weiterzuleiten. Weitere Informationen finden Sie unter [Konfigurieren der Syslog-Datensammlung](data-sources-syslog.md).

Auf einem überwachten Linux-Computer wird der Agent als `omsagent` aufgeführt. `omsconfig` ist der Konfigurations-Agent für den Log Analytics-Agent für Linux, der alle fünf Minuten nach einer neuen im Portal erstellten Konfiguration sucht. Die neue und aktualisierte Konfiguration wird auf die Agent-Konfigurationsdateien unter `/etc/opt/microsoft/omsagent/conf/omsagent.conf` angewendet.

## <a name="obtain-workspace-id-and-key"></a>Abrufen von Arbeitsbereichs-ID und -Schlüssel

Vor der Installation des Log Analytics-Agents für Linux benötigen Sie die Arbeitsbereichs-ID und den Schlüssel für Ihren Log Analytics-Arbeitsbereich. Diese Informationen sind während der Einrichtung des Agents erforderlich, um diesen ordnungsgemäß zu konfigurieren und sicherzustellen, dass er erfolgreich mit Azure Monitor kommunizieren kann.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

1. Wählen Sie in der linken oberen Ecke des Azure-Portals **Alle Dienste** aus. Geben Sie im Suchfeld **Log Analytics** ein. Die Liste wird während Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics-Arbeitsbereiche** aus.

2. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche den zuvor erstellten Arbeitsbereich aus. (Möglicherweise haben Sie ihm den Namen **DefaultLAWorkspace** gegeben.)

3. Wählen Sie **Erweiterte Einstellungen** aus:

    ![Menü „Erweiterte Einstellungen“ für Log Analytics im Azure-Portal](../learn/media/quick-collect-azurevm/log-analytics-advanced-settings-azure-portal.png) 
 
4. Wählen Sie **Verbundene Quellen** und dann **Linux Server** aus.

5. Der Wert rechts von **Arbeitsbereichs-ID** und **Primärschlüssel**. Kopieren Sie beide Angaben, und fügen Sie sie in den von Ihnen bevorzugten Editor ein.

## <a name="install-the-agent-manually"></a>Manuelles Installieren des Agents

Der Log Analytics-Agent für Linux wird in einem selbstextrahierenden und installierbaren Shell-Skriptpaket bereitgestellt. Dieses Paket enthält Debian- und RPM-Pakete für jede Komponente des Agents und kann direkt installiert oder zum Abrufen der einzelnen Pakete extrahiert werden. Es wird ein Paket für x64-Architekturen und eines für x86-Architekturen bereitgestellt. 

> [!NOTE]
> Bei Azure-VMs wird empfohlen, den Agent mithilfe der [Azure Log Analytics VM-Erweiterung](../../virtual-machines/extensions/oms-linux.md) für Linux auf den Computern zu installieren. 

1. [Laden Sie das entsprechende Paket (x64 oder x86) herunter](https://github.com/microsoft/OMS-Agent-for-Linux#azure-install-guide), und übertragen Sie es mithilfe von scp/sftp auf Ihren virtuellen oder physischen Linux-Computer.

2. Installieren Sie das Paket mit dem Argument `--install`. Wenn Sie während der Installation die Integration in einen Log Analytics-Arbeitsbereich vornehmen möchten, geben Sie die zuvor kopierten Parameter `-w <WorkspaceID>` und `-s <workspaceKey>` an.

    >[!NOTE]
    >Sie müssen das Argument `--upgrade` verwenden, falls abhängige Pakete wie omi, scx, omsconfig oder deren ältere Versionen installiert sind. Dies wäre dann der Fall, wenn der System Center Operations Manager-Agent für Linux bereits installiert ist. 

    ```
    sudo sh ./omsagent-*.universal.x64.sh --install -w <workspace id> -s <shared key>
    ```

3. Wenn Sie den Linux-Agent so konfigurieren möchten, dass er über ein Log Analytics-Gateway installiert wird und eine Verbindung mit einem Log Analytics-Arbeitsbereich herstellt, führen Sie den folgenden Befehl unter Angabe der Parameter für Proxy, Arbeitsbereichs-ID und Arbeitsbereichsschlüssel aus. Diese Konfiguration kann in der Befehlszeile durch den Zusatz `-p [protocol://][user:password@]proxyhost[:port]` angegeben werden. Für die *proxyhost*-Eigenschaft kann ein vollqualifizierter Domänenname oder eine IP-Adresse des Log Analytics-Gatewayservers verwendet werden.  

    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -p https://<proxy address>:<proxy port> -w <workspace id> -s <shared key>
    ```

    Wenn eine Authentifizierung erforderlich ist, müssen Sie den Benutzernamen und das Kennwort angeben. Beispiel: 
    
    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -p https://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
    ```

4. Führen Sie den folgenden Befehl aus, und stellen Sie die zuvor kopierte Arbeitsbereichs-ID sowie den Primärschlüssel bereit, um den Linux-Computer so zu konfigurieren, dass er eine Verbindung mit einem Log Analytics-Arbeitsbereich in Azure Government Cloud herstellt.

    ```
    sudo sh ./omsagent-*.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
    ```

Wenn Sie die Agent-Pakete installieren und so konfigurieren möchten, dass Berichte zu einem späteren Zeitpunkt an einen bestimmten Log Analytics-Arbeitsbereich übermittelt werden, führen Sie den folgenden Befehl aus:

```
sudo sh ./omsagent-*.universal.x64.sh --upgrade
```

Wenn Sie die Agent-Pakete aus dem Paket extrahieren möchten, ohne den Agent zu installieren, führen Sie den folgenden Befehl aus:

```
sudo sh ./omsagent-*.universal.x64.sh --extract
```

## <a name="install-the-agent-using-wrapper-script"></a>Installieren des Agents mithilfe eines Wrapperskripts

Mit den folgenden Schritten konfigurieren Sie die Einrichtung des Agents für Log Analytics in Azure und Azure Government Cloud mithilfe des Wrapperskripts für Linux-Computer, die direkt oder über einen Proxyserver kommunizieren können, um den auf GitHub gehosteten Agent herunterzuladen und zu installieren.  

Wenn Ihr Linux-Computer über einen Proxyserver mit Log Analytics kommunizieren muss, kann diese Konfiguration in der Befehlszeile durch den Zusatz `-p [protocol://][user:password@]proxyhost[:port]` angegeben werden. Für die *protocol*-Eigenschaft kann `http` oder `https` und für die *proxyhost*-Eigenschaft kann ein vollqualifizierter Domänenname oder eine IP-Adresse des Proxyservers verwendet werden. 

Beispiel: `https://proxy01.contoso.com:30443`

Wenn in beiden Fällen eine Authentifizierung erforderlich ist, müssen Sie den Benutzernamen und das Kennwort angeben. Beispiel: `https://user01:password@proxy01.contoso.com:30443`

1. Führen Sie den folgenden Befehl aus, und stellen Sie die Arbeitsbereichs-ID sowie den Primärschlüssel bereit, um den Linux-Computer so zu konfigurieren, dass er eine Verbindung mit einem Log Analytics-Arbeitsbereich herstellt. Mit dem folgenden Befehl wird der Agent heruntergeladen, die Prüfsumme des Agents überprüft und anschließend der Agent installiert.
    
    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

    Der folgende Befehl enthält den `-p`-Proxyparameter und die Beispielsyntax für den Fall, dass die Authentifizierung für Ihren Proxyserver erforderlich ist:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://]<proxy user>:<proxy password>@<proxyhost>[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY>
    ```

2. Führen Sie den folgenden Befehl aus, und stellen Sie die zuvor kopierte Arbeitsbereichs-ID sowie den Primärschlüssel bereit, um den Linux-Computer so zu konfigurieren, dass er eine Verbindung mit einem Log Analytics-Arbeitsbereich in Azure Government Cloud herstellt. Mit dem folgenden Befehl wird der Agent heruntergeladen, die Prüfsumme des Agents überprüft und anschließend der Agent installiert. 

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ``` 

    Der folgende Befehl enthält den `-p`-Proxyparameter und die Beispielsyntax für den Fall, dass die Authentifizierung für Ihren Proxyserver erforderlich ist:

   ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -p [protocol://]<proxy user>:<proxy password>@<proxyhost>[:port] -w <YOUR WORKSPACE ID> -s <YOUR WORKSPACE PRIMARY KEY> -d opinsights.azure.us
    ```
2. Starten Sie den Agent neu, indem Sie den folgenden Befehl ausführen: 

    ```
    sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
    ``` 

## <a name="upgrade-from-a-previous-release"></a>Upgrade von einem früheren Release

Das Upgrade von einer früheren Version wird ab Version 1.0.0-47 bei jedem Release unterstützt. Führen Sie die Installation mit dem Parameter `--upgrade` aus, um ein Upgrade aller Komponenten des Agents auf die neueste Version vorzunehmen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zu Neukonfiguration, Upgrade oder Entfernen des Agents auf dem virtuellen Computer finden Sie unter [Verwalten und Warten des Log Analytics-Agents für Windows und Linux](agent-manage.md).

- Lesen Sie [Problembehandlung für den Linux-Agent](agent-linux-troubleshoot.md), wenn bei der Installation oder Verwaltung des Agent Probleme auftreten.
