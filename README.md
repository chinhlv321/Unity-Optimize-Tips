# Unity-Optimize-Tips
- Using **localPosition** is faster than using **position**. This is also true for localRotation

- Reduce GetComponent calls. Using **GetComponent** or built-in component accessors can have a noticeable overhead. You can avoid this by getting a reference to the component once and assigning it to a variable (sometimes referred to as "caching" the reference): **cachedComponent** = **GetComponent**. This is also true for other Unity stuff, so caching transform is faster than not caching **transform**, and caching **Time.deltaTime** is faster than not caching **Time.deltaTime**

- **Vector3() \* (5f * 4f)** is faster than** Vector3() \* 5f * 4f**

- Caching **List.Count** is faster than not caching **List.Count**. But caching **Array.Length** is not faster than not caching **Array.Length** 

- Avoid vector math. **position += 5f** is slower than **position.x += 5f, position.y += 5f, position.z += 5f**

- Avoid **Mathf.Sqrt()** or **Vector3.magnitude** which involves a square root. It's better to use the square distance **Vector3.sqrMagnitude** if you for example need to compare vectors. This also means that you should avoid **Mathf.Pow()** because it can also do square roots if the second parameter is 0.5.

- Avoid **foreach**

- Use object pooling if you need to create and destroy a lot of items (such as bullets or enemies)

- Parenting an instantiated object immediately **newObj = Instantiate(go, parent)** is faster than doing it afterwards **newObj.transform.parent = parent**

- If you for example have many rotating coins, it's faster to rotate one single coin and then use **Graphics.DrawMesh()** to draw the other coins by using the mesh of the first coin

- **OnBecameVisible** and **OnBecameInvisible** is useful to avoid computations that are only necessary when the object is visible

- Use **Coroutines** if you don't need to update every frame. You should also limit the amount of scripts that's using **Update**(), the fewer scripts that's calling **Update**() the better

- Use structs instead of classes 

- Primitive colliders are faster than mesh colliders. And never move a static collider (a collider without a RigidBody). If you want to move it, then add a RigidBody to it and set it to kinematic

- Combine nearby objects that have the same material into single meshes so they are drawn together. You can do this either manually or by using Unity's Draw call batching. But your Unity projects may also be slower because it makes culling less efficient and more objects affected by light, so you have to test what's good for you! Another alternative (since Unity 5.4) is to use GPU instancing.

- Use deferred processing. If you are for example adding objects to the game while it's running, add a few each frame instead of all in one frame

- Use **System.Stopwatch** to measure time in Unity so you can see if what you are optimizing is improving

- Use **System.Text.StringBuilder** to combine strings instead of string concatenation **finalString = string1 + string2**

- Using **go.CompareTag("Enemy")** is faster than **go.tag == "Enemy"**

- Layers are faster than tags when it comes to working with collision logic

- **Mathf.Max(a, Mathf.Max(b, c))** is faster than **Mathf.Max(a, b, c)** because calling Max with three arguments means invoking **Mathf.Max(params int[] args)**, which then allocates 36 bytes on the heap for each function call. Calling functions with a variable number of arguments actually allocates those args on the heap in a temporary array

- There is no performance difference between **for** and while **loops**

- Raycasts. Don't extend the ray's length more than you need to. The greater the ray, the more objects need to be tested against it. Don't use use Raycasts inside a FixedUpdate() function, sometimes even inside an Update() may be an overkill. Be specific on what the ray should hit and always try to specify a layer mask on the raycast function. If you want a ray to hit an object which is on layer which id is 10, what you should specify is 1<<10, if you want for the ray to hit everything except what is on layer 10 then simply use the bitwise complement operator (~)

- Don't use **FindObject**. It's better to store the objects you want to find in a list and then you modify the list as the objects are active/inactive with **OnEnable** and **OnDisable**

- **Camera.main** calls **FindObjectWithTag("MainCamera")** so cache it instead once in the beginning
