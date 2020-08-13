---
title: Behandeln von Verbindungsproblemen im Zusammenhang mit dem Azure Arc für Server-Agent
description: In diesem Artikel erfahren Sie, wie Sie Probleme mit dem Connected Machine-Agent behandeln und beheben, die im Zusammenhang mit Azure Arc für Server (Vorschauversion) bei dem Versuch auftreten, eine Verbindung mit dem Dienst herzustellen.
ms.date: 07/20/2020
ms.topic: conceptual
ms.openlocfilehash: 5cd2560279085485a8ac7b285e4f601060a924f1
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88118007"
---
# <a name="troubleshoot-the-connected-machine-agent-connection-issues"></a>Behandeln von Verbindungsproblemen für den Connected Machine-Agent

Dieser Artikel enthält Informationen zur Behandlung und Behebung von Problemen, die auftreten können, wenn versucht wird, den Connected Machine-Agent von Azure Arc für Server (Vorschauversion) für Windows oder Linux zu konfigurieren. Hier wird sowohl die interaktive als auch die skalierbare Installationsmethode beim Konfigurieren der Verbindung mit dem Dienst behandelt. Allgemeine Informationen finden Sie in der [Übersicht über Arc für Server](./overview.md).

## <a name="agent-verbose-log"></a>Ausführliches Agent-Protokoll

Für die weiter unten in diesem Artikel beschriebenen Schritte benötigen Sie mindestens das ausführliche Protokoll. Es enthält die Ausgabe der Befehle des Tools **azcmagent**, wenn das Argument „verbose“ (-v) verwendet wird. Die Protokolldateien werden in `%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log` (Windows) bzw. in `/var/opt/azcmagent/log/azcmagent.log` (Linux) geschrieben.

### <a name="windows"></a>Windows

Hier sehen Sie ein Beispiel für den Befehl zum Aktivieren der ausführlichen Protokollierung mit dem Connected Machine-Agent für Windows bei einer interaktiven Installation:

```
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Hier sehen Sie ein Beispiel für den Befehl zum Aktivieren der ausführlichen Protokollierung mit dem Connected Machine-Agent für Windows bei einer skalierbaren Installation unter Verwendung eines Dienstprinzipals:

```
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect `
  --service-principal-id "{serviceprincipalAppID}" `
  --service-principal-secret "{serviceprincipalPassword}" `
  --resource-group "{ResourceGroupName}" `
  --tenant-id "{tenantID}" `
  --location "{resourceLocation}" `
  --subscription-id "{subscriptionID}"
  --verbose
```

### <a name="linux"></a>Linux

Hier sehen Sie ein Beispiel für den Befehl zum Aktivieren der ausführlichen Protokollierung mit dem Connected Machine-Agent für Linux bei einer interaktiven Installation:

>[!NOTE]
>Sie müssen auf Linux-Computern über *Stamm*zugriffsberechtigungen verfügen, um **azcmagent** ausführen zu können.

```
azcmagent connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Hier sehen Sie ein Beispiel für den Befehl zum Aktivieren der ausführlichen Protokollierung mit dem Connected Machine-Agent für Linux bei einer skalierbaren Installation unter Verwendung eines Dienstprinzipals:

```
azcmagent connect \
  --service-principal-id "{serviceprincipalAppID}" \
  --service-principal-secret "{serviceprincipalPassword}" \
  --resource-group "{ResourceGroupName}" \
  --tenant-id "{tenantID}" \
  --location "{resourceLocation}" \
  --subscription-id "{subscriptionID}"
  --verbose
```

## <a name="agent-connection-issues-to-service"></a>Probleme bei der Agent-Verbindung mit dem Dienst

Die folgende Tabelle enthält einige der bekannten Fehler sowie Vorschläge zur Problembehandlung und -behebung:

