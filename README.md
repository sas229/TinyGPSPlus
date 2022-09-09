# TinyGPSPlus
RP2040 port for TinyGPSPlus, forked from TinyGPSPlus for Arduino by Mikal Hart. Relies on the pico stdlib for replacement of Arduino millis() function with other Arduino specific functions replaced by stdlib alternatives. 

To incorporate in your project as an interface library, simply:

```
git clone https://github.com/sas229/TinyGPSPlus.git
```

Or:

```
git submodule add https://github.com/sas229/TinyGPSPlus.git
cd TinyGPSPlus
git submodule update
```

Then add the following to your main CMakeLists.txt file:

```
add_subdirectory(path_to_cloned_TinyGPSPlus_directory)
```

Finally, add TinyGPSPlus to your target link libraries:

```
target_link_libraries(my_project pico_stdlib TinyGPSPlus)
```

For API information see: https://github.com/mikalhart/TinyGPSPlus