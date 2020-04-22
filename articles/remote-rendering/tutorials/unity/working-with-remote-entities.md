---
title: Verwenden von Remoteentitäten in Unity
description: Tutorial zum Veranschaulichen der Arbeit mit ARR-Entitäten.
author: florianborn71
ms.author: flborn
ms.date: 02/01/2020
ms.topic: tutorial
ms.openlocfilehash: db1f6a53121e05b29f7e3441af027985a141bc2e
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310214"
---
# <a name="tutorial-working-with-remote-entities-in-unity"></a>Tutorial: Verwenden von Remoteentitäten in Unity

[Tutorial: Einrichten eines ganz neuen Unity-Projekts](project-setup.md) hat gezeigt, wie ein neues Unity-Projekt für die Zusammenarbeit mit Azure Remote Rendering (ARR) konfiguriert wird. In diesem Tutorial sehen wir uns die gängigsten Funktionen an, die ARR-Benutzer benötigen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Wählen von Objekten mithilfe von Raycasting
> * Überschreiben von Objektzuständen wie Tönungsfarbe, Auswahlzustand und Sichtbarkeit
> * Löschen von Remoteentitäten
> * Verschieben von Remoteentitäten
> * Verwenden von Schnittebenen für Suchvorgänge in Objekten

## <a name="prerequisites"></a>Voraussetzungen

* Dieses Tutorial baut auf dem [Tutorial: Einrichten eines ganz neuen Unity-Projekts](project-setup.md) auf.

> [!TIP]
> Das [ARR-Beispielrepository](https://github.com/Azure/azure-remote-rendering) enthält vorbereitete Unity-Projekte für alle Tutorials im Ordner *Unity*, die Sie als Referenz verwenden können.

## <a name="pick-objects"></a>Wählen von Objekten

Wir möchten mit Objekten interagieren, weshalb wir als Erstes Objekte unter dem Mauszeiger wählen müssen.

Erstellen Sie ein [neues Skript](https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html) namens **RemoteRaycaster**, und ersetzen Sie seinen gesamten Inhalt durch den nachstehenden Code:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using UnityEngine;

[RequireComponent(typeof(ARRServiceUnity))]
public class RemoteRaycaster : MonoBehaviour
{
    public double MaxDistance = 30.0;

    private ARRServiceUnity arrService = null;

    private void Awake()
    {
        arrService = GetComponent<ARRServiceUnity>();
    }

    private async Task<Entity> RemoteRayCast(Vector3 origin, Vector3 dir)
    {
        Entity entity = null;

        var raycast = new RayCast(origin.toRemotePos(), dir.toRemoteDir(), MaxDistance, HitCollectionPolicy.ClosestHit);

        var hits = await arrService.CurrentActiveSession.Actions.RayCastQueryAsync(raycast).AsTask();

        if (hits != null)
        {
            foreach (var hit in hits)
            {
                var hitEntity = hit.HitEntity;
                if (hitEntity == null)
                {
                    continue;
                }

                entity = hitEntity;
                break;
            }
        }

        return entity;
    }

    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        Vector3 position = Input.mousePresent ? Input.mousePosition : new Vector3(Screen.width / 2, Screen.height / 2, 0);
        var ray = Camera.main.ScreenPointToRay(position);

        Raycast(ray.origin, ray.direction);
    }

    public async void Raycast(Vector3 origin, Vector3 direction)
    {
        var entity = await RemoteRayCast(origin, direction);
        if (entity != null)
        {
            Debug.Log("Object Hit: " + entity.Name);
        }
    }
}
```

Fügen Sie diese Komponente dem *RemoteRendering*-Objekt in Ihrer Szene hinzu.

> [!WARNING]
>
> Die Komponente *RemoteRaycaster* erfordert die Komponente *ARRServiceUnity*, die an dasselbe Objekt angefügt werden muss. *ARRServiceUnity* ist eine Hilfsklasse, die den Zugriff auf einige ARR-Funktionen erleichtert. Es darf jedoch nur eine einzige Instanz dieser Komponente in der Szene geben. Achten Sie daher darauf, alle Komponenten, die *ARRServiceUnity* erfordern, demselben GameObject hinzuzufügen.
> Wenn Sie aus mehreren GameObject-Instanzen auf ARR-Funktionalität zugreifen möchten, fügen Sie die Komponente*ARRServiceUnity* entweder nur einer davon hinzu, und verweisen in den anderen Skripts auf diese Komponente, oder Sie greifen direkt auf die ARR-Funktionalität zu.

Klicken Sie auf „Wiedergeben“, stellen Sie eine Verbindung mit einer Sitzung her, und laden Sie ein Modell. Zeigen Sie nun auf Objekte in der Szene, und beobachten Sie die Konsolenausgabe. Sie sollte den Objektnamen jedes Teils ausgeben, über den Sie den Mauszeiger bewegen.

## <a name="highlight-objects"></a>Hervorheben von Objekten

Im nächsten Schritt möchten wir eine visuelle Rückmeldung geben, auf welche Teile eines Modells der Benutzer zeigt. Dazu fügen wir eine [HierarchicalStateOverrideComponent](../../overview/features/override-hierarchical-state.md) an die Entität an, die wir gewählt haben. Diese Komponente kann verwendet werden, um verschiedene Features eines Objekts zu aktivieren oder zu deaktivieren. Hier verwenden wir sie, um eine Tönungsfarbe festzulegen und [ Konturenrendering](../../overview/features/outlines.md) zu aktivieren.

Erstellen Sie eine weitere Skriptdatei namens **RemoteModelEntity**, und ersetzen Sie ihren Inhalt durch den folgenden Code:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;

public class RemoteModelEntity : MonoBehaviour
{
    public Color HighlightColor = new Color(1.0f, 0.0f, 0.0f, 0.1f);

    public Entity Entity => localSyncObject != null ? localSyncObject?.Entity : null;

    private RemoteEntitySyncObject localSyncObject = null;
    private ARRHierarchicalStateOverrideComponent localStateOverride = null;

    public void OnEnable()
    {
        localSyncObject = GetComponent<RemoteEntitySyncObject>();
        localStateOverride = GetComponent<ARRHierarchicalStateOverrideComponent>();

        if (localStateOverride == null)
        {
            localStateOverride = gameObject.AddComponent<ARRHierarchicalStateOverrideComponent>();
            var remoteStateOverride = localSyncObject.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();

            if (remoteStateOverride == null)
            {
                // if there is no HierarchicalStateOverrideComponent on the remote side yet, create one
                localStateOverride.Create(RemoteManagerUnity.CurrentSession);
            }
            else
            {
                // otherwise, bind our local stateOverride component to the remote component
                localStateOverride.Bind(remoteStateOverride);
            }
        }

        localStateOverride.RemoteComponent.TintColor = HighlightColor.toRemote();
    }

    public void OnDisable()
    {
        SetStateOverride(false);

        if (localStateOverride.IsComponentValid)
        {
            localStateOverride.RemoteComponent.Enabled = false;
            localStateOverride.enabled = false;
        }
    }

    private void SetStateOverride(bool on)
    {
        if (localStateOverride.IsComponentValid)
        {
            localStateOverride.RemoteComponent.UseTintColorState = on ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
            localStateOverride.RemoteComponent.SelectedState = on ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
        }
    }

    public void SetFocus(bool on)
    {
        SetStateOverride(on);
    }
}
```
> [!CAUTION]
> Weisen Sie dieses Skript keinem Spielobjekt zu, da es programmgesteuert durch den unten stehenden Code zugewiesen wird.

