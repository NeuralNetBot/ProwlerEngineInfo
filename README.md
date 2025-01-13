# Prowler Engine Info

## ECS
### Basic usage

Defining components:

```cpp
struct MyComponent
{
  float foo;
};
PROWLER_COMPONENT(MyComponent)
```

Creating entities and adding components:

```cpp
Entity& e = scene->CreateEntity("optional name");
e.addComponent<MyComponent>(/*optional args for constructors*/);
```

Iterating lists of components:

```cpp
for(auto [component, entity] : scene->getComponents<MyComponent>())
{
  std::cout << component.foo << " id: " << (int32_t)entity << std::endl;
}
```
or for faster iteration with no need for the entity handle
```cpp
for(auto& component : scene->getComponentsFast<MyComponent>())
{
  std::cout << component.foo << std::endl;
}
```
