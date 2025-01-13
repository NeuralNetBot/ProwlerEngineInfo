# Prowler Engine Info

## ECS
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
Entity& e = scene->CreateEntity("optional name");
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
