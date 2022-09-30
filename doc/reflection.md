# Component scripting

There is no true 'scripting' since components are just c++ classes. However, with the implementation of our homemade reflection, coding a component class is quite special. Here is the explanation.

## Declare component _(in .hpp)_
```c++
#include "game/component.hpp"  

namespace Game
{
    // Call class declaration from a macro
    KK_COMPONENT(MyComponent)
    
    // Add new variable, you will be able to reflect them in .cpp
    Vector3 position;
    float speed = 15.f;

    // Override Component class' method
    void Update() override;

    // Add new function
    void MyUpdate();

   // You can use private/protected
private:
    bool isGrounded = false;

    // Don't forget to end the scope!
    KK_COMPONENT_END
}
```    


## Define component _(in .cpp)_
```c++
#include "my_component.hpp"

// Define component static variable and open the field definitions scope,
// 'using namespace Game' called here
KK_COMPONENT_IMPL_BEGIN(MyComponent)
// Describe the field you want to reflect:
// Push the field in metadata with minimum information.
KK_FIELD_PUSH(MyComponent, position, DataType::FLOAT) ✅
// Tell the metadata that the field is a vector of 3 float.
KK_FIELD_COUNT(3) ✅
// Add a range to clamp position coordinates value
KK_FIELD_RANGE(0.f, 10.f) ✅

KK_FIELD_PUSH(MyComponent, isGrounded, DataType::BOOL) ✅
KK_FIELD_PUSH(MyComponent, speed, DataType::FLOAT) ✅

// Be sure that you are implementing an existing field
KK_FIELD_IMPL(MyComponent, pineapple, DataType::FLOAT) ❌  

// Don't forget to end the scope !
KK_COMPONENT_IMPL_END

void MyComponent::Update()
{
    MyUpdate();
}

void MyComponent::MyUpdate()
{
    Log::Info("my update !");
}
```  

## Macros lists
### _Class declaration .hpp_
> KK_COMPONENT -> The standard component declaration.  
> KK_PRIVATE_COMPONENT -> A private declaration. The component will be hidden in editor.  
> KK_COMPONENT_FROM -> Declare a new component which inherits from a specific component class.

### _Class definition .cpp_
> KK_COMPONENT_IMPL_BEGIN -> The only way to define a declared component class. Furthermore, it opens a scope for field declarations.  
> KK_COMPONENT_IMPL_END -> End the field declarations scope.

### _Field definition .cpp_
> KK_FIELD_PUSH -> Create and add serialized-field data. All the KK_FIELD_ action is applied to this field until the next KK_FIELD_PUSH.   
> KK_FIELD_PUSH_BUTTON -> Create a special field which is an editor button. Link a callback function to this button when it is pressed. This field does not need an existing variable.  
> KK_FIELD_RANGE -> Setup a range limit.  
> KK_FIELD_COUNT -> Evaluate the field as a vector and set the number of elements.  
> KK_FIELD_VIEWONLY -> Prevent field editing.  
> KK_FIELD_CHANGECALLBACK -> Setup a callback function called when field value changed or triggered.  
> KK_FIELD_DROPTARGET -> Setup a callback function called when field receive drag & drop data.  
> KK_FIELD_SAMELINE -> Set the current field on the same line that the previous field.  
> KK_FIELD_TOOLTIP -> Add a tooltip to the field. A tooltip is a descriptive box that appears when the mouse cursor is on the field in editor.  
> KK_FIELD_RENAME -> Rename the field for editor and serialization.

### Add new type to reflect
`EDataType` enum defines the type of the field.  
If you need to add a new Type to reflect, you can simply add it to the enum, and then add a `case` in `Helpers::Edit(io_component)` from `editor/helpers/game_edit.cpp`, and a `Read` and `Write` in the `Serializer` class.  
You can see that we consider camera and light as DataType, it is not a problem at all.

## Warning
In the reflection process, we need to know the offset of each serialized field to locate them in an unsigned char* pointer _(which is a cast of the component)_. To find this offset, we are using the C++ 11 `offsetof()` macro. But when we use `offsetof` with static data, we have an annoying warning _`offsetof could not work properly with static variable`_, but the engine compiles and executes without issue. So we disable the warning log.