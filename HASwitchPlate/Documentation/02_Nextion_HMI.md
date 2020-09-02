# Nextion HMI

## Basic first-time use

For most users the deployment of the Nextion firmware is straightforward - simply copy [the compiled firmware image](../Nextion_HMI/HASwitchPlate.tft) to a [FAT32-formatted microSD card](https://nextion.tech/faq-items/using-nextion-microsd/), insert the microSD card into the Nextion LCD, and then apply 5VDC to the power pins on the panel.  It should power up the panel, recognize the .TFT file, and update the panel automatically.  Once the firmware update has completed you can remove power and eject the microSD card.

Compiled TFT files are included for the [Basic](https://github.com/aderusha/HASwitchPlate/raw/master/Nextion_HMI/HASwitchPlate.tft) and [Enhanced](https://github.com/aderusha/HASwitchPlate/raw/master/Nextion_HMI/HASwitchPlate-Enhanced.tft) versions of the panel.  This project does not currently utilize any features offered in the Enhanced panel, and the Enhanced device does not fit in the provided enclosure.  **It is strongly recommended that you do not use the enhanced display**.

Once the project is assembled, future updates to the Nextion firmware can be handled over-the-air by utilizing the built-in web interface or by [issuing an MQTT command](06_MQTT_Control.md#command-syntax) with a URL to the target TFT file.

With the Nextion firmware flashed you can proceed to the [Electronics Assembly section](03_Electronics_Assembly.md) to test the assembled system.

---

## Nextion Editor

For advanced customization you will need to download the (Windows-only) [Nextion editor](https://nextion.tech/nextion-editor/).  You can find instructions on its use [here](https://www.itead.cc/blog/nextion-editor-a-basic-introduction).

If you want to edit the existing HASP interface, you will likely need to delete some pages in order to add your own work as the project consumes nearly the entire memory space of the Nextion basic panel.  If you'd like to start a new interface from the ground up, copy over the existing Page 0 page as the ESP8266 firmware makes use of page 0 for user interactions and WiFi setup.  The Home Assistant automations will probably need to be changed if any major modifications are made to the HMI.

## Nextion Page and Object IDs

Objects are referenced by page number (*not page name*) and object ID (*not object name*) as shown in the Nextion editor.  The Nextion notation for each object is of the form `p[1].b[2]` meaning page number 1, object ID 2.  Confusingly, button object *names* will start at `b0` but it might be object ID 2, or 7, etc. Other objects will be named similarly with different letters.  For example, the first text field on the page will be automatically named `t0`.  **Ignore these names**.  They have nothing to do with the object ID, and all objects regardless of type are still referenced as `p[<page number>].b[<object id>]`

![Page and Object IDs](https://github.com/aderusha/HASwitchPlate/blob/master/Documentation/Images/Nextion_Editor_Page_and_Object_Ids.png?raw=true)

Screenshots of each object number on each page of the HASP project can be found in the [Nextion HMI documentation section](02_Nextion_HMI.md#hasp-nextion-object-reference).

An object will have multiple attributes, some of which can be changed.  For example, a button named `p[1].b[2]` may have a "txt" attribute which we'd refer to as `p[1].b[2].txt`.  Not all attributes can be changed at runtime, the ones which can are shown in a green font in the Nextion editor (see the right pane of the image above.)

## Nextion Instruction Set

The Nextion accepts and sends commands over the serial interface.  Your Home Automation system will send Nextion instructions wrapped in an MQTT message to be delivered by HASP to your display.  A detailed guide to the Nextion instruction set can be found [here](https://nextion.tech/instruction-set/).

## Nextion color codes

The Nextion environment utilizes RGB 565 encoding.  [Use this handy convertor](https://nodtem66.github.io/nextion-hmi-color-convert/index.html) to select your colors and convert to the RGB 565 format.

## HASP Nextion Object Reference

| Page 0 | Pages 1-3 | Pages 4-5 |
|--------|-----------|-----------|
| ![Page 0](Images/NextionUI_p0_Init_Screen.png?raw=true) | ![Pages 1-3](Images/NextionUI_p1-p3_4buttons.png?raw=true) | ![Pages 4-5](Images/NextionUI_p4-p5_3sliders.png?raw=true) |

| Page 6 | Page 7 | Page 8 |
|--------|--------|--------|
| ![Page 6](Images/NextionUI_p6_8buttons.png?raw=true) | ![Page 7](Images/NextionUI_p7_12buttons.png?raw=true) | ![Page 8](Images/NextionUI_p8_5buttons+1slider.png?raw=true) |

| Page 9 | Page 10 | Page 11 |
|--------|---------|---------|
| ![Page 9](Images/NextionUI_p9_9buttons.png?raw=true) | ![Page 10](Images/NextionUI_p10_5buttons.png?raw=true) | ![Page 11](Images/NextionUI_p11_1button.png?raw=true)

## HASP Default Fonts

The Nextion display supports monospaced and proportional fonts.  For proportional fonts, the HASP project includes [Consolas](https://docs.microsoft.com/en-us/typography/font-list/consolas) in 4 sizes and [Webdings](https://en.wikipedia.org/wiki/Webdings#Character_set) in 1 size.

| Number | Font              | Max characters per line | Max lines per button |
|--------|-------------------|-------------------------|----------------------|
| 0      | Consolas 24 point | 20 characters           | 2 lines              |
| 1      | Consolas 32 point | 15 characters           | 2 lines              |
| 2      | Consolas 48 point | 10 characters           | 1 lines              |
| 3      | Consolas 80 point | 6 characters            | 1 lines              |
| 4      | Webdings 56 point | 8 characters            | 1 lines              |

The HASP also includes [Google's "Noto Sans"](https://github.com/googlefonts/noto-fonts) proportional font in 5 sizes.  These fonts also have the "[FontAwesome](https://fontawesome.com/cheatsheet)" icon set which can be mixed with normal ASCII text.

| Number | Font                       |
|--------|----------------------------|
| 5      | Noto Sans Regular 24 point |
| 6      | Noto Sans Regular 32 point |
| 7      | Noto Sans Regular 48 point |
| 8      | Noto Sans Regular 64 point |
| 9      | Noto Sans Regular 80 point |
| 10     | Noto Sans Bold 80 point    |

![HASP Fonts 0-3](Images/NextionUI_Fonts_0-3.png?raw=true) ![HASP Fonts 4-7](Images/NextionUI_Fonts_4-7.png?raw=true) ![HASP Fonts 8-10](Images/NextionUI_Fonts_8-10.png?raw=true)

## Using FontAwesome Icons

FontAwesome ("FA") icons are now included with HASP in fonts 6-10 (`Noto Sans` fonts).  To use these, open the [FontAwesome Cheat Sheet](https://fontawesome.com/cheatsheet), browse to any of the "Solid", "Regular", or "Brands" pages, highlight the icon in your browser, and then paste the result into your automation or the Nextion editor.  The result in your editor should look like this: 

When sent to your LCD with the appropriate font selected (again, only fonts 6-10), the icon should show up as seen on the Cheat Sheet.  You can also mix text and icons in the same string.

![Using FontAwesome Icons](Images/HASP-Icons.gif?raw=true)

## HOW TO: Run this software with an ESP8266 only

One feature of the Nextion Editor is the [Nextion Simulator](https://www.itead.cc/wiki/Nextion_Editor_Quick_Start_Guide#Debug.2C_online_simulator), which allows the user to debug an HMI being edited.  You've probably used this if you've worked on editing your own HMI file.  You can run through the screens using your mouse to issue touch commands and feed it commands and see output in the text boxes provided.

The Nextion simulator allows you to run [hardware-in-loop](https://en.wikipedia.org/wiki/Hardware-in-the-loop_simulation) with your microcontroller.  If you have a second USB UART kicking around (you could use a second WeMos or Arduino if you have one handy), connect the UART RX to pin D4 and the TX to pin D7 which are the pins you'd use to hook up a real panel.  In the Nextion Simulator you can then select "User MCU Input", select your UARTs COM port, set the Baud to 115200, then click Start.

Now the Simulator will accept input from and send output to your flashed ESP8266 without having a Nextion on hand!

![Nextion Editor Simulator](Images/Nextion_Editor_Simulator.png?raw=true)
