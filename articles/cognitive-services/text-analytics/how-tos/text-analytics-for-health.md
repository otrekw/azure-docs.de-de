---
title: Verwenden von Text Analytics for Health
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie medizinische Informationen mithilfe von Text Analytics for Health aus unstrukturiertem klinischem Text extrahiert und bezeichnet werden.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: aahi
ms.openlocfilehash: cbc950647dc2f7102f20766c22f434526ab66639
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122193"
---
# <a name="how-to-use-text-analytics-for-health-preview"></a>Gewusst wie: Verwenden von Text Analytics for Health (Vorschau)

> [!IMPORTANT] 
> Text Analytics for Health ist eine Vorschaufunktion, die „WIE BESEHEN“ und „MIT ALLEN MÄNGELN“ zur Verfügung gestellt wird. Daher sollte **Text Analytics for Health (Vorschau) nicht in einer Produktionsumgebung implementiert oder bereitgestellt werden.** Text Analytics for Health ist nicht für die Verwendung als medizinisches Hilfsmittel, zur klinischen Unterstützung, als Diagnosetool oder als sonstige Technologie für den Einsatz in Diagnose, Therapie, Entschärfung, Behandlung oder Prävention von Krankheiten oder medizinischen Fällen vorgesehen oder verfügbar, und Microsoft erteilt keine Lizenzen oder Rechte zur Nutzung dieser Funktion für die beschriebenen Zwecke. Diese Funktion ist nicht als Ersatz für professionelle medizinische Beratung oder medizinische Gutachten, Diagnosen, Behandlungen oder das klinische Urteilsvermögen einer medizinischen Fachkraft konzipiert oder vorgesehen und sollte nicht als solcher eingesetzt werden. Jede Verwendung von Text Analytics for Health liegt in der alleinigen Verantwortung des Kunden. Microsoft garantiert nicht, dass Text Analytics for Health oder die in Verbindung mit dieser Funktion bereitgestellten Materialien für medizinische Zwecke ausreichen oder anderweitig die gesundheitlichen oder medizinischen Anforderungen von Personen erfüllen. 


Azure Text Analytics for Health ist ein Containerdienst, der relevante medizinische Informationen aus unstrukturierten Texten wie Arztbriefen, Entlassungszusammenfassungen, klinischen Dokumenten und elektronischen Gesundheitsakten extrahiert und bezeichnet.  

## <a name="features"></a>Features

Der Text Analytics for Health-Container führt zurzeit die Erkennung benannter Entitäten (Named Entity Recognition, NER), Beziehungsextrahierung, Entitätsnegation und Entitätsverknüpfung für englischsprachigen Text in Ihrer eigenen Entwicklungsumgebung durch, die Ihren spezifischen Anforderungen an Sicherheit und Data Governance entspricht.

#### <a name="named-entity-recognition"></a>[Erkennung benannter Entitäten](#tab/ner)

Die Erkennung benannter Entitäten erkennt Wörter und Ausdrücke in unstrukturiertem Text, die einem oder mehr semantischen Typen zugeordnet werden können, wie etwa Diagnose, Namen von Medikamenten, Symptom/Auffälligkeit oder Alter.

> [!div class="mx-imgBorder"]
> ![Health NER](../media/ta-for-health/health-named-entity-recognition.png)

#### <a name="relation-extraction"></a>[Beziehungsextrahierung](#tab/relation-extraction)

Die Beziehungsextrahierung identifiziert sinnvolle Verbindungen zwischen Begriffen, die im Text erwähnt werden. Beispielsweise wird die Beziehung „Zeit der Bedingung“ durch Zuordnen eines Bedingungsnamens zu einem Zeitpunkt gefunden. 

> [!div class="mx-imgBorder"]
> ![Health RE](../media/ta-for-health/health-relation-extraction.png)


#### <a name="entity-linking"></a>[Entitätsverknüpfung](#tab/entity-linking)

Entitätsverknüpfung unterscheidet separate Entitäten, indem sie im Text erwähnte benannte Entitäten Konzepten zuordnet, die sie in einer vordefinierten Konzeptdatenbank findet. Beispielsweise im Unified Medical Language System (UMLS).

