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
