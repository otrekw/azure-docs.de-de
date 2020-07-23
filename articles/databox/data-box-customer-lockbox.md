---
title: Konfigurieren einer Lockbox für Azure Data Box
description: Erfahren Sie, wie Sie eine Kunden-Lockbox mit Azure Data Box verwenden.
services: databox
author: alkohli
ms.service: databox
ms.topic: how-to
ms.date: 07/10/2020
ms.author: alkohli
ms.subservice: pod
ms.openlocfilehash: 9c95760d03db976b59537adcecbe39a942b72126
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86209027"
---
# <a name="use-customer-lockbox-for-azure-data-box-preview"></a>Verwenden einer Kunden-Lockbox für Azure Data Box (Vorschau)

Azure Data Box wird zum Übertragen von Kundendaten in und aus Azure verwendet. In manchen Situationen muss der Microsoft-Support im Rahmen einer Supportanfrage möglicherweise auf Kundendaten zugreifen. Sie können die Kunden-Lockbox als Schnittstelle zum Überprüfen und Genehmigen oder Ablehnen dieser Datenzugriffsanforderungen verwenden. 

In diesem Artikel wird erläutert, wie Kunden-Lockbox-Anforderungen initiiert und für spätere Data Box-Import- und -Exportaufträge nachverfolgt werden. Der Artikel gilt sowohl für Azure Data Box- als auch für Azure Data Box Heavy-Geräte. 

## <a name="devops-workflow-for-data-access"></a>DevOps-Workflow für den Datenzugriff

Das Microsoft-Team für Support und Data Box-Vorgänge greift in der Regel nicht auf Kundendaten zu. Probleme werden nach Möglichkeit mithilfe von Standardtools und Telemetriedaten behoben. <!--The only scenarios where there is a need to access customer data is when there is an issue with the data that needs to be fixed. For example, if the data is copied to a wrong folder or is in an incorrect format and is likely to result in an upload or download failure, then Microsoft will try to access your data in the Azure datacenter.--> 

Wenn dies nicht möglich ist und Daten untersucht oder repariert werden müssen, fordert der Microsoft-Support über das Just-In-Time-Portal (JIT) erhöhte Zugriffsrechte an. Das JIP-Portal überprüft die Berechtigungsstufe, bietet mehrstufige Authentifizierung und umfasst außerdem eine Genehmigung von den internen genehmigenden Personen bei Microsoft. Beispielsweise kann „DevOps-Manager“ als genehmigende Person eingesetzt werden. 

Nachdem Ihre Anforderung erhöhter Zugriffsrechte über das JIT-Portal genehmigt wurde, fordert Microsoft bei Aktivierung der Lockbox auch Ihre explizite Einwilligung in den Zugriff auf die Daten an. Der Zugriff wird über den Kunden-Lockbox-Dienst im Portal angefordert und nachverfolgt. 

Wenn Sie die Lockbox nicht aktiviert haben, ist Ihre Einwilligung in den Zugriff auf die Daten nicht erforderlich.


## <a name="prerequisites-for-access-request"></a>Voraussetzungen für die Zugriffsanforderung

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben einen Azure Data Box-Auftrag erstellt gemäß den Anweisungen im
    1. [Tutorial: Bestellen von Azure Data Box](data-box-deploy-ordered.md) für Importaufträge.
    1. [Tutorial: Bestellen von Azure Data Box](data-box-deploy-export-ordered.md) für Exportaufträge.

2. Sie haben die Kunden-Lockbox für Data Box konfiguriert. Dieser Dienst kann abonniert werden. 

    1. Die Kunden-Lockbox ist derzeit als Vorschauversion für den Data Box-Dienst verfügbar. Wenn Sie die Kunden-Lockbox für Data Box in Ihrer Organisation aktivieren möchten, registrieren Sie sich für die [öffentliche Vorschau der Kunden-Lockbox für Azure](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Kwz02N6XVCoKNpxIpqE_hUNzlTUUNYVkozOVlFNVRSWDVHRkkwTFQyViQlQCN0PWcu).
    2. Die Kunden-Lockbox steht automatisch allen Kunden zur Verfügung, die einen Azure-Supportplan mit der Mindestebene „Developer“ besitzen. <!--How do you enable Lockbox? change this for Azure Data Box, perhaps you need a different support plan When you have an eligible support plan, no action is required by you to enable Customer Lockbox. Customer Lockbox requests are initiated by a Microsoft engineer if this action is needed to progress a support ticket that is filed from somebody in your organization.-->

3. Für dieses Problem wurde bereits ein Service Request oder ein Supportticket erstellt. Weitere Informationen zu Supporttickets finden Sie unter [Öffnen eines Supporttickets für Azure Data Box](data-box-disk-contact-microsoft-support.md).


## <a name="track-approve-request-via-lockbox"></a>Nachverfolgen und Genehmigen einer Anforderung über die Lockbox

Führen Sie die folgenden Schritte aus, um eine Anforderung für den Zugriff auf Kundendaten nachzuverfolgen und zu genehmigen:

1. Microsoft erkennt, dass beim Hochladen oder Herunterladen der Daten im Azure-Rechenzentrum ein Problem aufgetreten ist. Beispielsweise wurde der Data Box-Auftrag in der **Datenkopiephase** angehalten. 

    Der Supporttechniker stellt über die Supportsitzung eine Verbindung mit der Data Box her und versucht, das Problem mithilfe von Standardtools und Telemetriedaten zu beheben. Wenn die Data Box-Datenträger gesperrt und Freigaben nicht zugänglich sind, erstellt der Supporttechniker eine Lockbox-Anforderung. 
 
2. In diesem Fall wird die Benachrichtigung in der Regel an den Abonnementadministrator gesendet, Sie können jedoch auch eine Gruppe für Benachrichtigungen konfigurieren. 

3. Die Lockbox-Anforderung wird Ihnen zur Genehmigung im Azure-Portal angezeigt. 

    ![Anforderung im Azure-Portal](./media/data-box-customer-lockbox/3-lockbox-request-azure-portal.png)

    Sie können auf **Genehmigen** klicken, um die Lockbox-Anforderung im Portal zu genehmigen.

    ![Anforderung genehmigen](./media/data-box-customer-lockbox/4-lockbox-request-details-azure-portal.png)


    Nachdem die Anforderung genehmigt wurde, werden die Gerätedatenträger entsperrt, und die Freigaben sind in der Supportsitzung zugänglich.

4. Der Supporttechniker löst das Uploadproblem und deaktiviert anschließend die Supportsitzung.

Nach Behebung des Problems wird der Datenkopierauftrag bis zum Abschluss fortgesetzt.


## <a name="next-steps"></a>Nächste Schritte

- [Kunden-Lockbox für Microsoft Azure](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)

<!--- [Approve, audit support access requests to VMs using Customer Lockbox for Azure](https://azure.microsoft.com/blog/approve-audit-support-access-requests-to-vms-using-customer-lockbox-for-azure/)-->