> [!div class="mx-imgBorder"]
> ![Health EL](../media/ta-for-health/health-entity-linking.png)

Text Analytics for Health unterstützt die Verknüpfung mit den Vokabularen für Gesundheit und Biomedizin in der Metathesaurus Knowledge Source des Unified Medical Language System ([UMLS](https://www.nlm.nih.gov/research/umls/sourcereleasedocs/index.html)).

#### <a name="negation-detection"></a>[Negationserkennung](#tab/negation-detection) 

Die Bedeutung medizinischer Inhalte wird in hohem Maße durch Modifizierer wie Verneinung beeinflusst, die bei Fehldiagnosen kritische Auswirkungen haben können. Text Analytics for Health unterstützt die Negationserkennung für die verschiedenen im Text erwähnten Entitäten. 

> [!div class="mx-imgBorder"]
> ![Health NEG](../media/ta-for-health/health-negation.png)

---

Die vollständige Liste der unterstützten Entitäten finden Sie in den von Text Analytics for Health zurückgegeben [Entitätskategorien](../named-entity-types.md?tabs=health).

## <a name="supported-languages"></a>Unterstützte Sprachen

Text Analytics for Health unterstützt nur Dokumente in englischer Sprache.

## <a name="request-access-to-the-container-registry"></a>Anfordern des Zugriffs auf die Containerregistrierung

Füllen Sie das [Formular zum Anfordern von Cognitive Services-Containern](https://aka.ms/cognitivegate) aus, und übermitteln Sie es, um Zugriff auf den Container anzufordern. Zurzeit wird Ihnen die Verwendung von Text Analytics for Health nicht in Rechnung gestellt. 

[!INCLUDE [Request access to the container registry](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="install-the-container"></a>Installieren des Containers

Es gibt mehrere Möglichkeiten zum Installieren und Ausführen des Containers. 

1. Verwenden des [Azure-Portals](text-analytics-how-to-install-containers.md?tabs=healthcare) zum Erstellen einer Ressource für die Textanalyse und Verwenden von Docker zum Abrufen Ihres Containers.
2. Verwenden der unten dargestellten PowerShell- und [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)-Skripts zum Automatisieren der Containerkonfiguration bei der Ressourcenbereitstellung.

### <a name="install-the-container-using-azure-web-app-for-containers"></a>Installieren des Containers mithilfe von Azure Web App für Container

Azure [Web-App für Container](https://azure.microsoft.com/services/app-service/containers/) ist eine Azure-Ressource, die für die Ausführung von Containern in der Cloud vorgesehen ist. Sie bietet Standardfunktionen wie automatische Skalierung, Unterstützung von Docker-Containern und Docker Compose, HTTPS-Unterstützung und vieles mehr.

> [!NOTE]
> Mithilfe von Azure Web App erhalten Sie automatisch eine Domäne im Format `<appservice_name>.azurewebsites.net`

Führen Sie dieses PowerShell-Skript an der Azure CLI aus, um eine Web-App für Container zu erstellen, und verwenden Sie dazu Ihr Abonnement und das Containerimage über HTTPS. Warten Sie, bis das Skript abgeschlossen wurde (ungefähr 20 Minuten), bevor Sie die erste Anforderung absenden.

```bash
$subscription_name = ""                    # THe name of the subscription you want you resource to be created on.
$resource_group_name = ""                  # The name of the resource group you want the AppServicePlan
                                           #    and AppSerivce to be attached to.
$resources_location = ""                   # This is the location you wish the AppServicePlan to be deployed to.
                                           #    You can use the "az account list-locations -o table" command to
                                           #    get the list of available locations and location code names.
$appservice_plan_name = ""                 # This is the AppServicePlan name you wish to have.
$appservice_name = ""                      # This is the AppService resource name you wish to have.
$TEXT_ANALYTICS_RESOURCE_API_KEY = ""      # This should be taken from the Text Analytics resource.
$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT = "" # This should be taken from the Text Analytics resource.
$DOCKER_REGISTRY_SERVER_PASSWORD = ""      # This will be provided separately.
$DOCKER_REGISTRY_SERVER_USERNAME = ""      # This will be provided separately.
$DOCKER_IMAGE_NAME = "containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest"

az login
az account set -s $subscription_name
az appservice plan create -n $appservice_plan_name -g $resource_group_name --is-linux -l $resources_location --sku P3V2
az webapp create -g $resource_group_name -p $appservice_plan_name -n $appservice_name -i $DOCKER_IMAGE_NAME -s $DOCKER_REGISTRY_SERVER_USERNAME -w $DOCKER_REGISTRY_SERVER_PASSWORD
az webapp config appsettings set -g $resource_group_name -n $appservice_name --settings Eula=accept Billing=$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT ApiKey=$TEXT_ANALYTICS_RESOURCE_API_KEY

# Once deployment complete, the resource should be available at: https://<appservice_name>.azurewebsites.net
```

### <a name="install-the-container-using-azure-container-instance"></a>Installieren des Containers mithilfe von Azure Container Instance

Sie können auch eine Azure-Containerinstanz (ACI) verwenden, um die Bereitstellung zu vereinfachen. ACI ist eine Ressource, die Ihnen das Ausführen von Docker-Containern nach Bedarf in einer verwalteten, serverlosen Azure-Umgebung ermöglicht. 

Schritte zum Bereitstellen einer ACI-Ressource mithilfe des Azure-Portals finden Sie unter [Verwenden von Azure Container Instances](text-analytics-how-to-use-container-instances.md). Alternativ können Sie das unten dargestellte PowerShell-Skript an der Azure CLI verwenden, das eine ACI in Ihrem Abonnement erstellt und dazu das Containerimage verwendet.  Warten Sie, bis das Skript abgeschlossen wurde (ungefähr 20 Minuten), bevor Sie die erste Anforderung absenden.

> [!NOTE] 
> Azure Container Instances beinhalten keine HTTPS-Unterstützung für die integrierten Domänen. Wenn Sie HTTPS benötigen, müssen Sie es manuell konfigurieren. Dazu gehört auch das Erstellen eines Zertifikats und das Registrieren einer Domäne. Anweisungen für die Ausführung mit NGINX finden Sie unten.

```bash
$subscription_name = ""                    # The name of the subscription you want you resource to be created on.
$resource_group_name = ""                  # The name of the resource group you want the AppServicePlan
                                           # and AppService to be attached to.
$resources_location = ""                   # This is the location you wish the web app to be deployed to.
                                           # You can use the "az account list-locations -o table" command to
                                           # Get the list of available locations and location code names.
$azure_container_instance_name = ""        # This is the AzureContainerInstance name you wish to have.
$TEXT_ANALYTICS_RESOURCE_API_KEY = ""      # This should be taken from the Text Analytics resource.
$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT = "" # This should be taken from the Text Analytics resource.
$DOCKER_REGISTRY_SERVER_PASSWORD = ""      # This will be provided separately.
$DOCKER_REGISTRY_SERVER_USERNAME = ""      # This will be provided separately.
$DNS_LABEL = ""                            # This is the DNS label name you wish your ACI will have
$DOCKER_REGISTRY_LOGIN_SERVER = "containerpreview.azurecr.io"
$DOCKER_IMAGE_NAME = "containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest"

az login
az account set -s $subscription_name
az container create --resource-group $resource_group_name --name $azure_container_instance_name --image $DOCKER_IMAGE_NAME --cpu 5 --memory 12 --registry-login-server $DOCKER_REGISTRY_LOGIN_SERVER --registry-username $DOCKER_REGISTRY_SERVER_USERNAME --registry-password $DOCKER_REGISTRY_SERVER_PASSWORD --port 5000 --dns-name-label $DNS_LABEL --environment-variables Eula=accept Billing=$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT ApiKey=$TEXT_ANALYTICS_RESOURCE_API_KEY

# Once deployment complete, the resource should be available at: http://<unique_dns_label>.<resource_group_region>.azurecontainer.io:5000
```

### <a name="secure-aci-connectivity"></a>Schützen der ACI-Konnektivität

Standardmäßig wird bei der Verwendung von ACI mit der Container-API keine Sicherheit bereitgestellt. Das hat den Grund, dass Container meist als Teil eines Pods ausgeführt werden, das von außen durch eine Netzwerkbrücke geschützt ist. Jedoch können Sie einen Container mit einer Front-End-Komponente verändern, wodurch der Containerendpunkt privat bleibt. In den folgenden Beispielen wird [NGINX](https://www.nginx.com) als Eingangsgateway verwendet, um HTTPS/SSL und die Authentifizierung mit Clientzertifikaten zu unterstützen.

> [!NOTE]
> NGINX ist ein hochleistungsfähiger Open Source-HTTP-Server und -Proxy. Ein NGINX-Container kann verwendet werden, um eine TLS-Verbindung für einen einzelnen Container zu terminieren. Komplexere TLS-Terminierungslösungen sind mit NGINX-basierten Eingangsgateways ebenfalls möglich.

#### <a name="set-up-nginx-as-an-ingress-gateway"></a>Einrichten von NGINX als Eingangsgateway

NGINX verwendet [Konfigurationsdateien](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/), um Features zur Laufzeit zu aktivieren. Um TLS-Terminierung für einen anderen Dienst zu ermöglichen, müssen Sie ein SSL-Zertifikat angeben, um die TLS-Verbindung zu terminieren, und `proxy_pass`, um eine Adresse für den Dienst anzugeben. Unten sehen Sie ein Beispiel.


> [!NOTE]
> `ssl_certificate` erwartet die Angabe eines Pfads innerhalb des lokalen Dateisystems des NGINX-Containers. Die für `proxy_pass` angegebene Adresse muss im Netzwerk des NGINX-Containers verfügbar sein.

Der NGINX-Container lädt alle Dateien in der `_.conf_`, die unter `/etc/nginx/conf.d/` in den HTTP-Konfigurationspfad eingehängt werden.

```nginx
server {
  listen              80;
  return 301 https://$host$request_uri;
}
server {
  listen              443 ssl;
  # replace with .crt and .key paths
  ssl_certificate     /cert/Local.crt;
  ssl_certificate_key /cert/Local.key;

  location / {
    proxy_pass http://cognitive-service:5000;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP  $remote_addr;
  }
}
```

#### <a name="example-docker-compose-file"></a>Docker Compose-Beispieldatei

Im Beispiel unten wird gezeigt, wie eine [Docker Compose](https://docs.docker.com/compose/reference/overview)-Datei erstellt werden kann, um die Container für NGINX und Text Analytics for Health bereitzustellen:

```yaml
version: "3.7"
services:
  cognitive-service:
    image: {IMAGE_ID}
    ports:
      - 5000:5000
    environment:
      - eula=accept
      - billing={ENDPOINT_URI}
      - apikey={API_KEY}
      - Logging:Disk:Format=json
    volumes:
        # replace with path to logs folder
      - <path-to-logs-folder>:/output
  nginx:
    image: nginx
    ports:
      - 443:443
    volumes:
        # replace with paths for certs and conf folders
      - <path-to-certs-folder>:/cert
      - <path-to-conf-folder>:/etc/nginx/conf.d/
```

Führen Sie zum Initialisieren dieser Docker Compose-Datei den folgenden Befehl an einer Konsole auf der Stammebene der Datei aus:

```bash
docker-compose up
```

Weitere Informationen finden Sie in der NGINX-Dokumentation unter [NGINX SSL Termination](https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-http/) (NGINX SSL-Terminierung).


## <a name="example-api-request"></a>Beispiel für API-Anforderung
Der Container stellt REST-basierte Endpunkt-APIs für die Abfragevorhersage bereit.

Verwenden Sie die unten aufgeführte cURL-Beispielanforderung, um eine Abfrage an den bereitgestellten Container zu senden, und ersetzen Sie die Variable `serverURL` durch den passenden Wert.

```bash
curl -X POST 'http://<serverURL>:5000/text/analytics/v3.0-preview.1/domains/health' --header 'Content-Type: application/json' --header 'accept: application/json' --data-binary @example.json

```

Der folgende JSON-Code ist ein Beispiel für eine JSON-Datei, die an den POST-Text der Text Analytics for Health-API-Anforderung angefügt ist:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Patient reported itchy sores after swimming in the lake."
    },
    {
      "language": "en",
      "id": "2",
      "text": "Prescribed 50mg benadryl, taken twice daily."
    }
  ]
}
```

## <a name="api-response-body"></a>API-Antworttext

Der folgende JSON-Code ist ein Beispiel für den Antworttext der Text Analytics for Health-API:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "id": "0",
                    "offset": 17,
                    "length": 11,
                    "text": "itchy sores",
                    "type": "SYMPTOM_OR_SIGN",
                    "score": 0.97,
                    "isNegated": false
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "id": "0",
                    "offset": 11,
                    "length": 4,
                    "text": "50mg",
                    "type": "DOSAGE",
                    "score": 1.0,
                    "isNegated": false
                },
                {
                    "id": "1",
                    "offset": 16,
                    "length": 8,
                    "text": "benadryl",
                    "type": "MEDICATION_NAME",
                    "score": 0.99,
                    "isNegated": false,
                    "links": [
                        {
                            "dataSource": "UMLS",
                            "id": "C0700899"
                        },
                        {
                            "dataSource": "CHV",
                            "id": "0000044903"
                        },
                        {
                            "dataSource": "MMSL",
                            "id": "899"
                        },
                        {
                            "dataSource": "MSH",
                            "id": "D004155"
                        },
                        {
                            "dataSource": "NCI",
                            "id": "C300"
                        },
                        {
                            "dataSource": "NCI_DTP",
                            "id": "NSC0033299"
                        },
                        {
                            "dataSource": "PDQ",
                            "id": "CDR0000039163"
                        },
                        {
                            "dataSource": "PSY",
                            "id": "05760"
                        },
                        {
                            "dataSource": "RXNORM",
                            "id": "203457"
                        }
                    ]
                },
                {
                    "id": "2",
                    "offset": 32,
                    "length": 11,
                    "text": "twice daily",
                    "type": "FREQUENCY",
                    "score": 1.0,
                    "isNegated": false
                }
            ],
            "relations": [
                {
                    "relationType": "DOSAGE_OF_MEDICATION",
                    "score": 1.0,
                    "entities": [
                        {
                            "id": "0",
                            "role": "ATTRIBUTE"
                        },
                        {
                            "id": "1",
                            "role": "ENTITY"
                        }
                    ]
                },
                {
                    "relationType": "FREQUENCY_OF_MEDICATION",
                    "score": 1.0,
                    "entities": [
                        {
                            "id": "1",
                            "role": "ENTITY"
                        },
                        {
                            "id": "2",
                            "role": "ATTRIBUTE"
                        }
                    ]
                }
            ]
        }
    ],
    "errors": [],
    "modelVersion": "2020-05-08"
}
```

> [!NOTE] 
> In der Negationserkennung kann sich in manchen Fällen ein einzelner Negationsausdruck auf mehrere Begriffe zugleich beziehen. Die Negation einer erkannten Entität wird in der JSON-Ausgabe durch den booleschen Wert des `isNegated`-Flags dargestellt:

```json
{
  "id": "2",
  "offset": 90,
  "length": 10,
  "text": "chest pain",
  "type": "SYMPTOM_OR_SIGN",
  "score": 0.9972,
  "isNegated": true,
  "links": [
    {
      "dataSource": "UMLS",
      "id": "C0008031"
    },
    {
      "dataSource": "CHV",
      "id": "0000023593"
    },
    ...
```

## <a name="see-also"></a>Weitere Informationen

* [Übersicht über die Textanalyse](../overview.md)
* [Kategorien benannter Entitäten](../named-entity-types.md)
* [Neuigkeiten](../whats-new.md)