Als Nächstes müssen wir unsere Komponente *RemoteRaycaster* erweitern, um die Komponente *RemoteModelEntity* dem zuvor gewählten Objekt hinzuzufügen.

Fügen Sie den folgenden Code zur **RemoteRaycaster**-Implementierung hinzu, und entfernen Sie die duplizierten Funktionen:

```csharp
    private RemoteModelEntity focusedModel = null;

    public async void Raycast(Vector3 origin, Vector3 direction)
    {
        var remoteEntity = await RemoteRayCast(origin, direction);

        if (focusedModel != null)
        {
            if (focusedModel.Entity == remoteEntity)
            {
                // picked the same object as before
                return;
            }

            ClearFocus();
        }

        if (remoteEntity == null)
        {
            // picked no object at all
            return;
        }

        // get the instance of a Unity GameObject for the ARR entity
        var entityGO = remoteEntity.GetOrCreateGameObject(UnityCreationMode.DoNotCreateUnityComponents);

        // ensure the game object has the RemoteModelEntity component
        focusedModel = entityGO.GetComponent<RemoteModelEntity>();
        if (focusedModel == null)
        {
            focusedModel = entityGO.AddComponent<RemoteModelEntity>();
        }

        focusedModel.SetFocus(true);
    }

    private void ClearFocus()
    {
        focusedModel.SetFocus(false);
        focusedModel = null;
    }
```

Führen Sie Ihr Projekt aus, und zeigen Sie auf ein Modell. Sie sollten sehen, dass es einen roten Farbton und eine weiße Auswahlkontur erhält.

## <a name="isolate-the-selected-object"></a>Isolieren des ausgewählten Objekts

Ein weiterer Zweck der [HierarchicalStateOverrideComponent](../../overview/features/override-hierarchical-state.md) ist die Möglichkeit, Sichtbarkeit außer Kraft zu setzen. Dies ermöglicht Ihnen, ein ausgewähltes Objekt vom restlichen Modell zu isolieren. Öffnen Sie das Skript **RemoteModelEntity**, fügen Sie den folgenden Code hinzu, und entfernen Sie die duplizierten Funktionen:

