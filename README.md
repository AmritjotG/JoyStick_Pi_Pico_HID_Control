# JoyStick_Pi_Pico_HID_Control
Utilized a raspberry pi pico to interface with any computer such that a joystick could be used as an input device replacement for a mouse. The code is based in Circuit Python, a subset of python that can make use of the HID Library by AdaFruit, this library is what allows for the input information to be turned into if statements and outputted as the relevant movements in the computer




import time
import analogio
import board
import digitalio
import usb_hid
from adafruit_hid.mouse import Mouse
 
mouse = Mouse(usb_hid.devices)
 
x_axis = analogio.AnalogIn(board.A0)
y_axis = analogio.AnalogIn(board.A1)
 
btn_left = digitalio.DigitalInOut(board.GP18)
btn_left.direction = digitalio.Direction.INPUT
btn_left.pull = digitalio.Pull.DOWN
 
btn_right = digitalio.DigitalInOut(board.GP19)
btn_right.direction = digitalio.Direction.INPUT
btn_right.pull = digitalio.Pull.DOWN
 
pot_min = 0.00
pot_max = 3.29
step = (pot_max - pot_min) / 20.0
 
 
def get_voltage(pin):
    return (pin.value * 3.3) / 65536
 
 
def steps(axis):
    return round((axis - pot_min) / step)
 
 
while True:
    x = get_voltage(x_axis)
    y = get_voltage(y_axis)
 
    if btn_left.value:
        mouse.click(Mouse.LEFT_BUTTON)
        time.sleep(0.2)
    if btn_right.value:
        mouse.click(Mouse.RIGHT_BUTTON)
        time.sleep(0.2)
 
         # print(steps(x))
    if steps(x) > 11.0:
        mouse.move(x=1)
    if steps(x) < 9.0:
        mouse.move(x=-1)
 
    if steps(x) > 19.0:
        mouse.move(x=8)
    if steps(x) < 1.0:
        mouse.move(x=-8)
 
         # print(steps(y))
    if steps(y) > 11.0:
        mouse.move(y=-1)
    if steps(y) < 9.0:
        mouse.move(y=1)
 
    if steps(y) > 19.0:
        mouse.move(y=-8)
    if steps(y) < 1.0:
        mouse.move(y=8)
