---
title: Veröffentlichen eines VM-Angebots im Azure Marketplace
description: Hier erfahren Sie Schritt für Schritt, wie Sie ein vorhandenes VM-Angebot im Azure Marketplace veröffentlichen.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: dsindona
ms.openlocfilehash: bb875a5c4ab1b898b64fe22140414e5d5b7830b8
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273782"
---
# <a name="publish-a-virtual-machine-offer"></a>Veröffentlichen eines VM-Angebots

> [!IMPORTANT]
> Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer Azure Virtual Machines-Angebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote in Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Erstellen eines Azure Virtual Machines-Angebots](https://aka.ms/CreateAzureVMoffer).

 Nachdem Sie das Angebot im Portal definiert und die zugeordneten technischen Ressourcen erstellt haben, wird das Angebot im letzten Schritt zur Veröffentlichung übermittelt. Das folgende Diagramm zeigt die wichtigsten Schritte des Veröffentlichungsprozesses, die ausgeführt werden müssen, um ein Angebot live zu schalten:

![Schritte zur Veröffentlichung eines VM-Angebots](./media/publishvm_013.png)

Die folgende Tabelle enthält die Beschreibung dieser Schritte sowie die voraussichtliche maximale Dauer der einzelnen Schritte:
<!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Veröffentlichungsschritt**           | **Time**    | **Beschreibung**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Überprüfen der Voraussetzungen         | 15 Min.   | Die Angebotsinformationen und -einstellungen werden überprüft.                        |
| Validierung der Testversion (optional) | 2 Stunden | Wenn Sie ausgewählt haben, dass die Testversion aktiviert wird, überprüft Microsoft die Konfiguration der Testversion sowie ihre Bereitstellung und Replikation in den ausgewählten Regionen. |
| Zertifizierung                  | 3 Tage | Das Angebot wird vom Azure-Zertifizierungsteam analysiert. Dabei wird das Angebot auf Viren, Schadsoftware, Sicherheitskonformität und Sicherheitsprobleme untersucht. Sollte ein Problem gefunden werden, erhalten Sie entsprechendes Feedback. |
| Bereitstellung                   | 4 Tage   | Das VM-Angebot wird in den Produktionssystemen des Marketplace repliziert.               |
| Paketerstellung und Registrierung von generierten Leads | \< 1 Stunde  | Die technischen Ressourcen des Angebots werden für die Verwendung durch die Kunden gepackt, und die Leadsysteme werden konfiguriert und eingerichtet. |
|  Einholen der Genehmigung des Herausgebers             |  -        | Letzte Überprüfung und Bestätigung durch den Herausgeber, bevor das Angebot live geschaltet wird. Sie können Ihr Angebot in den ausgewählten Abonnements (in den Schritten für die Angebotsinformationen) bereitstellen, um sicherzugehen, dass es alle Ihre Anforderungen erfüllt.  |
| Bereitstellung                   | 4 Tage | Das fertige VM-Angebot wird in den Produktionssystemen und Regionen des Marketplace repliziert. | 
| Live                           | 4 Tage | Das VM-Angebot wird veröffentlicht, in den erforderlichen Regionen repliziert und öffentlich verfügbar gemacht. |
|  |  |

Es kann bis zu 16 Tage dauern, bis dieser Prozess abgeschlossen ist.  Nach dem Ausführen dieser Veröffentlichungsschritte wird Ihr VM-Angebot im [Microsoft Azure Marketplace ](https://azuremarketplace.microsoft.com/marketplace/) aufgeführt. 

