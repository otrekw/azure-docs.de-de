---
title: Behandeln von Verbindungsproblemen mit dem Agent für Azure Arc-fähige Server
description: In diesem Artikel erfahren Sie, wie Sie Probleme mit dem Connected Machine-Agent behandeln und beheben, die im Zusammenhang mit Azure Arc-fähigen Servern bei dem Versuch auftreten, eine Verbindung mit dem Dienst herzustellen.
ms.date: 04/12/2021
ms.topic: conceptual
ms.openlocfilehash: ae26b599a72129b5ed7f47d76d10353be5c0e8ac
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107497998"
---
# <a name="troubleshoot-azure-arc-enabled-servers-agent-connection-issues"></a>Behandeln von Verbindungsproblemen mit dem Agent für Azure Arc-fähige Server

Dieser Artikel enthält Informationen zur Behandlung und Behebung von Problemen, die auftreten können, wenn versucht wird, den Connected Machine-Agent für Azure Arc-fähige Server für Windows oder Linux zu konfigurieren. Hier wird sowohl die interaktive als auch die skalierbare Installationsmethode beim Konfigurieren der Verbindung mit dem Dienst behandelt. Allgemeine Informationen finden Sie in der [Übersicht über Arc-fähige Server](./overview.md).

## <a name="agent-error-codes"></a>Agent-Fehlercodes

Wenn Sie beim Konfigurieren des Agents für Azure Arc-fähige Server eine Fehlermeldung erhalten, können Sie in der folgenden Tabelle die wahrscheinliche Ursache und die vorgeschlagenen Schritte zum Beheben des Problems ermitteln. Sie benötigen den Fehlercode `AZCM0000` („0000“ kann eine beliebige vierstellige Zahl sein), der in der Konsole oder Skriptausgabe ausgegeben wird, um fortzufahren.

