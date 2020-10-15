---
title: Initialisierer-CodePackages in Service Fabric
description: Beschreibt Initialisierer-CodePackages in Service Fabric.
author: shsha-msft
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: shsha
ms.openlocfilehash: 3be079b97c2660437344f88203fdda06cc6d6740
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86258981"
---
# <a name="initializer-codepackages"></a>Initialisierer-CodePackages

Ab Version 7.1 unterstützt Service Fabric **Initialisierer-CodePackages** für [Container-][containers-introduction-link] und [ausführbare Gastanwendungen][guest-executables-introduction-link]. Initialisierer-CodePackages bieten die Möglichkeit, Initialisierungen im ServicePackage-Bereich auszuführen, bevor andere CodePackages mit der Ausführung beginnen. Ihre Beziehung zu einem ServicePackage ist analog zur Beziehung eines [SetupEntryPoint][setup-entry-point-link] zu einem CodePackage.

Bevor Sie mit diesem Artikel fortfahren, sollten Sie sich mit dem [Service Fabric-Anwendungsmodell][application-model-link] und dem [Service Fabric-Hostingmodell][hosting-model-link] vertraut machen.

> [!NOTE]
> Initialisierer-CodePackages Code Pakete werden derzeit nicht für Dienste unterstützt, die mit dem [Reliable Services][reliable-services-link]-Programmiermodell geschrieben wurden.
 
## <a name="semantics"></a>Semantik

Es wird erwartet, dass ein Initialisierer-CodePackage bis zum **erfolgreichen Abschluss (Exitcode 0)** ausgeführt wird. Ein fehlerhaftes Initialisierer-CodePackage wird neu gestartet, bis es erfolgreich abgeschlossen wurde. Mehrere Initialisierer-CodePackages sind zulässig und werden bis zum **erfolgreichen Abschluss**, **sequenziell**, **in einer angegebenen Reihenfolge** ausgeführt, bevor andere CodePackages im ServicePackage mit der Ausführung beginnen.

## <a name="specifying-initializer-codepackages"></a>Angeben von Initialisierer-CodePackages
Sie können ein CodePackage als Initialisierer markieren, indem Sie das **Initializer**-Attribut im Servicemanifest auf **TRUE** festlegen. Wenn mehrere Initialisierer-CodePackages vorhanden sind, kann ihre Ausführungsreihenfolge über das **ExecOrder**-Attribut angegeben werden. **ExecOrder-** muss eine nicht negative ganze Zahl sein und ist nur für Initialisierer-CodePackages gültig. Initialisierer-CodePackages mit niedrigeren Werten für **ExecOrder** werden zuerst ausgeführt. Wenn **ExecOrder** für ein Initialisierer-CodePackage nicht angegeben wird, wird der Standardwert 0 angenommen. Die relative Ausführungsreihenfolge von Initialisierer-CodePackages mit einem gleichen Wert von **ExecOrder** ist nicht festgelegt.

Im folgenden Codeausschnitt aus einem Dienstmanifest werden drei CodePackages beschrieben, von denen zwei als Initialisierer markiert sind. Wenn dieses ServicePackage aktiviert ist, wird *InitCodePackage0* zuerst ausgeführt, da es den niedrigsten Wert von **ExecOrder** aufweist. Nach erfolgreichem Abschluss (Exitcode 0) von *InitCodePackage0* wird *InitCodePackage1* ausgeführt. Schließlich wird nach erfolgreichem Abschluss von *InitCodePackage1* *WorkloadCodePackage* ausgeführt.

```xml
<CodePackage Name="InitCodePackage0" Version="1.0" Initializer="true" ExecOrder="0">
  ...
</CodePackage>

<CodePackage Name="InitCodePackage1" Version="1.0" Initializer="true" ExecOrder="1">
  ...
</CodePackage>

<CodePackage Name="WorkloadCodePackage" Version="1.0">
  ...
</CodePackage>
```
## <a name="complete-example-using-initializer-codepackages"></a>Vollständiges Beispiel mit Initialisierer-CodePackages

Sehen wir uns ein vollständiges Beispiel mit Initialisierer-CodePackages an.