```csharp
    private bool isolated = false;
    private HierarchicalStateOverrideComponent parentOverride = null;

    public void ToggleIsolate()
    {
        SetIsolated(!isolated);
    }

    public void SetIsolated(bool on)
    {
        if (localStateOverride == null || !localStateOverride.IsComponentValid || isolated == on)
        {
            return;
        }

        // find the top most parent object that has a HierarchicalStateOverrideComponent
        if (parentOverride == null)
        {
            var modelRoot = transform;

            while (modelRoot.parent != null)
            {
                modelRoot = modelRoot.parent;

                var parentSyncObject = modelRoot.GetComponent<RemoteEntitySyncObject>();

                var stateOverrideComp = parentSyncObject?.Entity.FindComponentOfType<HierarchicalStateOverrideComponent>();

                if (stateOverrideComp != null)
                {
                    parentOverride = stateOverrideComp;
                }
            }
        }

        if (parentOverride != null)
        {
            isolated = on;

            parentOverride.HiddenState = isolated ? HierarchicalEnableState.ForceOn : HierarchicalEnableState.InheritFromParent;
            localStateOverride.RemoteComponent.HiddenState = isolated ? HierarchicalEnableState.ForceOff : HierarchicalEnableState.InheritFromParent;
        }
    }

    public void SetFocus(bool on)
    {
        SetStateOverride(on);

        if (!on)
        {
            SetIsolated(false);
        }
    }
```

Dieser Code beruht auf einer Zustandsüberschreibungskomponente beim obersten Objekt in der Hierarchie, die alle Objekte unsichtbar macht. Anschließend setzt sie die Sichtbarkeit beim ausgewählten Objekt wieder außer Kraft, um dieses eine Objekt sichtbar zu machen. Daher müssen wir beim Stammobjekt eine Zustandsüberschreibungskomponente erstellen.

Öffnen Sie das Skript **RemoteRendering**, und fügen Sie den nachstehenden Code oben in die *LoadModel*-Funktion ein:

```csharp
    public async void LoadModel()
    {
        // create a root object to parent a loaded model to
        modelEntity = arrService.CurrentActiveSession.Actions.CreateEntity();
        arrService.CurrentActiveSession.Actions.CreateComponent(ObjectType.HierarchicalStateOverrideComponent, modelEntity);

        ...
    }
```

Schließlich benötigen wir eine Möglichkeit zum Umschalten der Sichtbarkeit. Öffnen Sie das Skript **RemoteRaycaster**, und ersetzen Sie die *Update*-Funktion:

```csharp
    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        var ray = Camera.main.ScreenPointToRay(Input.mousePosition);

        Raycast(ray.origin, ray.direction);

        if (Input.GetMouseButtonDown(1) && focusedModel != null)
        {
            focusedModel.ToggleIsolate();
        }
    }
```

Führen Sie den Code aus, und klicken Sie mit der rechten Maustaste auf einen Teil des Modells. Der Rest des Modells wird ausgeblendet, und nur das hervorgehobene Element bleibt sichtbar.

## <a name="remove-gameobject-instances-of-remote-entities"></a>Entfernen von GameObject-Instanzen von Remoteentitäten

Möglicherweise haben Sie bemerkt, dass der Code weiterhin Objekte erstellt, sie aber nie bereinigt. Dies ist auch im Panel mit der Objekthierarchie sichtbar. Wenn Sie während der Simulation die Remoteobjekthierarchie erweitern, sehen Sie immer mehr Objekte, die jedes Mal angezeigt werden, wenn Sie den Mauszeiger über einen neuen Teil des Modells bewegen.

Zu viele Objekte in einer Szene wirken sich negativ auf die Leistung aus. Nicht mehr benötigte Objekte müssen stets bereinigt werden.

Fügen Sie den folgenden Code in das Skript **RemoteRaycaster** ein, und entfernen Sie die duplizierten Funktionen:

```csharp
    private void ClearFocus()
    {
        focusedModel.SetFocus(false);
        CleanHierarchy(focusedModel.gameObject);
        focusedModel = null;
    }

    private void CleanHierarchy(GameObject focusedGO)
    {
        var sync = focusedGO?.GetComponent<RemoteEntitySyncObject>();
        if (sync == null || !sync.IsEntityValid)
        {
            return;
        }

        sync.Entity.DestroyGameObject(EntityExtensions.DestroyGameObjectFlags.DestroyEmptyParents | EntityExtensions.DestroyGameObjectFlags.KeepRemoteRoot);
    }
```

## <a name="move-objects"></a>Verschieben von Objekten

Im nächsten Schritt möchten wir ein ausgewähltes Objekt verschieben. Fügen Sie in das Skript **RemoteRaycaster** diesen Code ein, und entfernen Sie die duplizierte Funktion:

