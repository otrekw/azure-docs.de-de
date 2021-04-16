---
title: Wechseln/Umschalten zwischen zwei Azure-Clouddiensten (erweiterter Support)
description: Wechseln/Umschalten zwischen zwei Azure-Clouddiensten (erweiterter Support)
ms.topic: how-to
ms.service: cloud-services-extended-support
author: surbhijain
ms.author: surbhijain
ms.reviewer: gachandw
ms.date: 04/01/2021
ms.custom: ''
ms.openlocfilehash: 6f96656af9afd9874cc6273a9cea9ed43e8c69cc
ms.sourcegitcommit: af6eba1485e6fd99eed39e507896472fa930df4d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2021
ms.locfileid: "106294231"
---
# <a name="swapswitch-between-two-azure-cloud-services-extended-support"></a>Wechseln/Umschalten zwischen zwei Azure-Clouddiensten (erweiterter Support)
Mit Clouddiensten (erweiterter Support) können Sie zwischen zwei unabhängigen Clouddienstbereitstellungen wechseln. Anders als bei Clouddiensten (klassisch) ist das Konzept von Slots beim Azure Resource Manager-Modell nicht vorhanden. Wenn Sie sich für die Bereitstellung eines neuen Release eines Clouddiensts (erweiterter Support) entscheiden, können Sie ihn mit einem anderen vorhandenen Clouddienst (erweiterter Support) austauschbar machen, sodass Sie Ihr neues Release mithilfe dieser Bereitstellung stagen und testen können. Ein Clouddienst kann nur zum Zeitpunkt der Bereitstellung des zweiten Clouddiensts (des Paars) mit einem anderen Clouddienst austauschbar gemacht werden. Bei Verwendung der ARM-vorlagenbasierten Bereitstellungsmethode wird hierzu die Eigenschaft „SwappableCloudService“ im Netzwerkprofil des Clouddienstobjekts auf die ID des anderen Clouddiensts festgelegt. 

```
"networkProfile": {
 "SwappableCloudService": {
              "id": "[concat(variables('swappableResourcePrefix'), 'Microsoft.Compute/cloudServices/', parameters('cloudServicesToBeSwappedWith'))]"
            },
```
> [!Note] 
> Sie können nicht zwischen einem klassischen Clouddienst und einem Clouddienst mit erweitertem Support wechseln.

Verwenden Sie **Austausch**, um die URLs auszutauschen, mit denen die beiden Clouddienste adressiert werden, und dadurch einen neuen (gestageten) Clouddienst zum Produktionsrelease höher zu stufen.
Sie können Bereitstellungen über die Seite Cloud-Dienste oder über das Dashboard austauschen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)den Clouddienst aus, den Sie aktualisieren möchten. Dieser Schritt öffnet das Blatt für die Clouddienstinstanz.
2. Wählen Sie auf dem Blatt die Option **Austausch** aus.
   :::image type="content" source="media/swap-cloud-service-1.png" alt-text="Abbildung: Austauschoption für den Clouddienst":::
   
3. Die folgende Bestätigungsaufforderung wird geöffnet:
   
   :::image type="content" source="media/swap-cloud-service-2.png" alt-text="Abbildung: Austauschen des Clouddiensts":::
   
4. Klicken Sie nach dem Überprüfen der Bereitstellungsinformationen auf OK, um die Bereitstellungen auszutauschen.
Der Austausch erfolgt schnell, da sich nur die virtuellen IP-Adressen (VIPs) für die beiden Clouddienste ändern.

Um Computekosten zu sparen, können Sie einen der (als Stagingumgebung für die Anwendungsbereitstellung festgelegten) Clouddienste löschen, nachdem Sie sich vergewissert haben, dass Ihr ausgetauschter Clouddienst erwartungsgemäß funktioniert.

