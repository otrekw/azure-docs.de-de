---
title: Azure Service Fabric-Tests
description: Informationen zum Modellieren eines Livetests in Azure Service Fabric mit Anwendungs- und Dienstmanifestdateien
ms.topic: conceptual
author: tugup
ms.author: tugup
ms.date: 3/12/2020
ms.openlocfilehash: 07a1b836ca7ea79244e303f54654dfcaa6e5fcb9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82137585"
---
# <a name="liveness-probe"></a>Livetest
Ab Version 7.1 unterstützt Azure Service Fabric einen Livetestmechanismus für [Containeranwendungen][containers-introduction-link]. Mit einem Livetest können Sie die Reaktionsfähigkeit einer Containeranwendung melden, die neu gestartet wird, wenn sie nicht schnell antwortet.
Dieser Artikel bietet eine Übersicht über das Definieren eines Livetests über Manifestdateien.

Bevor Sie mit diesem Artikel fortfahren, sollten Sie sich mit dem [Service Fabric-Anwendungsmodell][application-model-link] und dem [Service Fabric-Hostingmodell][hosting-model-link] vertraut machen.

> [!NOTE]
> Der Livetest wird nur für Container im NAT-Netzwerkmodus unterstützt.

## <a name="semantics"></a>Semantik
Sie können nur einen Livetest pro Container angeben und dessen Verhalten mit den folgenden Feldern steuern:

* `initialDelaySeconds`: Anfängliche Verzögerung (in Sekunden) bis zum Starten der Ausführung des Tests, nachdem der Container gestartet wurde. Der unterstützte Wert ist **int**. Der Standardwert ist 0, und der Mindestwert ist 0.

* `timeoutSeconds`: Zeitraum in Sekunden, nach dem der Test als fehlerhaft betrachtet wird, wenn er nicht erfolgreich abgeschlossen wurde. Der unterstützte Wert ist **int**. Der Standardwert ist 1, und der Mindestwert ist 1.

* `periodSeconds`: Zeitraum in Sekunden zur Angabe der Häufigkeit des Tests. Der unterstützte Wert ist **int**. Der Standardwert ist 10, und der Mindestwert ist 1.

* `failureThreshold`: Beim Erreichen dieses Werts wird der Container neu gestartet. Der unterstützte Wert ist **int**. Der Standardwert ist 3, und der Mindestwert ist 1.

* `successThreshold`: Bei einem Fehler muss der Test für diesen Wert erfolgreich ausgeführt werden, damit er als erfolgreich angesehen wird. Der unterstützte Wert ist **int**. Der Standardwert ist 1, und der Mindestwert ist 1.

Es kann jeweils höchstens ein Test zu einem Container vorhanden sein. Wenn der Test nicht in dem in **timeoutSeconds** festgelegten Zeitraum abgeschlossen wird, warten Sie, und zählen Sie die Zeit bis **failureThreshold**. 

Außerdem stellt Service Fabric die folgenden [Integritätsberichte][health-introduction-link] für **DeployedServicePackage** für den Test bereit:

* `OK`: Der Test ist erfolgreich für den in **successThreshold** festgelegten Wert.

* `Error`: Der Test **failureCount** ==  **failureThreshold** vor dem Neustart des Containers.

* `Warning`: 
    * Beim Test tritt ein Fehler auf und **failureCount** < **failureThreshold**. Dieser Integritätsbericht bleibt so lange bestehen, bis **failureCount** den in **failureThreshold** oder **successThreshold** festgelegten Wert erreicht.
    * Bei erfolgreicher Ausführung nach einem Fehler wird die Warnung beibehalten, jedoch mit aktualisierten aufeinanderfolgenden Erfolgen.

## <a name="specifying-a-liveness-probe"></a>Angeben eines Livetests

Sie können den Test in der Datei „ApplicationManifest.xml“ unter **ServiceManifestImport** angeben.

Der Test kann für jede der folgenden Möglichkeiten erfolgen:

* HTTP
* TCP
* Exec 

### <a name="http-probe"></a>HTTP-Test

Für den HTTP-Test sendet Service Fabric eine HTTP-Anforderung an den angegebenen Port und Pfad. Ein Rückgabecode, der größer oder gleich 200 und kleiner als 400 ist, zeigt einen Erfolg an.

Hier ist ein Beispiel für das Angeben eines HTTP-Tests:

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <HttpGet Path="/" Port="8081" Scheme="http">
              <HttpHeader Name="Foo" Value="Val"/>
              <HttpHeader Name="Bar" Value="val1"/>
            </HttpGet>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

Der HTTP-Test weist zusätzliche Eigenschaften auf, die Sie festlegen können:

* `path`: Pfad für die HTTP-Anforderung.

* `port`: Port für Tests. Diese Eigenschaft ist obligatorisch. Der Bereich liegt zwischen 1 und 65535.

* `scheme`: Schema zum Herstellen einer Verbindung mit dem Codepaket. Wenn diese Eigenschaft auf „HTTPS“ festgelegt ist, wird die Zertifikatüberprüfung übersprungen. Die Standardeinstellung ist „HTTP“.

* `httpHeader`: Header, die in der Anforderung festgelegt werden sollen. Sie können mehrere Header angeben.

* `host`: Host-IP-Adresse für die Verbindung.

### <a name="tcp-probe"></a>TCP-Test

Bei TCP-Tests versucht Service Fabric, einen Socket für den Container über den angegebenen Port zu öffnen. Wenn eine Verbindung hergestellt werden kann, wird der Test als erfolgreich betrachtet. Hier ist ein Beispiel für die Angabe eines Tests, der einen TCP-Socket verwendet:

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <TcpSocket Port="8081"/>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

### <a name="exec-probe"></a>Exec-Test

Bei diesem Test wird ein **exec**-Befehl in den Container ausgegeben und auf den Abschluss des Befehls gewartet.

> [!NOTE]
> Der **exec**-Befehl nimmt eine durch Kommas getrennte Zeichenfolge entgegen. Der Befehl im folgenden Beispiel funktioniert mit einem Linux-Container.
> Wenn Sie Windows-Container testen möchten, verwenden Sie **cmd**.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <Exec>
              <Command>ping,-c,2,localhost</Command>
            </Exec>
          </Probe>        
       </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

## <a name="next-steps"></a>Nächste Schritte
Verwandte Informationen finden Sie im folgenden Artikel:
* [Service Fabric und Container][containers-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[health-introduction-link]: service-fabric-health-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md