|Meldung |Fehler |Wahrscheinliche Ursache |Lösung |
|--------|------|---------------|---------|
|„Failed to acquire authorization token device flow“ (Fehler beim Abrufen des Authentifizierungstoken-Geräteflows.) |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp 40.126.9.7:443: connect: network is unreachable.` |Der Endpunkt `login.windows.net` ist nicht erreichbar. | Überprüfen Sie die Konnektivität mit dem Endpunkt. |
|„Failed to acquire authorization token device flow“ (Fehler beim Abrufen des Authentifizierungstoken-Geräteflows.) |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp 40.126.9.7:443: connect: network is Forbidden`. |Der Zugriff auf den Endpunkt `login.windows.net` wird durch einen Proxy oder durch eine Firewall blockiert. | Überprüfen Sie die Konnektivität mit dem Endpunkt, und vergewissern Sie sich, dass sie nicht durch eine Firewall oder einen Proxyserver blockiert wird. |
|„Failed to acquire authorization token device flow“ (Fehler beim Abrufen des Authentifizierungstoken-Geräteflows.)  |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp lookup login.windows.net: no such host`. | Das Gruppenrichtlinienobjekt *Computerkonfiguration\Administrative Vorlagen\System\Benutzerprofile\Benutzerprofile, die älter als eine bestimmte Anzahl von Tagen sind, beim Systemneustart löschen* ist aktiviert. | Vergewissern Sie sich, dass das Gruppenrichtlinienobjekt aktiviert ist und den betroffenen Computer als Ziel hat. Weitere Detailinformationen finden Sie in Fußnote <sup>[1](#footnote1)</sup>. |
|„Failed to acquire authorization token from SPN“ (Fehler beim Abrufen des Authentifizierungstokens vom SPN.) |`Failed to execute the refresh request. Error = 'Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/token?api-version=1.0: Forbidden'` |Der Zugriff auf den Endpunkt `login.windows.net` wird durch einen Proxy oder durch eine Firewall blockiert. |Überprüfen Sie die Konnektivität mit dem Endpunkt, und vergewissern Sie sich, dass sie nicht durch eine Firewall oder einen Proxyserver blockiert wird. |
|„Failed to acquire authorization token from SPN“ (Fehler beim Abrufen des Authentifizierungstokens vom SPN.) |`Invalid client secret is provided` |Falsches oder ungültiges Dienstprinzipalgeheimnis. |Überprüfen Sie das Dienstprinzipalgeheimnis. |
| „Failed to acquire authorization token from SPN“ (Fehler beim Abrufen des Authentifizierungstokens vom SPN.) |`Application with identifier 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' was not found in the directory 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant` |Falscher Dienstprinzipal und/oder falsche Mandanten-ID. |Überprüfen Sie den Dienstprinzipal und/oder die Mandanten-ID.|
|„Get ARM Resource Response“ (ARM-Ressourcenantwort abrufen) |`The client 'username@domain.com' with object id 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' does not have authorization to perform action 'Microsoft.HybridCompute/machines/read' over scope '/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01' or the scope is invalid. If access was recently granted, please refresh your credentials."}}" Status Code=403` |Falsche Anmeldeinformationen und/oder Berechtigungen. |Vergewissern Sie sich, dass Sie der Rolle **Onboarding von Azure Connected Machine** angehören oder dass der Dienstprinzipal dieser Rolle angehört. |
|„Failed to AzcmagentConnect ARM resource“ (Fehler bei „AzcmagentConnect“ für die ARM-Ressource.) |`The subscription is not registered to use namespace 'Microsoft.HybridCompute'` |Azure-Ressourcenanbieter sind nicht registriert. |Registrieren Sie die [Ressourcenanbieter](./agent-overview.md#register-azure-resource-providers). |
|„Failed to AzcmagentConnect ARM resource“ (Fehler bei „AzcmagentConnect“ für die ARM-Ressource.) |`Get https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01?api-version=2019-03-18-preview:  Forbidden` |Der Zugriff auf den Endpunkt `management.azure.com` wird durch einen Proxyserver oder durch eine Firewall blockiert. |Überprüfen Sie die Konnektivität mit dem Endpunkt, und vergewissern Sie sich, dass sie nicht durch eine Firewall oder einen Proxyserver blockiert wird. |

<a name="footnote1"></a><sup>1</sup>Wenn dieses Gruppenrichtlinienobjekt aktiviert ist und für Computer mit dem Connected Machine-Agent gilt, löscht es das Benutzerprofil, das dem integrierten Konto zugeordnet ist, das für den *himds*-Dienst angegeben ist. Dadurch wird auch das Authentifizierungszertifikat gelöscht, das für die Kommunikation mit dem Dienst verwendet wird, das 30 Tage lang im lokalen Zertifikatspeicher zwischengespeichert wird. Vor Ablauf des 30-Tage-Limits wird versucht, das Zertifikat zu erneuern. Um dieses Problem zu beheben, führen Sie die Schritte zum [Aufheben der Registrierung des Computers](manage-agent.md#unregister-machine) aus, und anschließend registrieren Sie ihn erneut, während der `azcmagent connect`-Dienst ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem hier nicht aufgeführt wird, oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Erhalten Sie Antworten von Azure-Experten über [Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-arc.html).

* Stellen Sie eine Verbindung mit [@AzureSupport](https://twitter.com/azuresupport) her, dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Der Azure-Support verbindet die Azure-Community mit Antworten, Support und Experten.

* Erstellen Sie einen Azure-Supportfall. Wechseln Sie zur [Azure-Supportwebsite](https://azure.microsoft.com/support/options/), und wählen Sie **Support erhalten** aus.