---
title: Azure Stack Edge-Sicherheit | Microsoft-Dokumentation
description: Hier werden die Sicherheits- und Datenschutzfeatures beschrieben, die Ihr Azure Stack Edge-Gerät, Ihren Dienst und Ihre Daten sowohl lokal als auch in der Cloud schützen.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 08/21/2019
ms.author: alkohli
ms.openlocfilehash: 3d567ec4d760be24fdbb79ff85bd6db0eb4a66c8
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82568839"
---
# <a name="azure-stack-edge-security-and-data-protection"></a>Azure Stack Edge-Sicherheit und -Datenschutz

Sicherheit ist bei der Einführung neuer Technologien immer ein zentrales Anliegen. Das gilt insbesondere, wenn die Technologie mit vertraulichen oder proprietären Daten verwendet wird. Azure Stack Edge trägt dazu bei, dass Ihre Daten nur von autorisierten Entitäten angezeigt, geändert oder gelöscht werden können.

In diesem Artikel werden die Sicherheitsfeatures von Azure Stack Edge beschrieben, mit denen die einzelnen Komponenten der Lösung und die darin gespeicherten Daten geschützt werden.

Azure Stack Edge besteht aus vier Hauptkomponenten, die miteinander interagieren:

- **In Azure gehosteter Azure Stack Edge-Dienst**. Die Verwaltungsressource zum Erstellen des Geräteauftrags, zum Konfigurieren des Geräts sowie zum anschließenden Nachverfolgen des Auftrags bis zur Erfüllung
- **Azure Stack Edge-Gerät**. Das Übertragungsgerät, das an Sie gesendet wird, um Ihre lokalen Daten in Azure zu importieren
- **Mit dem Gerät verbundene Clients/Hosts:** Die Clients in Ihrer Infrastruktur, die mit dem Azure Stack Edge-Gerät verbunden werden und zu schützende Daten enthalten.
- **Cloudspeicher:** Der Speicherort auf der Azure-Cloudplattform, an dem die Daten gespeichert werden. Hierbei handelt es sich in der Regel um das Speicherkonto, das mit der von Ihnen erstellten Azure Stack Edge-Ressource verknüpft ist.

## <a name="azure-stack-edge-service-protection"></a>Schutz des Azure Stack Edge-Diensts

Der Azure Stack Edge-Dienst ist ein in Azure gehosteter Verwaltungsdienst. Der Dienst wird zum Konfigurieren und Verwalten des Geräts verwendet.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="azure-stack-edge-device-protection"></a>Schutz des Azure Stack Edge-Geräts

Das Azure Stack Edge-Gerät ist ein lokales Gerät, das Sie bei der Transformation Ihrer Daten unterstützt. Hierzu werden die Daten lokal verarbeitet und dann an Azure gesendet. Für Ihr Gerät gilt Folgendes:

- Es benötigt einen Aktivierungsschlüssel für den Zugriff auf den Azure Stack Edge-Dienst.
- Es ist jederzeit durch ein Gerätekennwort geschützt.
- Es ist ein gesperrtes Gerät. BMC und BIOS des Geräts sind kennwortgeschützt. Das BIOS ist durch eingeschränkten Benutzerzugriff geschützt.
- Sicherer Start ist aktiviert.
- Windows Defender Device Guard wird darauf ausgeführt. Device Guard stellt sicher, dass nur vertrauenswürdige Anwendungen ausgeführt werden können, die Sie in Ihren Codeintegritätsrichtlinien definieren.

### <a name="protect-the-device-via-activation-key"></a>Schützen des Geräts mittels Aktivierungsschlüssel

Dem Azure Stack Edge-Dienst, den Sie in Ihrem Azure-Abonnement erstellt haben, können nur autorisierte Azure Stack Edge-Geräte hinzugefügt werden. Um ein Gerät zu autorisieren, müssen Sie es mithilfe eines Aktivierungsschlüssels für den Azure Stack Edge-Dienst aktivieren.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Weitere Informationen finden Sie unter [Abrufen des Aktivierungsschlüssels](azure-stack-edge-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Schützen des Geräts mittels Kennwort

Mit Kennwörtern wird sichergestellt, dass nur autorisierte Benutzer Zugriff auf Ihre Daten haben. Azure Stack Edge-Geräte sind nach dem Start gesperrt.

Ihre Möglichkeiten:

- Stellen Sie über einen Browser eine Verbindung mit der lokalen Webbenutzeroberfläche des Geräts her, und geben Sie ein Kennwort an, um sich bei dem Gerät anzumelden.
- Stellen Sie eine HTTP-Remoteverbindung mit der PowerShell-Schnittstelle des Geräts her. Die Remoteverwaltung ist standardmäßig aktiviert. Anschließend können Sie das Gerätekennwort angeben, um sich bei dem Gerät anzumelden. Weitere Informationen finden Sie unter [Herstellen einer Remoteverbindung mit Ihrem Azure Stack Edge-Gerät](azure-stack-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Verwenden Sie zum [Ändern des Kennworts](azure-stack-edge-manage-access-power-connectivity-mode.md#manage-device-access) die lokale Webbenutzeroberfläche. Sollten Sie das Kennwort ändern, benachrichtigen Sie alle Benutzer mit Remotezugriff, damit bei ihnen keine Anmeldefehler auftreten.

## <a name="protect-your-data"></a>Schützen Ihrer Daten

In diesem Abschnitt werden die Azure Stack Edge-Sicherheitsfeatures für Daten während der Übertragung sowie für gespeicherte Daten beschrieben.

### <a name="protect-data-at-rest"></a>Schutz gespeicherter Daten

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]
- BitLocker XTS-AES-256-Bit-Verschlüsselung wird verwendet, um lokale Daten zu schützen.


### <a name="protect-data-in-flight"></a>Schützen von Daten während der Übertragung

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Schützen von Daten über Speicherkonten

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Rotieren und [synchronisieren Sie Ihre Speicherkontoschlüssel](azure-stack-edge-manage-shares.md#sync-storage-keys) regelmäßig, um zu vermeiden, dass nicht autorisierte Benutzer auf Ihr Speicherkonto zugreifen können.

## <a name="manage-personal-information"></a>Verwalten persönlicher Informationen

Der Azure Stack Edge-Dienst erfasst persönliche Informationen in folgenden Szenarien:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Eine Anleitung zum Anzeigen der Liste mit den Benutzern, die auf eine Freigabe zugreifen oder sie löschen können, finden Sie unter [Verwalten von Freigaben auf der Azure Stack Edge-Ressource](azure-stack-edge-manage-shares.md).

Weitere Informationen finden Sie im [Trust Center in der Microsoft-Datenschutzrichtlinie](https://www.microsoft.com/trustcenter).

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen Ihres Azure Stack Edge-Geräts](azure-stack-edge-deploy-prep.md)
