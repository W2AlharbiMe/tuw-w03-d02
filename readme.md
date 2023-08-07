# Homework



# 1
![img1](https://user-images.githubusercontent.com/110352036/214043369-d0c388e0-30c7-4f6f-b47d-877fa7a70579.png)


**Output:**
```bash
hey form message1
```


**Explanation:**
it appears that in this code there's only 1 method that's annotated with `@Bean` which's `getMessage1`, since this is the only method that's annotated with `@Bean` it will run first
and save string "1"  in the container.

---


# 2
![img2](https://user-images.githubusercontent.com/110352036/214044044-8daab2c3-5e06-493c-be5b-49ca49598975.png)

**Output:**
```bash
hey from message1
hey from message2
```



**Explanation:**
there are 2 methods that are annotated with `@Bean`. the first method `getMessage1` is annotated with `@Qualifier("1")` right after `@Bean`, this method returns the string "1" and it will be saved in the container. the second method `getMessage2` require a parameter of type `String` called `data` this parameter was annotated with `@Qualifier("1")` which will point to the returned value from `getMessage1` because that method was annotated with the exact same `@Qualifier("1")`, so parameter `data` depends on `getMessage1` returned value this is why the first method that will run is `getMessage1` and then `getMessage2`.


---

# 3
![img3](https://user-images.githubusercontent.com/110352036/214044106-69893ed6-81bb-4abc-8966-38a4dd8a7e4a.png)



**Output:**
```bash
...
hey from message3
hey from message2
...
```


**Explanation:**
this code have 3 methods and all of them are annotated with `@Bean`, `@Qualifier`. I can't tell when method `getMessage1` will run it could be the first or in the middle or the last, but what i'm pretty sure about is that `getMessage3` will run before `getMessage2` because `getMessage2` parameter `data` depends on `getMessage3` returned value.

---

# 4
![img4_1](https://user-images.githubusercontent.com/110352036/214044208-e866fed2-81f6-4fdb-910d-b7688d9099f2.png)
![img4_2](https://user-images.githubusercontent.com/110352036/214044228-f7b3c7e2-6af0-4957-84bc-53e1348793d7.png)

**Output:**
```bash
hey from message1
hey from Main Controller
hey from message3
hey from message2
```


**Explanation:**
this code have 3 methods and all of them are annotated with `@Bean`, `@Qualifier`, there's also a component `MainController` that have a constructor. the constructor parameter `data` have been annotated with `@Qualifier("1")` which means the parameter `data` depends on `getMessage1` returned value. according to spring docs annotation `@Component` will **Instantiate** an instance of `MyController` and save it in the container. my guess would be that the container will pass `getMessage1` returned value in the constructor. spring will create an instance of `MyController` after getting the returned value from `getMessage1`, so what i'm pretty sure about is that `getMessage1` will run before **Instantiating** an instance of `MyController` because the constructor parameter `data` depends on `getMessage1` returned value then  `getMessage3` will run before `getMessage2` because `getMessage2` parameter `data` depends on `getMessage3` returned value. the order would be:

- call `getMessage1`
- **Instantiate** instance of `MyController` and pass `getMessage1` returned value.
- call `getMessage3` and pass the returned value to `getMessage2`
- call `getMessage2`


---

# 5 
![img5_1](https://user-images.githubusercontent.com/110352036/214045127-d653f6a8-064a-45b1-8bb8-43a2a40f8463.png)
![img5_2](https://user-images.githubusercontent.com/110352036/214045192-a6e82281-6d54-4a84-bef6-d2f44ddfcf9d.png)



**Output:**
```bash
...
hey from message3
hey from message2
hey from Main Controller
...
```



**Explanation:**
this code have 3 methods and all of them are annotated with `@Bean`, `@Qualifier`, there's also a component `MainController` that have a constructor. the constructor parameter `data` have been annotated with `@Qualifier("2")` which means the parameter `data` depends on `getMessage2` returned value. according to spring docs annotation `@Component` will **Instantiate** an instance of `MyController` and save it in the container. my guess would be that the container will pass `getMessage2` returned value in the constructor. however method `getMessage2` will run right after `getMessage3` and then after that it will create an instance of `MyController` and pass the returned value from `getMessage2` because the constructor parameter `data` depends on `getMessage2` returned value, and before calling `getMessage2` spring will call `getMessage3` because `getMessage2` parameter `data` depends on `getMessage3` returned value, so the order would be:
- call `getMessage3` and pass the returned value to `getMessage2`
- call `getMessage2`
- **Instantiate** instance of `MyController` and pass `getMessage2` returned value.
- as for `getMessage1` I can't tell when it will be called, it could be the first or in middle or last.