Im Anschluss sehen Sie die REST-API zum Durchführen eines Austauschs zweier Clouddienstbereitstellungen mit erweitertem Support:
```http
POST https://management.azure.com/subscriptions/subId/providers/Microsoft.Network/locations/region/setLoadBalancerFrontendPublicIpAddresses?api-version=2020-11-01
```
```
{
  "frontendIPConfigurations": [
    {
    "id": "#LBFE1#",
    "properties": {
    "publicIPAddress": {
    "id": "#PIP2#"
    }
      }
    },
   {
    "id": "#LBFE2#",
    "properties": {
    "publicIPAddress": {
    "id": "#PIP1#"
     }
       }
    }
  ]
 }
```
## <a name="common-questions-about-swapping-deployments"></a>Häufig gestellte Fragen zum Austauschen der Bereitstellungen

### <a name="what-are-the-prerequisites-for-swapping-between-two-cloud-services"></a>Welche Voraussetzungen müssen erfüllt sein, um zwei Clouddienste austauschen zu können?
Es gibt zwei Hauptvoraussetzungen für einen erfolgreichen Austausch von Clouddiensten (erweiterter Support):
* Wenn Sie für einen der austauschbaren Clouddienste eine statische/reservierte IP-Adresse verwenden möchten, muss der andere Clouddienst ebenfalls eine reservierte IP-Adresse verwenden. Andernfalls tritt bei dem Austausch ein Fehler auf.
* Alle Instanzen Ihrer Rollen müssen ausgeführt werden, bevor Sie den Austausch durchführen können. Sie können den Status Ihrer Instanzen im Azure-Portal auf dem Blatt Übersicht überprüfen. Alternativ können Sie den Befehl „Get-AzRole“ in Windows PowerShell verwenden.

Updates von Gastbetriebssystemen sowie Dienstreparaturvorgänge können ebenfalls dazu führen, dass beim Austauschen von Bereitstellungen ein Fehler auftritt. Weitere Informationen finden Sie unter Behandeln von Problemen mit der Clouddienstbereitstellung.

### <a name="can-i-perform-a-vip-swap-in-parallel-with-another-mutating-operation"></a>Kann ein VIP-Austausch gleichzeitig mit einem anderen Änderungsvorgang durchgeführt werden?
Nein. Der VIP-Austausch ist eine reine Netzwerkänderung, die abgeschlossen sein muss, bevor ein anderer Computevorgang für den Clouddienst bzw. für die Clouddienste ausgeführt wird. Wenn für die Clouddienste während eines VIP-Austauschs ein Aktualisierungs-, Lösch- oder Autoskalierungsvorgang ausgeführt oder ein VIP-Austausch ausgelöst wird, während ein anderer Computevorgang aktiv ist, kann dies einen unerwünschten Zustand des Clouddiensts ohne Möglichkeit zur Wiederherstellung zur Folge haben. 

### <a name="does-a-swap-incur-downtime-for-my-application-how-should-i-handle-it"></a>Führt ein Austausch zu einer Ausfallzeit für die Anwendung? Wie sollte ich dabei vorgehen?
Wie im vorherigen Abschnitt beschrieben, erfolgt der Austausch von Clouddiensten in der Regel schnell, da es sich nur um eine Konfigurationsänderung in Azure Load Balancer handelt. In einigen Fällen kann der Austausch jedoch zehn oder mehr Sekunden dauern und zu vorübergehenden Verbindungsausfällen führen. Um die Auswirkungen auf Ihre Kunden zu minimieren, ist es empfehlenswert, Clientwiederholungslogik zu implementieren.

## <a name="next-steps"></a>Nächste Schritte 
- Überprüfen Sie die [Bereitstellungsvoraussetzungen](deploy-prerequisite.md) für Cloud Services (erweiterter Support).
- Sehen Sie sich die [häufig gestellten Fragen](faq.md) zu Cloud Services (erweiterter Support) an.
- Stellen Sie eine Cloud Service-Instanz (erweiterter Support) über das [Azure-Portal](deploy-portal.md), mit [PowerShell](deploy-powershell.md), einer [Vorlage](deploy-template.md) oder [Visual Studio](deploy-visual-studio.md) bereit.
