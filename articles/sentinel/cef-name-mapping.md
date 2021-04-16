---
title: Zuordnung von CEF-Schlüsseln (Common Event Format) zu CommonSecurityLog-Feldnamen
description: In diesem Artikel werden CEF-Schlüssel den entsprechenden Feldnamen im CommonSecurityLog in Azure Sentinel zugeordnet.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: reference
ms.date: 03/16/2021
ms.openlocfilehash: 6c23fe86af030d371e12914062bb9558e8db3484
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2021
ms.locfileid: "104776052"
---
# <a name="cef-and-commonsecuritylog-field-mapping"></a>CEF- und CommonSecurityLog-Feldzuordnung

In den folgenden Tabellen werden CEF-Feldnamen (Common Event Format) den Namen zugeordnet, die in CommonSecurityLog in Azure Sentinel verwendet werden. Sie sind möglicherweise hilfreich, wenn Sie in Azure Sentinel mit einer CEF-Datenquelle arbeiten.

Weitere Informationen finden Sie unter [Verbinden der externen Lösung mithilfe von Common Event Format](connect-common-event-format.md).

## <a name="a---c"></a>A–C

|CEF-Schlüsselname  |CommonSecurityLog-Feldname  |BESCHREIBUNG  |
|---------|---------|---------|
| act    |    <a name="deviceaction"></a> DeviceAction     |  Die im Ereignis angegebene Aktion.       |
|   app  |    ApplicationProtocol     |  Das in der Anwendung verwendete Protokoll wie HTTP, HTTPS, SSHv2, Telnet, POP, IMPA, IMAPS usw.   |
| cnt    |    EventCount     |  Ein dem Ereignis zugeordneter Zähler, der anzeigt, wie oft das gleiche Ereignis beobachtet wurde.       |
| | | |

## <a name="d"></a>D

