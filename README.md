# Using Arduino in Visual Studio Code on Debian, for Adafruit Feather ESP32-S3 Reverse TFT

## 1. Install arduino-cli

1. As per page https://arduino.github.io/arduino-cli/0.35/installation/
   do this:
    ```
    sudo -s
    curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | BINDIR=/usr/local/bin sh
    ```

## 2. Debian / Linux Mint

Using a relatively recent (as per December 2023) Debian-based distro.

1. On a "Linux Mint" version 21.2 (Victoria), based on Ubuntu version 22.04.2
   LTS (Jammy), it looks we only need:
    ```
    sudo apt install python3-serial
    ```

## 3. Set up VSCode

1. As per the page https://www.circuitstate.com/tutorials/how-to-use-vs-code-for-creating-and-uploading-arduino-sketches/ 
   "How to Use VS Code for Creating and Uploading Arduino Sketches", install the
   "Arduino" extension from Microsoft.

2. Click on the extension's settings, then:

    - In "Arduino: Path", specify `/usr/local/bin/arduino-cli`.

## 4. Visit project

1. Open VSCode on the folder containing the desired `*.ino` file.

## 5. Board setup

(As per https://learn.adafruit.com/esp32-s3-reverse-tft-feather/arduino-ide-setup-2)

1. In the Arduino plugin's settings, under "Arduino: Additional Urls", add:
    ```
    https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
    ```

2. F1 (command palette) `Arduino: Board Manager`
    - In the filter, enter: ESP32
    - Locate "esp32 by Espressif Systems", click Install

## 6. Sketch setup

1. F1 `Arduino: Board config`
    - Selected board: `Adafruit Feather ESP32-S3 Reverse TFT (esp-32)`
    - All fields:

        ![Board config](img/arduino-board-config.png?raw=true "Board config")

    - These values apparently correctly flash the firmware and skip
      circuitpython.


## 6. Libraries

1. F1 `Arduino: Library manager`, then add:
    - Adafruit GFX Library
    - Adafruit ST7735 and ST7789 Library

## 7. Flash

1. In the VSCode tab showing the `.ino` file, click
   ![Upload](img/upload.png?raw=true "Upload"). With the board config as per §5.3,
   this should work fine.

2. Click the reset button on the board.

## 8. Git

The `.vscode/` folder contains settings that are IMHO not optimally distributed
in their config files when it is about git tracking, so I suggest to handle the
files as follows:
| File                    | Contains                                   | Decision     |
| ----------------------- | ------------------------------------------ | ------------ |
| `arduino.json`          | Board name and conf (1), port (2)          | Track        |
| `c_cpp_properties.json` | Toolchain paths (3) and compiler flags (4) | Do not track |

- (1) Board name and conf should be tracked
- (2) Port should not be tracked, as they depend on OS
- (3) Paths should not be tracked, as they depend on user
- (4) Flags may be tracked, unless we accept the default values, which we will

So, if we accept this unperfect situation:

1. Edit your `.gitignore` file and add these lines:
    ```
    .vscode/c_cpp_properties.json
    build/
    ```

2. When this pop-up appears:

     ![Use-bundled](img/use-bundled-arduino-cli.png?raw=true "Use bundled arduino-cli")

   click "Use bundled arduino-cli".
