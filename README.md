# code_for_robot_forward-backwards-right-left. 

import serial
from decimal import *
import RPi.GPIO as GPIO
from time import sleep
import os
import time
import pyrebase

m1pin1=40;
m1pin2=38;
m2pin1=36;
m2pin2=35;
m3pin1=33;
m3pin2=31;
m4pin1=29;
m4pin2=32;
m5pin1=16;
m5pin2=18;
m6pin1=11;
m6pin2=13;

GPIO.setmode(GPIO.BOARD)
#wheel pin 
GPIO.setup(m1pin1,GPIO.OUT)
GPIO.setup(m1pin2,GPIO.OUT)
GPIO.setup(m2pin1,GPIO.OUT)
GPIO.setup(m2pin2,GPIO.OUT)
GPIO.setup(m3pin1,GPIO.OUT)
GPIO.setup(m3pin2,GPIO.OUT)
GPIO.setup(m4pin1,GPIO.OUT)
GPIO.setup(m4pin2,GPIO.OUT)
GPIO.setup(m5pin1,GPIO.OUT)
GPIO.setup(m5pin2,GPIO.OUT)
GPIO.setup(m6pin1,GPIO.OUT)
GPIO.setup(m6pin2,GPIO.OUT)

#from picamera import PiCamera
#camera = PiCamera()
#camera.resolution = (1920, 1080)
#camera.start_preview()
#print('Hello')

import pyrebase
config = {
  "apiKey": "5VjeYDr9BRkfkZg8b0JJ3P4sdL0JZtqsBmpfl2AU",

                  ECE/SoE/DSU                                                                                                                            Page 54 

  "authDomain": "logistics-and-delivery-robot-default-rtdb.firebaseio.com",

  "databaseURL": "https://logistics-and-delivery-robot-default-rtdb.firebaseio.com",

  "storageBucket": "logistics-and-delivery-robot"
}

firebase = pyrebase.initialize_app(config)
db = firebase.database()
print(db)

def find(str, ch):
    for i, ltr in enumerate(str):
        if ltr == ch:
            yield i

def gpsread():
        ser = serial.Serial('/dev/ttyUSB0', 9600,timeout=0.5)
        cd=1
        while cd <= 1:
            ck=0
            fd=""
            while ck <= 1:
                rcv = ser.readline(50)
                rcv = str(rcv)
                #print(rcv)
                fd=fd+rcv
                ck=ck+1
         
            if '$GPRMC' in fd:
                ps=fd.find('$GPRMC')
                dif=len(fd)-ps
                if dif > 50:
                    data=fd[ps:(ps+50)]
                    #print data
                    ds=data.find('A')
                    if ds > 0 and ds < 20:
                        p=list(find(data, ","))
                        lat=data[(p[2]+1):p[3]]
                        lon=data[(p[4]+1):p[5]]
         
                        s1=lat[2:len(lat)]
                        s1=Decimal(s1)
                        s1=s1/60
                        s11=int(lat[0:2])
                        s1=s11+s1
         
                        s2=lon[3:len(lon)]
                        s2=Decimal(s2)
                        s2=s2/60

                        s22=int(lon[0:3])
                        s2=s22+s2
                        cd=cd+1
                        #print s1
                        #print s2
                        s1=round(s1,6)
                        s2=round(s2,6)                        
                        #gps=str(s1)+'  '+str(s2)
                        
                        return s1,s2
                    
def forward():
    GPIO.output(m1pin1,1)
    GPIO.output(m1pin2,0)
    GPIO.output(m2pin1,1)
    GPIO.output(m2pin2,0)
    GPIO.output(m3pin1,1)
    GPIO.output(m3pin2,0)
    GPIO.output(m4pin1,1)
    GPIO.output(m4pin2,0)
    GPIO.output(m5pin1,1)
    GPIO.output(m5pin2,0)
    GPIO.output(m6pin1,1)
    GPIO.output(m6pin2,0)
    print('Forward')
    time.sleep(5)
    GPIO.output(m1pin1,0)
    GPIO.output(m1pin2,0)
    GPIO.output(m2pin1,0)
    GPIO.output(m2pin2,0)
    GPIO.output(m3pin1,0)
    GPIO.output(m3pin2,0)
    GPIO.output(m4pin1,0)
    GPIO.output(m4pin2,0)
    GPIO.output(m5pin1,0)
    GPIO.output(m5pin2,0)
    GPIO.output(m6pin1,0)
    GPIO.output(m6pin2,0)
    time.sleep(2)