|CEF-Schlüsselname  |CommonSecurityLog-Name  |BESCHREIBUNG  |
|---------|---------|---------|
|Gerätehersteller     |  DeviceVendor       | Eine Zeichenfolge, die in Verbindung mit den Geräteprodukt- und Versionsdefinitionen den Typ des sendenden Geräts eindeutig identifiziert.       |
|Geräteprodukt     |   DeviceProduct      |   Eine Zeichenfolge, die in Verbindung mit den Gerätehersteller- und Versionsdefinitionen den Typ des sendenden Geräts eindeutig identifiziert.        |
|Geräteversion     |   DeviceVersion      |      Eine Zeichenfolge, die in Verbindung mit den Geräteprodukt-und Herstellerdefinitionen den Typ des sendenden Geräts eindeutig identifiziert.     |
|DeviceEventClassID     |   DeviceEventClassID     |   Eine Zeichenfolge oder ganze Zahl, die als eindeutiger Bezeichner pro Ereignistyp fungiert.      |
| destinationDnsDomain    | DestinationDnsDomain        |   Der DNS-Teil des vollqualifizierten Domänennamens (FQDN).      |
| destinationServiceName | DestinationServiceName | Der Dienst, auf den das Ereignis ausgerichtet ist. Beispiel: `sshd`.|
| destinationTranslatedAddress | DestinationTranslatedAddress | Identifiziert das übersetzte Ziel, auf das das Ereignis in einem IP-Netzwerk verweist, als IPv4-IP-Adresse. |
| destinationTranslatedPort | DestinationTranslatedPort | Port nach der Übersetzung, z. B. als Firewall. <br>Gültige Portnummern: `0` - `65535` |
| deviceDirection | <a name="communicationdirection"></a> CommunicationDirection | Alle Informationen zur Richtung der überwachten Kommunikation. Gültige Werte: <br>- `0` = Eingehend <br>- `1` = Ausgehend |
| deviceDnsDomain | DeviceDnsDomain | Der DNS-Domänenteil des vollqualifizierten Domänennamens (FQDN) |
| deviceExternalID | DeviceExternalID | Ein Name, der das Gerät eindeutig identifiziert, das das Ereignis erzeugt. |
| deviceFacility | DeviceFacility | Die Facility, die das Ereignis erzeugt.|
| deviceInboundInterface | DeviceInboundInterface |Die Schnittstelle für den Eingang des Pakets oder der Daten in das Gerät.  |
| deviceNtDomain | DeviceNtDomain | Die Windows-Domäne der Geräteadresse |
| deviceOutboundInterface | DeviceOutboundInterface |Die Schnittstelle für den Ausgang des Pakets oder der Daten aus dem Gerät. |
| devicePayloadId |DevicePayloadId |Eindeutiger Bezeichner für die mit dem Ereignis verknüpften Nutzdaten. |
| deviceProcessName | ProcessName | Dem Ereignis zugeordneter Prozessname. <br><br>In UNIX beispielsweise der Prozess, der den Syslog-Eintrag erzeugt. |
| deviceTranslatedAddress | DeviceTranslatedAddress | Identifiziert die übersetzte Geräteadresse, auf die sich das Ereignis bezieht, in einem IP-Netzwerk. <br><br>Das Format ist eine IPv4-Adresse. |
| dhost |DestinationHostName | Das Ziel, auf das sich das Ereignis in einem IP-Netzwerk bezieht.  <br>Das Format muss ein FQDN sein, der dem Zielknoten zugeordnet ist, wenn ein Knoten verfügbar ist. Zum Beispiel: `host.domain.com` oder `host`. |
| dmac | DestinationMacAddress | Die MAC-Zieladresse (FQDN) |
| dntdom | DestinationNTDomain | Der Windows-Domänenname der Zieladresse.|
| dpid | DestinationProcessId |Die ID des Zielprozesses, der dem Ereignis zugeordnet ist.|
| dpriv | DestinationUserPrivileges | Definiert die Berechtigungen der Zielverwendung. <br>Gültige Werte: `Admninistrator`, `User`, `Guest` |
| dproc | DestinationProcessName | Der Name des Zielprozesses des Ereignisses, z. B. `telnetd` oder `sshd.` |
| dpt | DestinationPort | Zielport. <br>Gültige Werte: `*0` - `65535` |
| dst | DestinationIP | Die IpV4-Zieladresse, auf die sich das Ereignis in einem IP-Netzwerk bezieht. |
| dtz | DeviceTimeZon | Zeitzone des Geräts, das das Ereignis erzeugt |
| duid |DestinationUserId | Identifiziert den Zielbenutzer nach ID. |
| duser | DestinationUserName |Identifiziert den Zielbenutzer nach Namen.|
| dvc | DeviceAddress | Die IPv4-Adresse des Geräts, das das Ereignis erzeugt. |
| dvchost | DeviceName | Der dem Geräteknoten zugeordnete FQDN, wenn ein Knoten verfügbar ist. Zum Beispiel: `host.domain.com` oder `host`.| 
| dvcmac | DeviceMacAddress | Die MAC-Adresse des Geräts, das das Ereignis erzeugt. |
| dvcpid | Prozess-ID | Definiert die ID des Prozesses auf dem Gerät, das das Ereignis erzeugt. |

## <a name="e---i"></a>E - I

|CEF-Schlüsselname  |CommonSecurityLog-Name  |BESCHREIBUNG  |
|---------|---------|---------|
|end     |  EndTime       | Der Zeitpunkt, zu dem die mit dem Ereignis verbundene Aktivität endete.        |
|externalId    |   ExternalID      | Eine ID, die vom Ursprungsgerät verwendet wird. In der Regel handelt es sich um steigende Werte, die jeweils einem Ereignis zugeordnet sind.        |
|fileCreateTime     |  FileCreateTime      | Zeitpunkt, zu dem die Datei erstellt wurde.        |
|fileHash     |   Dateihash      |   Hash einer Datei.      |
|fileId     |   FileID      |Eine ID, die einer Datei zugeordnet ist, z. B. INode.         |
| fileModificationTime | FileModificationTime |Zeitpunkt der letzten Dateiänderung. |
| filePath | FilePath | Vollständiger Pfad der Datei einschließlich des Dateinamens. Zum Beispiel: `C:\ProgramFiles\WindowsNT\Accessories\wordpad.exe` oder `/usr/bin/zip`.|
| filePermission |FilePermission |Die Berechtigungen der Datei. |
| fileType | FileType | Dateityp wie Pipe, Socket usw.|
|fname | FileName| Der Dateiname ohne Pfad. |
| fsize | FileSize | Die Größe der Datei. |
|Host    |  Computer       | Host, von Syslog        |
|in     |  ReceivedBytes      |Anzahl der eingehend übertragenen Bytes.         |
| | | |

