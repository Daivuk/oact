# oact

Onut's action manager to implement undo/redo in applications.
oact stands for "Onut's Action".

## Usage

It is recommended to use shared_ptr with this library. So resources that are stack into the history retains throught the lambda capture. Also capture lambdas by values [=] for better results.

```cpp
// In initialization code
auto pActionMgr = oact::ActionManager::create();

...

// Moved an entity in the editor
void onEntityMoved(Entity *pEntity,
                   Vector3 prevPosition,
                   Vector3 newPosition)
{
    pActionMgr->addAction("Move Action",
        [=]() { // onRedo
            pEntity->setPosition(newPosition);
        },
        [=]() { // onUndo
            pEntity->setPosition(prevPosition);
        });
}

...

// Undo/Redo action
if (keyPressed(Ctrl) && keyPressed(Z))
    pActionMgr->undo();

if (keyPressed(Ctrl) && keyPressed(Y))
    pActionMgr->redo();

```

## Change history size

The default is 1000. Depending on the application, this can be heavy on memory.

```cpp
pActionMgr->setMaxHistory(50);
```

## Clearing

If loading a new document and history need to be cleared, call `clear()`.

## Init an Destroy

Two extra lambdas are provided in `doAction` and `addAction`. Especially useful if your application does manual memory management.

```cpp
void *pCustomActionData = malloc(...);

pActionMgr->doAction("Some Action",
    [=]() { // onRedo
        ...
    },
    [=]() { // onUndo
        ...
    },
    [=]() { // onInit
        pEntity->retain();
    },
    [=]() { // onDestroy
        free(pCustomActionData);
        pEntity->release();
    });
```