```csharp
    private Vector3 lastPosition = Vector3.zero;

    private void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            return;
        }

        if (Input.GetMouseButton(0))
        {
            if (focusedModel)
            {
                // note: mousePosition is in 2D screen-space coordinates and has no relation with
                // the 3D object position. This just happens to work good enough for demonstration.
                var delta = Input.mousePosition - lastPosition;
                focusedModel.transform.position += delta * Time.deltaTime;
            }
        }
        else
        {
            Vector3 position = Input.mousePresent ? Input.mousePosition : new Vector3(Screen.width / 2, Screen.height / 2, 0);
            var ray = Camera.main.ScreenPointToRay(position);

            Raycast(ray.origin, ray.direction);

            if (Input.GetMouseButtonDown(1) && focusedModel != null)
            {
                focusedModel.ToggleIsolate();
            }
        }

        lastPosition = Input.mousePosition;
    }
```

> [!IMPORTANT]
> Wenn Sie diesen Code ausführen, werden Sie feststellen, dass nichts passiert. Das liegt daran, dass die Änderung der Transformation eines Objekts die Zustandsänderung aus Leistungsgründen nicht automatisch mit dem Server synchronisiert. Stattdessen müssen Sie diese Zustandsänderung entweder manuell an den Server übertragen, oder Sie aktivieren **SyncEveryFrame** für die Komponente *RemoteEntitySyncObject*.

Öffnen Sie das Skript **RemoteModelEntity**, und fügen Sie diese Zeile hinzu:

```csharp
    public void OnEnable()
    {
        ...

        localSyncObject.SyncEveryFrame = true;
    }
```

Wenn Sie den Code erneut ausführen, sollten Sie in der Lage sein, mit der linken Maustaste auf ein Objekt zu klicken und es mit der Maus umher zu ziehen.

## <a name="add-a-cut-plane"></a>Hinzufügen einer Schnittebene

Das letzte Feature, das wir in diesem Tutorial ausprobieren wollen, sind [Schnittebenen](../../overview/features/cut-planes.md). Eine Schnittebene schneidet Teile von gerenderten Objekten weg, sodass Sie in sie hineinschauen können.

Erstellen Sie in der Szene **CutPlane** ein neues GameObject. Erstellen Sie ein neues Skript, und nennen Sie es **RemoteCutPlane**. Fügen Sie die Komponente dem neuen GameObject hinzu.

Öffnen Sie die Skriptdatei, und ersetzen Sie ihren Inhalt durch folgenden Code:

```csharp
using Microsoft.Azure.RemoteRendering;
using Microsoft.Azure.RemoteRendering.Unity;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RemoteCutPlane : MonoBehaviour
{
    private ARRCutPlaneComponent localCutPlaneComponent = null;
    private RemoteEntitySyncObject remoteEntitySync = null;

    void Update()
    {
        if (!RemoteManagerUnity.IsConnected)
        {
            // can't do anything while we are not connected
            return;
        }

        if (localCutPlaneComponent == null)
        {
            localCutPlaneComponent = gameObject.CreateArrComponent<ARRCutPlaneComponent>(RemoteManagerUnity.CurrentSession);
        }

        if (remoteEntitySync == null)
        {
            remoteEntitySync = gameObject.GetComponent<RemoteEntitySyncObject>();
            remoteEntitySync.SyncEveryFrame = true;
        }

        localCutPlaneComponent.RemoteComponent.Normal = Axis.X;
        localCutPlaneComponent.RemoteComponent.FadeLength = 0.025f;
        localCutPlaneComponent.RemoteComponent.FadeColor = new Color4Ub(255, 128, 0, 255);
        localCutPlaneComponent.RemoteComponent.Enabled = true;
    }

    void OnDisable()
    {
        if (localCutPlaneComponent && localCutPlaneComponent.IsComponentValid)
        {
            localCutPlaneComponent.RemoteComponent.Enabled = false;
        }

        if (remoteEntitySync && remoteEntitySync.IsEntityValid)
        {
            remoteEntitySync.SyncEveryFrame = false;
        }
    }
}
```

Wenn Sie Ihren Code jetzt ausführen, sollten Sie sehen, wie das Modell von der Ebene aufgeschnitten wurde. Sie können das *CutPlane*-Objekt auswählen und es im Fenster *Szene* verschieben und drehen. Sie können die Schnittebene ein- und ausschalten, indem Sie das Schnittebenenobjekt deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

Sie kennen jetzt die wichtigste Funktionalität für die Interaktion mit Remoteobjekten. Im nächsten Tutorial sehen wir uns das Anpassen des Erscheinungsbilds einer Szene an.

> [!div class="nextstepaction"]
> [Tutorial: Ändern der Umgebung und Materialien](changing-environment-and-materials.md)
