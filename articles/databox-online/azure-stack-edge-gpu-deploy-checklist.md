---
title: Prüfliste für das Sammeln von Informationen vor der Bereitstellung eines Azure Stack Edge GPU-Geräts | Microsoft-Dokumentation
description: In diesem Artikel werden die Informationen beschrieben, die gesammelt werden können, bevor Sie Ihr Azure Stack Edge GPU-Gerät bereitstellen.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/31/2020
ms.author: alkohli
ms.openlocfilehash: 9d7b3388b7e3a31c23b34b21017f012d40e9f849
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89268141"
---
# <a name="deployment-checklist-for-your-azure-stack-edge-gpu-device"></a>Bereitstellungsprüfliste für Ihr Azure Stack Edge GPU-Gerät  

In diesem Artikel werden die Informationen beschrieben, die vor der tatsächlichen Bereitstellung Ihres Azure Stack Edge-Geräts gesammelt werden können. 

Stellen Sie mithilfe der folgenden Prüfliste sicher, dass Ihnen diese Informationen vorliegen, nachdem Sie eine Bestellung für ein Azure Stack Edge-Gerät aufgegeben haben und bevor Sie es erhalten. 

## <a name="deployment-checklist"></a>Bereitstellungsprüfliste 

| Phase                             | Parameter                                                                                                                                                                                                                           | Details                                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Geräteverwaltung               | <li>Microsoft Azure-Abonnement</li><li>Azure Active Directory Graph-API</li><li>Microsoft Azure Storage-Konto</li>|<li>Für Azure Stack Edge/Data Box Gateway aktiviert, Berechtigungen für Mitwirkende</li><li>Sicherstellen des Administrator- oder Benutzerzugriffs</li><li>Zugriffsanmeldeinformationen erforderlich</li> |
| Geräteinstallation               | Netzkabel im Paket. <br>Für die USA wird ein SVE 18/3-Kabel für 125 Volt und 15 Ampère mit einem Steckverbinder für NEMA 5-15P und C13 (Eingang zu Ausgang) mitgeliefert.                                                                                                                                                                                                          | Mit dem Gerät bereitgestellt.<br>Weitere Informationen finden Sie in der Liste [unterstützter Netzkabel nach Land](azure-stack-edge-technical-specifications-power-cords-regional.md).                                                                                        |
|                                   | <li>Mindestens ein 1-GbE-RJ-45-Netzwerkkabel für Port 1  </li><li> Mindestens ein 25-GbE-SFP+-Kupferkabel für Port 3, Port 4, Port 5 oder Port 6</li>| Diese Kabel muss der Kunde beschaffen.<br>Eine vollständige Liste der unterstützten Kabel, Switches und Transceiver für Netzwerkkarten von Geräten finden Sie unter [Interoperabilitätsmatrix für Cavium FastlinQ 41000 Series](https://www.marvell.com/documents/xalflardzafh32cfvi0z/) und [Mit Mellanox Dual Port 25G ConnectX-4-Kanal-Netzwerkadapter kompatible Produkte](https://docs.mellanox.com/display/ConnectX4LxFirmwarev14271016/Firmware+Compatible+Products).| 
| Erstmalige Geräteverbindung      | Port 1 hat für die erstmalige Verbindung eine feste IP-Adresse (192.168.100.10/24). <li>Laptop für Direktverbindung mit Port 1 mit der IP-Adresse 192.168.100.0/24 im Netzwerk erforderlich.</li><li> Verwenden Sie die lokale Benutzeroberfläche des Geräts an `https://192.168.100.10` für die weitere Konfiguration.</li><li> Nach der erstmaligen Einrichtung benötigt das Gerät mindestens einen GbE-Switch. Es besteht kein Zugriff auf die lokale Webbenutzeroberfläche, wenn nicht mindestens ein 1-GbE-Switch angeschlossen ist.</li>|                                                                                                                   |
| Anmeldung beim Gerät                      | Das Kennwort des Geräteadministrators muss 8 bis 16 Zeichen lang sein. <br>Es muss drei der folgenden Zeichen enthalten: Großbuchstaben, Kleinbuchstaben, Ziffern und Sonderzeichen.                                            | Das Standardkennwort lautet *Password1*, das nach der ersten Anmeldung abläuft.                                                     |
| Netzwerkeinstellungen                  | Das Gerät verfügt über zwei 1-GbE-und vier 25-GbE-Netzwerkports. <li>Port 1 wird nur zum Konfigurieren von Verwaltungseinstellungen verwendet. Mindestens ein Datenport kann angeschlossen und konfiguriert werden. </li><li> Mindestens eine Schnittstelle zum Datennetzwerk (PORT 2 bis PORT 6) muss mit dem Internet verbunden sein und über Azure-Konnektivität verfügen.</li><li> IP-Einstellungen unterstützen die DHCP-/statische IPv4-Konfiguration. | Die statische IPv4-Konfiguration erfordert das Angeben von IP-Adresse, DNS-Server und Standardgateway.                                                                                                                  |
| Compute-Netzwerkeinstellungen     | <li>Zwei statische öffentliche IP-Adressen für Kubernetes-Knoten und mindestens eine statische IP-Adresse für den Azure Stack Edge Hub-Dienst für den Zugriff auf Computemodule erforderlich.</li><li>Eine IP-Adresse für jeden zusätzlichen Dienst oder Container erforderlich, auf den von außerhalb des Kubernetes-Clusters zugegriffen werden muss.</li>                                                                                                                       | Nur die statische IPv4-Konfiguration wird unterstützt.                                                                      |
| (Optional) Webproxyeinstellungen     | <li>IP-Adresse/FQDN des Webproxyservers, Port: </li><li>Benutzername, Kennwort des Webproxys</li>                                                                                                                                                                                                    | Wird derzeit im Setup von Compute nicht unterstützt.                                                                     |
| Firewall- und Porteinstellungen        | Verwenden Sie die [aufgelisteten URL-Muster und -Ports](azure-stack-edge-system-requirements.md#networking-port-requirements), die für IP-Adressen von Geräten zulässig sind.                                                                                                                                                  |                                                                                                                   |
| (Optional) MAC-Adresse            | Wenn die MAC-Adresse in die Zulassungsliste aufgenommen werden muss, rufen Sie die Adresse des angeschlossenen Ports auf der lokalen Benutzeroberfläche des Geräts ab. |                                                                                                                   |
| (Optional) Port des Netzwerkswitches    | Gerät hostet Hyper-V-VMs für Compute. Einige Portkonfigurationen von Netzwerkswitches unterstützen diese Einrichtung nicht standardmäßig.                                                                                                        |                                                                                                                   |
| (Empfohlen) Zeiteinstellungen       | Konfigurieren Sie die Zeitzone sowie den primären und sekundären NTP-Server.                                                                                                                                                                    | Konfigurieren Sie im lokalen Netzwerk den primären und sekundären NTP-Server.<br>Wenn der lokale Server nicht verfügbar ist, können öffentliche NTP-Server konfiguriert werden.                                                    |
| (Optional) Einstellungen für Updateserver | <li>IP-Adresse des Updateservers im lokalen Netzwerk und Pfad zum WSUS-Server erforderlich. </li> | Standardmäßig wird der öffentliche Windows Update-Server verwendet.|
| Geräteeinstellungen                   | <li>In der DNS-Organisation registrierter Gerätename </li><li>DNS-Domäne</li> |                                                                                                                   |
| (Optional) Zertifikate                      | Die vom Gerät generierten Zertifikate verwenden <br><br> Wenn Sie eigene Zertifikate bereitstellen, benötigen Sie Folgendes: <li>Vertrauenswürdiges Stammsignaturzertifikat im DER-Format mit *.cer* </li><li>Endpunktzertifikate in *PFX-* Format</li>|Endpunktzertifikate sind für Azure Resource Manager, Blob Storage und lokale Webbenutzeroberfläche inbegriffen.                                                                                                                   |
| Aktivierung                        | Aktivierungsschlüssel von Azure Stack Edge-/Data Box Gateway-Ressource erforderlich.                                                                                                                                                       | Nach der Generierung läuft der Schlüssel nach 3 Tagen ab.                                                                        |


## <a name="next-steps"></a>Nächste Schritte

Bereiten Sie die Bereitstellung Ihres [Azure Stack Edge-Geräts](azure-stack-edge-gpu-deploy-prep.md) vor.