## <a name="m---p"></a>M - P

|CEF-Schlüsselname  |CommonSecurityLog-Name  |BESCHREIBUNG  |
|---------|---------|---------|
|mldg   |  Meldung       | Eine Meldung, die weitere Details zum Ereignis enthält.        |
|Name     |  Aktivität       |   Eine Zeichenfolge, die eine lesbare und verständliche Beschreibung des Ereignisses darstellt.     |
|oldFileCreateTime     |  OldFileCreateTime       | Zeitpunkt, zu dem die alte Datei erstellt wurde.        |
|oldFileHash     |   OldFileHash      |   Hash der alten Datei.      |
|oldFileId     |   OldFileId     |   Eine ID, die der alten Datei zugeordnet ist, z. B. INode.      |
| oldFileModificationTime | OldFileModificationTime |Zeitpunkt der letzten Änderung der alten Datei. |
| oldFileName |  OldFileName |Name der alten Datei. |
| oldFilePath | OldFilePath | Vollständiger Pfad der alten Datei einschließlich des Dateinamens. <br>Zum Beispiel: `C:\ProgramFiles\WindowsNT\Accessories\wordpad.exe` oder `/usr/bin/zip`.|
| oldFilePermission | OldFilePermission |Berechtigungen der alten Datei. |
|oldFileSize | OldFileSize | Größe der alten Datei.|
| oldFileType | OldFileType | Dateityp der alten Datei wie Pipe, Socket usw.|
| out | SentBytes | Anzahl der ausgehend übertragenen Bytes. |
| Ergebnis | Ergebnis | Ergebnis des Ereignisses wie `success` oder `failure`.|
|proto    |  Protocol       | Transportprotokoll, das das verwendete Layer-4-Protokoll identifiziert. <br><br>Mögliche Werte sind Protokollnamen wie `TCP` oder `UDP`.        |
| | | |

## <a name="r---t"></a>R - T

