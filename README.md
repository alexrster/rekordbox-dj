# <b>Rekordbox DJ</b> – MIDI mappings

## Installation

Mapping files should be applied sequentially!

## Mapping file columns

| #  | Column Name | Format | Description |
| -- | ----------- | ------ | ----------- |
| 1  | name        | string | Mapping name (doesn't affect anything) |
| 2  | function    | functionName (string) | Rekordbox DJ function name (could include '.') |
| 3  | type        | typeName | Control type (see <u>details below</u>) |
| 4  | input       | int | MIDI code to be received from INPUT (for activating the function) |
| 5  | &nbsp;&nbsp;&nbsp;deck1       | int | MIDI code to be received from INPUT for Deck 1, or '0', or _emtpy_ |
| 6  | &nbsp;&nbsp;&nbsp;deck2       | int | MIDI code to be received from INPUT for Deck 2, or '1', or _emtpy_ |
| 7  | &nbsp;&nbsp;&nbsp;deck3       | int | MIDI code to be received from INPUT for Deck 3, or '2', or _emtpy_ |
| 8  | &nbsp;&nbsp;&nbsp;deck4       | int | MIDI code to be received from INPUT for Deck 4, or '3', or _emtpy_ |
| 9  | output      | int | MIDI code to be sent to OUTPUT (on activating the function) |
| 10 | &nbsp;&nbsp;&nbsp;deck1       | int | MIDI code to be sent to OUTPUT for Deck 1, or '0', or _emtpy_ |
| 11 | &nbsp;&nbsp;&nbsp;deck2       | int | MIDI code to be sent to OUTPUT for Deck 2, or '1', or _emtpy_ |
| 12 | &nbsp;&nbsp;&nbsp;deck3       | int | MIDI code to be sent to OUTPUT for Deck 3, or '2', or _emtpy_ |
| 13 | &nbsp;&nbsp;&nbsp;deck4       | int | MIDI code to be sent to OUTPUT for Deck 4, or '3', or _emtpy_ |
| 14 | option      | optionName | Function-specific options (see <u>details below</u>) |
| 15 | comment     | string | – Comma-separated Function-specific parameters (see <u>details below</u>)<br/>– User-defined comment |

### CSV

This is the definition of the Sync function mapped to a button in a DDJ-1000 controller. Every needed data is separated by commas and always follows the same order. There must be always 14 commas, and you can put the info between them, but there is no need to fill every space between commas as some data is not required or necessary with all the mappable functions.

The data must be set the following way:

* In the first two spaces between commas you have write the name of the command and the function which controls. With many commands it’s the same word, or you can even write nothing in the second space.
* The third space is for setting the type of control you are going to use. “Button”, “Rotary”, “KnobSlider”, “KnobSliderHiRes” is the usual info you’ll see here.
* The following space is for writing down the MIDI command in hex format. This is the input command.
* The following 4 spaces are for setting the MIDI channels for the MIDI command, but you only have to write something here if it’s a command that needs different MIDI channels for each deck. Remember one thing: MIDI channels are expressed subtracting -1 to the number you want to write, so if you want to set MIDI channel number 1, you have to write 0.
* The following space is for the output MIDI command, write only a command here if you want  to control led lightning or some kind of indicator. As in the input command, the following four spaces are for setting the different MIDI channels for each deck just in case the command needs that.
* The following space is for options. You can set more than one option if you separate them using “;”. I only know how to use some options and I’ll explain them later in the article.
* The last space is just for commenting, you can write whatever you want here.
* If you look into a .csv Rekordbox mapping file you’ll see a lot of lines starting with a “#” symbol. That symbol indicates that the full line is a comment and has no effect on the mapping function.

### MIDI functions

* **FX1-1Select.Down**: Rekordbox only allows end users to map the command that let’s you move UP in the FX list, but this one let’s move down in the same list. Of course it can be used as **FX2-1Select.Down** for the FX bank 2.
* **FX1-1Select.FXNAME**: This command allows direct selection of an FX writing down in capital letters the name of the FX after the dot, and eliminating spaces in the name of the FX. So if you want to select Enigma Jet in FX bank 2 you have to write **FX2-1Select.ENIGMAJET**.
* **FX1-1On.ECHO**: Selects AND activates an FX, as the previous command you have to write the name of the FX after the dot in capital letters and without spaces. This command does not work for the RMX expansion FXs.
* **FX1-1Momentary**: Enables the FX unit only while you are pushing the mapped button or pad.
* **FX1AssignSampler**, **FX1AssignMaster**: Assigns the FX unit to the Master or Sampler, this of course can be used writing **FX2** instead of **FX1**.
* **FX1Assign.MASTER**, **FX1Assign.MIC**, **FX1Assign.LEFT**, **FX1Assign.RIGHT**: Work in a similar way as the previous commands (left and right are fort left and right assigned crossfader channels) but behave a bit different because those commands need to be mapped to a button that acts like a switch.
* **SFX1-1On**, **SFX1-1Select**, **SFX1-Touch**, **SFX1-Slider**: Those commands are for the Slide FX that can be controlled with the DDJ-XP1. I have tried to use them with no luck, I think that Rekordbox looks for a real DDJ-XP1 to be connected. I’ve tried to trick the software using a  virtual MIDI port with the same name than de DDJ-XP1 MIDI port but no luck again, I think that Rekordbox sends sysex data to the controller and waits for a response to know if there is a real XP1 connected.

### Command options

You can set some options at the penultimate space between commas, I have decoded some of those options. If you want to set more than one option you will need to separate them using “;”.

* **RO**: this option hides  the command to be shown in the mapping utility. Some commands are hidden by default and this option is not needed.
* **Min=x**: changing the X with a number from 0 to 127 you set the minimum CC or  velocity value that the command needs to be executed.
* **Max=x**: changing the X with a number from  0 to 127 you set the maximum CC or velocity value that the command will accept to be executed.
* **Value=x**: not sure about this one, some commands seem to accept this option for some led light indicators, but I’m completely sure of how to make us of it. Try to experiment.
* **Blink=x**: not sure again about this one, but it’s always used with led light commands. It sets the blink speed of an indicator, but I’m not sure about the values that are valid. You can also experiment with this one.  

As I explained in the previous section, the **FX1-1On.FXNAME** command does not work with the RMX expansion pack effects, but there is a trick to make something similar. You can edit (manually from an editor, not with Rekordbox) in the **FX1-1Select** command the **Button** and write **KnobSlider**, and that command will let you use a knob or a fader to select the FX.

## References

- https://djtechtools.com/2019/06/24/hacking-rekordbox-fx-and-adding-rmx-1000-control/
