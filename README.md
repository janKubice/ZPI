# ZPIfrom m5stack import *
from m5ui import *
from uiflow import *
import unit
from m5stack import machine

def change_state(pin):
  global swap
  swap = not swap

setScreenColor(0x222222)
delay = 1
relay = unit.get(unit.RELAY, unit.PORTA)
earth = unit.get(unit.EARTH, unit.PORTB)

bBtn = machine.Pin(38, machine.Pin.IN)

swap = True
relayMode = False

bBtn.irq(trigger=machine.Pin.IRQ_FALLING, handler=change_state)

while True:
  lcd.clear()
  setScreenColor(0x222222)
  
  if earth.analogValue > 1:
    setScreenColor(0x123456)
    speaker.setVolume(1)
    speaker.tone(1800,200)
  else:
    speaker.setVolume(0)
  
  if btnA.isPressed():
    setScreenColor(0x666666)
    relay.on()
    time.sleep(delay)
    relay.off()

  if swap:
    time.sleep(delay)
    if relayMode == True:
      relay.off()
      relayMode = False
    else:
      relay.on()
      relayMode = True
