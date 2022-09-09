# TinyGPSPlus
RP2040 port for TinyGPSPlus, forked from TinyGPSPlus for Arduino by Mikal Hart. Relies on the pico stdlib for replacement of Arduino millis() function with other Arduino specific functions replaced by stdlib alternatives. 

To incorporate in your project as an interface library, simply:

```bash
git clone https://github.com/sas229/TinyGPSPlus.git
```

Or alternatively:

```bash
git submodule add https://github.com/sas229/TinyGPSPlus.git
cd TinyGPSPlus
git submodule update
```

Then add the following to your main CMakeLists.txt file:

```cmake
add_subdirectory(path_to_cloned_TinyGPSPlus_directory)
```

Finally, add TinyGPSPlus to your target link libraries:

```cmake
target_link_libraries(my_project pico_stdlib TinyGPSPlus)
```

A basic example using a typical uart GPS unit running at 9600 baud might look a bit like this in C++:

```cpp
#include <stdio.h>
#include <string>
#include <iostream>
#include <iomanip>
#include <cstring>
#include "pico/stdlib.h"
#include "TinyGPSPlus.h"

// UART defines.
#define UART0_TX_PIN    0
#define UART0_RX_PIN    1
#define UART0_BAUD_RATE 9600
#define UART0_DATA_BITS 8
#define UART0_STOP_BITS 1
#define UART0_PARITY    UART_PARITY_NONE

// Namespaces.
using namespace std;

TinyGPSPlus gps;

int main()
{
    stdio_init_all();
    
    // Initialise UART0.
    uart_init(uart0, UART0_BAUD_RATE);
    gpio_set_function(UART0_TX_PIN, GPIO_FUNC_UART);
    gpio_set_function(UART0_RX_PIN, GPIO_FUNC_UART);

    // Set the data format.
    uart_set_format(uart0, UART0_DATA_BITS, UART0_STOP_BITS, UART0_PARITY);

    char ch;
    string sentence;
    while (true) {
        while (uart_is_readable(uart0)) {
            ch = uart_getc(uart0);
            gps.encode(ch);
            if (ch == 36) {
                // Print captured sentence and then clear.
                char* sentence_char_array = &sentence[0];
                cout << sentence;
                sentence.clear();
                sentence += ch;
            }
            else {
                sentence += ch;
            }
        } 
        // Print parsed GPS location.
        if (gps.location.isUpdated() && gps.time.isUpdated()) {
            cout << "\nTime: " << setfill('0') << setw(2) << +gps.time.hour();
            cout << ":" << setfill('0') << setw(2) << +gps.time.minute();
            cout << ":" << setfill('0') << setw(2) << +gps.time.second();
            cout << "; Latitude: " << gps.location.lat();
            cout << "; Longitude: " << gps.location.lng() << "\n";
        }
    }

    return 0;
}
```

For further API information see: https://github.com/mikalhart/TinyGPSPlus