|CEF-Schlüsselname  |CommonSecurityLog-Name  |Beschreibung  |
|---------|---------|---------|
|Grund     |  `Reason`      |Der Grund für die Generierung eines Überwachungsereignisses. <br><br>Zum Beispiel: `Bad password` oder `Unknown user`.         |
|Anforderung     |   RequestURL      | Die URL, auf die für eine HTTP-Anforderung zugegriffen wird, einschließlich des Protokolls. Zum Beispiel, `http://www/secure.com`        |
|requestClientApplication     |   RequestClientApplication      |   Der Benutzer-Agent, der der Anforderung zugeordnet ist.      |
| requestContext | RequestContext | Beschreibt den Inhalt, von dem die Anforderung stammt, z. B. den HTTP-Referrer. |
| requestCookies | RequestCookies |Der Anforderung zugeordnete Cookies. |
| requestMethod | RequestMethod | Die zum Zugreifen auf eine URL verwendete Methode. <br><br>Gültige Werte sind Methoden wie `POST`, `GET` usw. |
| rt | ReceiptTime | Der Zeitpunkt, zu dem das mit der Aktivität verbundene Ereignis empfangen wurde. |
|severity     |  <a name="logseverity"></a> LogSeverity       |  Eine Zeichenfolge oder ganze Zahl, die die Wichtigkeit des Ereignisses beschreibt.<br><br> Gültige Zeichenfolgenwerte: `Unknown`, `Low`, `Medium`, `High`, `Very-High` <br><br>Gültige ganzzahlige Werte sind: `0`-`3` = Niedrig, `4`-`6` = Mittel, `7`-`8` = Hoch, `9`-`10` = Sehr hoch |
| shost    | SourceHostName        |Identifiziert die Quelle, auf die sich das Ereignis in einem IP-Netzwerk bezieht. Das Format muss ein vollqualifizierter Domänenname (DQDN) sein, der dem Quellknoten zugeordnet ist, wenn ein Knoten verfügbar ist. Zum Beispiel: `host` oder `host.domain.com`. |
| smac | SourceMacAddress | MAC-Adresse der Quelle. |
| sntdom | SourceNTDomain | Der Windows-Domänenname für die Quelladresse. |
| sourceDnsDomain | SourceDnsDomain | Der DNS-Domänenteil des vollständigen FQDN. |
| sourceServiceName | SourceServiceName | Der Dienst, der für das Generieren des Ereignisses verantwortlich ist. |
| sourceTranslatedAddress | SourceTranslatedAddress | Identifiziert die übersetzte Quelle, auf die sich das Ereignis bezieht, in einem IP-Netzwerk. |
| sourceTranslatedPort | SourceTranslatedPort | Quellport nach der Übersetzung, z. B. als Firewall. <br>Gültige Portnummern sind `0` - `65535`. |
| spid | SourceProcessId | Die ID des Quellprozesses, der dem Ereignis zugeordnet ist.|
| spriv | SourceUserPrivileges | Die Berechtigungen des Quellbenutzers. <br><br>Gültige Werte sind `Administrator`, `User`, `Guest`. |
| sproc | SourceProcessName | Der Name des Quellprozesses des Ereignisses.|
| spt | SourcePort | Die Quellportnummer. <br>Gültige Portnummern sind `0` - `65535`. |
| src | SourceIP |Die Quelle, auf die ein Ereignis in einem IP-Netzwerk verweist, als IPv4-Adresse. |
| start | StartTime | Der Zeitpunkt, zu dem die Aktivität, auf die das Ereignis verweist, gestartet wurde. |
| suid | SourceUserID | Identifiziert den Quellbenutzer anhand der ID. |
| Typ | EventType | Der Ereignistyp. Hierzu gehören folgende Werte: <br>- `0`: Basisereignis <br>- `1`: aggregiert <br>- `2`: Korrelationsereignis <br>- `3`: Aktionsereignis <br><br>**Hinweis**: Dieses Ereignis kann bei Basisereignissen ausgelassen werden. |
| | | |

## <a name="unmapped-fields"></a>Nicht zugeordnete Felder

Die folgenden **CommonSecurityLog**-Feldnamen haben keine Zuordnungen in CEF-Schlüsseln:


|CommonSecurityLog-Feldname  |Beschreibung  |
|---------|---------|
|**OriginalLogSeverity**     |  Immer leer, wird für die Integration in CiscoASA unterstützt. <br>Ausführliche Informationen zu den Werten für den Protokollschweregrad siehe Feld [LogSeverity](#logseverity).       |
|**RemoteIP**     |     Die Remote-IP-Adresse. <br>Dieser Wert basiert auf dem Feld [CommunicationDirection](#communicationdirection), wenn möglich.     |
|**RemotePort**     |   Der Remoteport. <br>Dieser Wert basiert auf dem Feld [CommunicationDirection](#communicationdirection), wenn möglich.      |
|**SimplifiedDeviceAction**     |   Vereinfacht den [DeviceAction](#deviceaction)-Wert in einen statischen Satz von Werten, während der ursprüngliche Wert im Feld [DeviceAction](#deviceaction) beibehalten wird. <br>Beispiel: `Denied` > `Deny`.      |
|     |         |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Verbinden der externen Lösung mithilfe von Common Event Format](connect-common-event-format.md).
