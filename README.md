# teams_meet_michrophone_fix

![banner](https://github.com/user-attachments/assets/7166869f-7044-4e72-95f0-6dddc45b82b1)

## Michrophone fix for Microsoft Teams and Goofle Meet

The point of this script is to work around an annoying problem that seems to occur with all major video conferencing web services on Linux, the automatic setting of the microphone volume. <br />
Although this function seems theoretically deactivable, it is not, and the system microphone volume ends up systematically lowered to a level low enough to make it possible to use it. <br />
With this script it does not matter if the application you are using will try to lower the microphone volume. <br />

## Usage
Replace:

    alsa_input.pci-0000_00_1b.0.analog-stereo

With your input device, you can find it here:

    pacmd list
    
Now simply start the application in loop mode with the desired microphone volume (0 to 100) as argument.

### Script 

```
#!/bin/bash

#############################################################################################################################################################################
#███████ ██    ██ ███    ██  ██████ ████████ ██  ██████  ███    ██ ███████
#██      ██    ██ ████   ██ ██         ██    ██ ██    ██ ████   ██ ██
#█████   ██    ██ ██ ██  ██ ██         ██    ██ ██    ██ ██ ██  ██ ███████
#██      ██    ██ ██  ██ ██ ██         ██    ██ ██    ██ ██  ██ ██      ██
#██       ██████  ██   ████  ██████    ██    ██  ██████  ██   ████ ███████
#############################################################################################################################################################################
HELP() # SHOW HELP MESSAGE
{
    echo "This script changes the microphone volume in a loop to ensure that external applications do not interfere."
    echo
    echo "Options:"
    echo "-h --help            Show this help message"
    echo ""
    echo "-l --loop            The microphone changes volume in a loop every 0.1 seconds with the input volume"
    echo "                     Syntax ./microphone.sh 100"
    echo ""
}

LOOP()
{
    FULL=65536
    VOLUME=$1
    VALUE_TEMP=$(( 65500 / 100 ))
    VALUE=$(( VALUE_TEMP * VOLUME ))

    while true
    do
      pacmd set-source-volume alsa_input.pci-0000_00_1b.0.analog-stereo $VALUE
      sleep 0.1
    done
}
#############################################################################################################################################################################
#███    ███  █████  ██ ███    ██
#████  ████ ██   ██ ██ ████   ██
#██ ████ ██ ███████ ██ ██ ██  ██
#██  ██  ██ ██   ██ ██ ██  ██ ██
#██      ██ ██   ██ ██ ██   ████
#############################################################################################################################################################################
if [ ! -z "$1" ] && [ ! -z "$2" ] && [ -z "$3" ];
then
    case $1 in

    -h|--help)
        HELP
        ;;

    -l|--loop)
        LOOP $2
        ;;

    *)
        echo "Use -h or --help to get the list of valid options and know what the program does"
        ;;
    esac

else
    echo "Use -h or --help to get the list of valid options and know what the program does"
fi
#############################################################################################################################################################################
```