> [!IMPORTANT]
> Im folgenden Beispiel wird davon ausgegangen, dass Sie mit der Erstellung von [Windows-Containeranwendungen mit Service Fabric und Docker][containers-getting-started-link] vertraut sind.
>
> In diesem Beispiel wird auf mcr.microsoft.com/windows/nanoserver:1809 verwiesen. Windows Server-Container sind nicht mit allen Versionen eines Hostbetriebssystems kompatibel. Weitere Informationen finden Sie unter [Versionskompatibilität von Windows-Containern](/virtualization/windowscontainers/deploy-containers/version-compatibility).

Die folgende Datei „ServiceManifest.xml“ basiert auf dem zuvor beschriebenen ServiceManifest-Codeausschnitt. *InitCodePackage0*, *InitCodePackage1* und *WorkloadCodePackage* sind CodePackages, die Container darstellen. Nach der Aktivierung wird *InitCodePackage0* zuerst ausgeführt. Es protokolliert eine Meldung in einer Datei und wird dann beendet. Als nächstes wird *InitCodePackage1* ausgeführt, das auch eine Meldung in einer Datei protokolliert und dann beendet wird. Zum Schluss wird die Ausführung von *WorkloadCodePackage* gestartet. Es protokolliert ebenfalls eine Meldung in einer Datei, gibt den Inhalt der Datei an **stdout** aus und pingt dann endlos.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ServiceManifest Name="WindowsInitCodePackageServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>Windows Init CodePackage Service</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="WindowsInitCodePackageServiceType"  UseImplicitHost="true"/>
  </ServiceTypes>
  <CodePackage Name="InitCodePackage0" Version="1.0" Initializer="true" ExecOrder="0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from InitCodePackage0. &gt; C:\WorkspaceOnContainer\log.txt</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="InitCodePackage1" Version="1.0" Initializer="true" ExecOrder="1">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from InitCodePackage1. &gt;&gt; C:\WorkspaceOnContainer\log.txt</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="WorkloadCodePackage" Version="1.0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from WorkloadCodePackage. &gt;&gt; C:\WorkspaceOnContainer\log.txt &amp;&amp; type C:\WorkspaceOnContainer\log.txt &amp;&amp; ping -t 127.0.0.1 &gt; nul</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>
</ServiceManifest>
```

In der folgenden Datei „ApplicationManifest.xml“ wird eine Anwendung auf der Grundlage der oben erläuterten Datei „ServiceManifest.xml“ beschrieben. Beachten Sie, dass sie die gleiche **Volumeeinbindung** für alle Container angibt, d. h. **C:\WorkspaceOnHost** wird in allen drei Containern unter **C:\WorkspaceOnContainer** eingebunden. Das Ergebnis davon ist, dass alle Container in der Reihenfolge, in der sie aktiviert werden, in dieselbe Protokolldatei schreiben.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WindowsInitCodePackageApplicationType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <Description>Windows Init CodePackage Application</Description>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="WindowsInitCodePackageServicePackage" ServiceManifestVersion="1.0"/>
    <Policies>
      <ContainerHostPolicies CodePackageRef="InitCodePackage0" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>

     <ContainerHostPolicies CodePackageRef="InitCodePackage1" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>

      <ContainerHostPolicies CodePackageRef="WorkloadCodePackage" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>

  <DefaultServices>
    <Service Name="WindowsInitCodePackageService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="WindowsInitCodePackageServiceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
Nachdem das ServicePackage erfolgreich aktiviert wurde, sollte der Inhalt von **C:\WorkspaceOnHost\log.txt** wie folgt lauten.

```console
C:\Users\test>type C:\WorkspaceOnHost\log.txt
Hi from InitCodePackage0.
Hi from InitCodePackage1.
Hi from WorkloadCodePackage.
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln.

* [Service Fabric und Container][containers-introduction-link]
* [Service Fabric und ausführbare Gastdateien][guest-executables-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[containers-getting-started-link]: service-fabric-get-started-containers.md
[guest-executables-introduction-link]: service-fabric-guest-executables-introduction.md
[reliable-services-link]: service-fabric-reliable-services-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md
[setup-entry-point-link]: service-fabric-run-script-at-service-startup.md
