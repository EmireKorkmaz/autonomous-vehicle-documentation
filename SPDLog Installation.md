## **SPDLog Installation**

### Before beginning make sure youive your "--version" & "-git" command are up to date and "gcc", "g++", & "cmake" are installed.

> #### Start with the steps listed below.

1. Open Terminal and enter the command:

        git clone https://github.com/gabime/spdlog.git 

    and download the repository.

2. Change directory to downloaded repository.

3. Create a build folder using command: `mkdir build` and change directory to build folder.

4. Type command: `cmake ..` and wait for setup to finish.

5. Type command: `make` and wait for build to complete.

6. Install the Spdlog using command `sudo make install` (make sure you know your root pass phrase).

7. Finish installation with using command `sudo ldconfig`.

> After Installation completes, include spdlog library by adding `#include <spdlog/spdlog.h>` to your C++ code to use SPDLog functions.