| Fehlercode | Wahrscheinliche Ursache | Vorschlag zur Problemlösung |
|------------|----------------|-----------------------|
| AZCM0000 | Die Aktion war erfolgreich | – |
| AZCM0001 | Unbekannter Fehler. | Wenden Sie sich an den Microsoft-Support |
| AZCM0011 | Der Benutzer hat die Aktion abgebrochen (STRG+C) | Den vorherigem Befehl nochmals ausführen |
| AZCM0012 | Das bereitgestellte Zugriffstoken ist ungültig | Abrufen eines neuen Zugriffstokens und Wiederholen des Vorgangs |
| AZCM0013 | Die bereitgestellten Tags sind ungültig | Überprüfen Sie, ob die Tags durch Kommas getrennt in doppelte Anführungszeichen eingeschlossen sind und dass alle Namen oder Werte mit Leerzeichen in einfache Anführungszeichen eingeschlossen sind: `--tags "SingleName='Value with spaces',Location=Redmond"`
| AZCM0014 | Die Cloud ist ungültig | Geben Sie eine unterstützte Cloud an: `AzureCloud` oder `AzureUSGovernment` |
| AZCM0015 | Die angegebene Korrelations-ID ist keine gültige GUID | Geben Sie eine gültige GUID für `--correlation-id` an |
| AZCM0016 | Dies ist ein erforderlicher Parameter | Überprüfen Sie die Ausgabe, um zu ermitteln, welche Parameter fehlen |
| AZCM0017 | Der Ressourcenname ist ungültig | Geben Sie einen Namen an, der nur alphanumerische Zeichen, Bindestriche und/oder Unterstriche verwendet. Der Name darf nicht mit einem Bindestrich oder Unterstrich enden. |
| AZCM0018 | Der Befehl wurde ohne Administratorberechtigung ausgeführt | Wiederholen Sie den Befehl mit Administrator- oder Stammberechtigungen in einer Eingabeaufforderung oder Konsolensitzung mit erhöhten Rechten. |
| AZCM0041 | Die angegebenen Anmeldeinformationen sind ungültig | Überprüfen Sie bei Geräteanmeldungen, ob das angegebene Benutzerkonto Zugriff auf den Mandanten und das Abonnement hat, in dem die Serverressource erstellt wird. Überprüfen Sie bei Dienstprinzipal-Anmeldungen die Client-ID und das Geheimnis auf Richtigkeit, das Ablaufdatum des Geheimnisses und dass der Dienstprinzipal aus demselben Mandanten stammt, in dem die Serverressource erstellt wird. |
| AZCM0042 | Fehler beim Erstellen der Arc-fähigen Serverressource | Vergewissern Sie sich, dass der angegebene Benutzer/Dienstprinzipal Zugriff zum Erstellen von Arc-fähigen Serverressourcen in der angegebenen Ressourcengruppe hat. |
| AZCM0043 | Fehler beim Löschen der Arc-fähigen Serverressource | Vergewissern Sie sich, dass der angegebene Benutzer/Dienstprinzipal Zugriff zum Erstellen von Arc-fähigen Serverressourcen in der angegebenen Ressourcengruppe hat. Wenn die Ressource in Azure nicht mehr vorhanden ist, verwenden Sie das `--force-local-only`-Flag , um fortzufahren. |
| AZCM0044 | Es ist bereits eine Ressource mit diesem Namen vorhanden | Geben Sie einen anderen Namen für den `--resource-name`-Parameter an oder löschen Sie den vorhandenen Arc-fähigen Server in Azure und versuchen Sie es erneut. |
| AZCM0061 | Der Agent-Dienst kann nicht erreicht werden | Stellen Sie sicher, dass Sie den Befehl in einem Benutzerkontext mit erhöhten Rechten (Administrator/Stamm) ausführen und dass der HIMDS-Dienst auf Ihrem Server ausgeführt wird. |
| AZCM0062 | Fehler beim Verbinden mit dem Server | Überprüfen Sie andere Fehlercodes in der Ausgabe, um spezifischere Informationen zu erhalten. Wenn der Fehler nach dem Erstellen der Azure-Ressource aufgetreten ist, müssen Sie den Arc-Server aus Ihrer Ressourcengruppe löschen, bevor Sie den Vorgang wiederholen. |
| AZCM0063 | Fehler beim Trennen der Verbindung mit dem Server | Überprüfen Sie andere Fehlercodes in der Ausgabe, um spezifischere Informationen zu erhalten. Wenn dieser Fehler weiterhin auftritt, können Sie die Ressource in Azure löschen und dann `azcmagent disconnect --force-local-only` auf dem Server ausführen, um die Verbindung mit dem Agent zu trennen. |
| AZCM0064 | Der Agent-Dienst antwortet nicht | Überprüfen Sie den Stauts des `himds`-Service, um sicherzugehen, dass er ausgeführt wird. Starten Sie den Dienst, wenn er nicht ausgeführt wird. Wenn er ausgeführt wird, warten Sie eine Minute, und versuchen Sie es dann erneut. |
| AZCM0065 | Ein interner Agent-Kommunikationsfehler ist aufgetreten | Wenden Sie sich an den Microsoft-Support |
| AZCM0066 | Der Agent-Webdienst reagiert nicht oder ist nicht verfügbar | Wenden Sie sich an den Microsoft-Support |
| AZCM0067 | Der Agent ist bereits mit Azure verbunden | Führen Sie zuerst die Schritte unter [Trennen der Verbindung des Agents](manage-agent.md#unregister-machine) aus und versuchen Sie es dann erneut. |
| AZCM0068 | Interner Fehler beim Trennen des Servers von Azure | Wenden Sie sich an den Microsoft-Support |
| AZCM0081 | Fehler beim Herunterladen des Zertifikats der Azure Active Directory verwalteten Identität | Wenn diese Meldung beim Versuch auftritt, den Server mit Azure zu verbinden, kann der Agent nicht mit dem Azure Arc-Dienst kommunizieren. Löschen Sie die Ressource in Azure und versuchen Sie erneut, eine Verbindung herzustellen. |
| AZCM0101 | Der Befehl wurde nicht erfolgreich analysiert | Führen Sie `azcmagent <command> --help` aus, um die richtige Befehlssyntax zu überprüfen |
| AZCM0102 | Der Hostname des Computers kann nicht abgerufen werden | Führen Sie `hostname` aus, um nach Fehlermeldungen auf Systemebene zu suchen, und wenden Sie sich dann an Microsoft-Support. |
| AZCM0103 | Fehler beim Generieren des RSA-Schlüssels | Wenden Sie sich an den Microsoft-Support |
| AZCM0104 | Fehler beim Lesen von Systeminformationen | Überprüfen Sie, ob die für die Ausführung von `azcmagent` verwendete Identität über Administrator-/Stammberechtigungen auf dem System verfügt und wiederholen Sie den Vorgang. |

## <a name="agent-verbose-log"></a>Ausführliches Agent-Protokoll

Für die weiter unten in diesem Artikel beschriebenen Schritte benötigen Sie mindestens das ausführliche Protokoll. Es enthält die Ausgabe der Befehle des Tools **azcmagent**, wenn das Argument „verbose“ (-v) verwendet wird. Die Protokolldateien werden in `%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log` (Windows) bzw. in `/var/opt/azcmagent/log/azcmagent.log` (Linux) geschrieben.

### <a name="windows"></a>Windows

Hier sehen Sie ein Beispiel für den Befehl zum Aktivieren der ausführlichen Protokollierung mit dem Connected Machine-Agent für Windows bei einer interaktiven Installation:

```console
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Hier sehen Sie ein Beispiel für den Befehl zum Aktivieren der ausführlichen Protokollierung mit dem Connected Machine-Agent für Windows bei einer skalierbaren Installation unter Verwendung eines Dienstprinzipals:

```console
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
>Sie müssen auf Linux-Computern über *Stamm* zugriffsberechtigungen verfügen, um **azcmagent** ausführen zu können.

```bash
azcmagent connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

Hier sehen Sie ein Beispiel für den Befehl zum Aktivieren der ausführlichen Protokollierung mit dem Connected Machine-Agent für Linux bei einer skalierbaren Installation unter Verwendung eines Dienstprinzipals:

```bash
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
