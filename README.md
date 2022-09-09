# TinyGPSPlus
RP2040 port for TinyGPSPlus, forked from TinyGPSPlus for Arduino by Mikal Hart. Relies on the pico stdlib for timing. 

To incorporate in your project as an interface library, simply:

```
git clone https://github.com/sas229/TinyGPSPlus.git
```

Then add the following to your main CMakeLists.txt file:

```
add_subdirectory(path_to_TinyGPSPlus_folder)
```

Finally, add TinyGPSPlus to your target link libraries:

```
target_link_libraries(my_project pico_stdlib TinyGPSPlus)
```

