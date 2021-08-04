## JavaScript

#### How we can implement a pure function with **_Array.map**_ and other methods like this ?
You can implement your method like below code:
```
MyArray.map(({...value}, index) => {
                value.property = AnyValue
                return value
                })
                
                
```
In fact with each rotation of the loop, you keep a copy of your array property and put it in the main array.
