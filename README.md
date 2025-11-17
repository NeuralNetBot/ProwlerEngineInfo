# Prowler Engine Info

## Project is currrently closed source

## Basic Editor

Entities can be created and change parenting in the scene graph through the scene panel.

![image](https://github.com/user-attachments/assets/2dd2ff24-bdeb-4b45-8210-35d785c097d4)

Entity component attributees can be modified and added through the entity panel.

![image](https://github.com/user-attachments/assets/5ff6931a-1b16-4e21-ac89-c6647f09cd04)

Full editor view

![image](https://github.com/user-attachments/assets/ae3a49c7-9126-41e1-9990-be9bfbb3489c)

## Scripts

### Info
Scripts are contained in a ScriptComponent for any entity.

```cpp
classs MyScript : public Prowler::Script
{
public:
    void onCreate()            //called on script creation
    {
        cc = entity.getComponent<CameraComponent>();
    }
    void onUpdate(float dt).   //called every tick
    {
        cc.fov = 60 + 15 * sin(dt);
    }
    void onDestroy() {}.       //called on script removal or entity deletion 
private:
    CameraComponent* cc;

    float myFloat;
    PROWLER_SCRIPT_PARAM(MyScript, myFloat) //params will show up editable in editor
    Vec3 myVec;
    PROWLER_SCRIPT_PARAM(MyScript, myVec)
}:
PROWLER_SCRIPT(MyScript)
```

## ECS

### Info
Components are stored in dense continuous arrays with custom pool based allocation.\
Maintains references even when entities or components are removed from the scene.

### Basic usage

Defining components:

```cpp
struct MyComponent
{
  float foo;

  MyComponent(float foo) : foo(foo) {}
};
PROWLER_COMPONENT(MyComponent)
```

Creating entities and working with components:

```cpp
Entity& e = scene->CreateEntity("optional entity name");
e.addComponent<MyComponent>(/*optional args for constructors*/ 1.0f);

bool hasMyComponent = e.hasComponent<MyComponent>();

MyComponent& myComp = e.getComponent<MyComponent>();
```

Iterating lists of components:

```cpp
for(auto [component, entity] : scene->getComponents<MyComponent>())
{
  std::cout << component.foo << " id: " << (int32_t)entity << std::endl;
}

//or for faster iteration with no need for the entity handle

for(auto& component : scene->getComponentsFast<MyComponent>())
{
  std::cout << component.foo << std::endl;
}
```

## Asset management
Assets are all refered to under a UUID for every unique asset.

Assets can be retrieved and or loaded as the following.
```cpp
AssetManager::get<ExampleAssetType>(my_asset_uuid);
```
These can also be called within scripts to create new entities and assign their respective assets manually.

Loading will happen asynchronously if the asset is yet to be loaded.
If the asset is not loaded the asset will return a temporary/placeholder asset that will automatically update and propagate the nessesary events to ensure it is properly in use.
In the event an asset cannot be loaded, such as a invalid handle or missing files, the asset will remain as the temporary/placeholder asset and give a suitable error message.

All assets are stored under two modes:
Editor Mode where the assets are stored in a user friendly YAML files linking to source files such as images or 3d formats.
Runtime Mode where the assets get put into a pure binary format and compressed down into faster runtime formats such as BCn formats for textures.
