### Uniforms
Uniforms are a way to send data into shaders that we can later use to process the scene. 
When you want to use an uniform and set it, you need to find its location and then set it using ```glUniform``` command.
The command does not necessarily match the type you are setting, but using wrong setter can result into your data leaking into other neighboring uniforms.

### Finding uniform location
This location is 'per program', thus having more than one uniform of the same name in one program (all its shaders) can result into not being able to set one of them.

Function ```glGetUniformLocation``` returns our uniform location and required program handle (id of program given when the program is created),
and the name of our uniform.
We could be calling this function every time we want to set a uniform, but that would be wasteful.

### Caching uniform location
The best solution in our case would be a cache that stores all previously accessed uniforms.
We will be using ```std::unordered_map``` because it's quick and we do not require any order to be followed.

```cpp
std::unordered_map<std::string, int> cache;
```
This map will store an int (location) and our uniforms name. Now we just need to fill it. We will use a function that will return location
and require name of our uniform.

```cpp
int getUniformLocation(const std::string& name) {

}
```
Remember to use ```const std::string&``` to use const reference instead of copy.

The first thing we need to do is find the uniform name in our cache, if nothing is found, then cache the location and return it.
```cpp
if (cache.find(name) != cache.end()) {
  return cache[name];
}
```

This will return our location if it was found. Now we need to find its location in our program.
```cpp
int location;

if ((location = glGetUniformLocation(programID, name.c_str())) != -1) {
  cache[name] = location;
} else {
  printf_s("INVALID LOCATION\n");
}
```

Here you can see that we created a temporary int to store out location. You can see, that the function takes C-style string, so we need to convert it.
We set it and immediately compare in the if statement to -1, 
which means that our name is invalid and does not exist in the program scope. If we are successful, we can cache our location.
If the location is invalid, then we want to notify the user.

Now the last thing is to return our newly gained location. You can see the whole function below:

```cpp
int getUniformLocation(const std::string& name) {
  if (cache.find(name) != cache.end()) {
    return cache[name];
  }
  
  int location;
  if ((location = glGetUniformLocation(programID, name.c_str()) != -1) {
    cache[name] = location;
  } else {
    printf_s("INVALID LOCATION\n");
  }
  
  return location;
}
```

### Setting uniforms
When setting a lot of different uniforms, it can be a hassle to use the correct setter every time. Usually, you would want to abstract the raw OpenGL code a bit.
One way to simplify the process a bit is to setup many functions to set varying types and then create one templated setter that would choose them by the type of our argument.

The only thing that we need to set an uniform is its location and value. Writting our ```getUniformLocation``` in every wrapper function would be pretty obscure, so we will use integer parameter and pass the location from the big setter function.
Now we can write some wrapper setters:
```cpp
void setUniformS(int loc, float a) { glUniform1f(loc, a); }    
void setUniformS(int loc, vec2& a) { glUniform2fv(loc, 1, &a[0]); }
```

The only requirement is that all our setters have the same name.
When we have all our required setters prepared, we can use templates to hide all of them behind one big setter.
Note that this setter has to have different name than our smaller setters.

We will use variadic templates to allow many parameters at once (since one glUniform does not always have only one parameter)
```cpp
template <typename ... T> void setUniform(const std::string& name, T&& ... args) {
  setUniformS(getUniformLocation(name), std::forward<T>(args)...);
}
```

We will use ```T&& ...``` as a parameter - ```&&``` will allow us to use perfect forwarding and ```...``` marks expanding arguments.
This however poses a problem for our small setters. We will probably want to ban all calls that do not have any parameters.
We can do that by using empty setter with and false assertion:
```cpp
void setUniformS(int loc) { assert(false); }
```

The reason to name all our setters the same was to make the compiler decide what function we want to use depending on our arguments.
We can forward those arguments to the appropriate function by using ```std::forward<T>(args)...```. This will expand and forward all our arguments.