def reverse():
    GPIO.output(m1pin1,0)
    GPIO.output(m1pin2,1)
    GPIO.output(m2pin1,0)
    GPIO.output(m2pin2,1)
    GPIO.output(m3pin1,0)
    GPIO.output(m3pin2,1)
    GPIO.output(m4pin1,0)
    GPIO.output(m4pin2,1)
    GPIO.output(m5pin1,0)
    GPIO.output(m5pin2,1)
    GPIO.output(m6pin1,0)
    GPIO.output(m6pin2,1)
    print('Reverse')
    time.sleep(5)
    GPIO.output(m1pin1,0)
    GPIO.output(m1pin2,0)
    GPIO.output(m2pin1,0)
    GPIO.output(m2pin2,0)
    GPIO.output(m3pin1,0)
    GPIO.output(m3pin2,0)
    GPIO.output(m4pin1,0)
    GPIO.output(m4pin2,0)
    GPIO.output(m5pin1,0)
    GPIO.output(m5pin2,0)
    GPIO.output(m6pin1,0)
    GPIO.output(m6pin2,0)
    time.sleep(2)

def stop():
    GPIO.output(m1pin1,0)
    GPIO.output(m1pin2,0)
    GPIO.output(m2pin1,0)
    GPIO.output(m2pin2,0)
    GPIO.output(m3pin1,0)
    GPIO.output(m3pin2,0)
    GPIO.output(m4pin1,0)
    GPIO.output(m4pin2,0)
    GPIO.output(m5pin1,0)
    GPIO.output(m5pin2,0)
    GPIO.output(m6pin1,0)
    GPIO.output(m6pin2,0)
    print('stop')
    time.sleep(0.5)

def right():
    print('right')
    GPIO.output(m2pin1,0)
    GPIO.output(m2pin2,1)
    GPIO.output(m3pin1,0)
    GPIO.output(m3pin2,1)
    GPIO.output(m6pin1,0)
    GPIO.output(m6pin2,1)
    
    GPIO.output(m1pin1,1)
    GPIO.output(m1pin2,0)
    GPIO.output(m4pin1,1)
    GPIO.output(m4pin2,0)
    GPIO.output(m5pin1,1)
    GPIO.output(m5pin2,0)
    
    time.sleep(5)
    GPIO.output(m1pin1,0)
    GPIO.output(m1pin2,0)
    GPIO.output(m2pin1,0)
    GPIO.output(m2pin2,0)
    GPIO.output(m3pin1,0)
    GPIO.output(m3pin2,0)
    GPIO.output(m4pin1,0)
    GPIO.output(m4pin2,0)
    GPIO.output(m5pin1,0)
    GPIO.output(m5pin2,0)
    GPIO.output(m6pin1,0)
    GPIO.output(m6pin2,0)
    time.sleep(2)

def left():
    print('left')
    GPIO.output(m1pin1,0)
    GPIO.output(m1pin2,1)
    GPIO.output(m4pin1,0)
    GPIO.output(m4pin2,1)
    GPIO.output(m5pin1,0)
    GPIO.output(m5pin2,1)
    
    GPIO.output(m2pin1,1)
    GPIO.output(m2pin2,0)
    GPIO.output(m3pin1,1)
    GPIO.output(m3pin2,0)
    GPIO.output(m6pin1,1)
    GPIO.output(m6pin2,0)

    time.sleep(5)
    GPIO.output(m1pin1,0)
    GPIO.output(m1pin2,0)
    GPIO.output(m2pin1,0)
    GPIO.output(m2pin2,0)
    GPIO.output(m3pin1,0)
    GPIO.output(m3pin2,0)
    GPIO.output(m4pin1,0)
    GPIO.output(m4pin2,0)
    GPIO.output(m5pin1,0)
    GPIO.output(m5pin2,0)
    GPIO.output(m6pin1,0)
    GPIO.output(m6pin2,0)
    time.sleep(2)
    

try:
    while True:
#         s1,s2= gpsread()
#         print(s1)
#         print(s2)
#         #print(type(float(s1)))
#         db.update({"Lat":float(s1),"Long":float(s2)})
        print("Reading Cloud")
        value = db.child("STATUS").get()
        status=value.val()
        print(status)
        if status=="1":
            forward()
        if status=="2":
            reverse()
        if status=="3":
            right()
        if status=="4":
            left()
        if status=="5":
            stop()

except KeyboardInterrupt:
    print('Stopped by user')
    GPIO.cleanup()
        
