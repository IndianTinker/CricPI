#!/usr/bin/env python

import requests
from bs4 import BeautifulSoup
import time

import Adafruit_Nokia_LCD as LCD
import Adafruit_GPIO.SPI as SPI

from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

DC = 23
RST = 24
SPI_PORT = 0
SPI_DEVICE = 0

# Hardware SPI usage:
disp = LCD.PCD8544(DC, RST, spi=SPI.SpiDev(SPI_PORT, SPI_DEVICE, max_speed_hz=400000))


# Initialize library.
disp.begin(contrast=60)

# Clear display.
disp.clear()
disp.display()

# Create blank image for drawing.
# Make sure to create image with mode '1' for 1-bit color.
image = Image.new('1', (LCD.LCDWIDTH, LCD.LCDHEIGHT))

# Get drawing object to draw on image.
draw = ImageDraw.Draw(image)

# Draw a white filled box to clear 
draw.rectangle((0,0,LCD.LCDWIDTH,LCD.LCDHEIGHT), outline=255, fill=255)
font = ImageFont.truetype('/home/pi/Minecraftia-Regular.ttf', 8)

url="" #PutURL here
r=requests.get(url)
data=r.text
soup=BeautifulSoup(data)

#We have the gold lets process it now

play=soup.title.text
team1=play[0:play.find("Vs")-1]
team2=play[play.find("Vs")+len("Vs")+1:play.find("LIVE")-1]
string1=team1[0:3] + " vs "+ team2[0:3]
#print string1 #print from 12 row
draw.text((20,0), string1, font=font)
#draw.text((10,38),'Adafruit', font=font)


score1=soup.find("div",{"id":"firstInnings_tabber"}).text
score2=soup.find("div",{"id":"secondInnings_tabber"})
matchStatus=soup.find("p",{"id":"match_status"}).text
current=soup.find("a",{"class":"current"}).text
matchStatus=matchStatus[matchStatus.find("Match Status: ")+len("Match Status: "):len(matchStatus)]

if (matchStatus=="Match Ended"):
	matchResult=soup.find("p",{"id":"match_result"}).text
	#print matchResult
	string5=matchStatus
else:
	string5=matchStatus
	
#print current
string2=current[0:3] 
#print string2
draw.text((4,8), string2, font=font)

#print matchStatus 
#print score1
#print score2

if (score2==None):
	#print "Second Innings not started/ Fetch failed"
	innings=1
else:
	#print "Second innings ACTIVE"
	innings=2
	score2=score2.text
	
#ScoreParser
def parseScore(score):
	ini={'Score':'0/0','Overs':'0','R/R':'0','Fours':'0','Sixes':'0','Extras':'0'}
	ini['Score']=score[0:score.find("Overs")]
	ini['Overs']=score[score.find("Overs")+len("Overs"):score.find("R/R")]
	ini['R/R']=score[score.find("R/R")+len("R/R"):score.find("Fours")]
	ini['Fours']=score[score.find("Fours")+len("Fours"):score.find("Sixes")]
	ini['Sixes']=score[score.find("Sixes")+len("Sixes"):score.find("Extras")]
	ini['Extras']=score[score.find("Extras")+len("Extras"):len(score)+1]
	return ini
if (innings==1):
	scoret1=parseScore(score1.encode())
else:
	scoret1=parseScore(score1.encode())
	scoret2=parseScore(score2.encode())

if (innings==1):
	#print scoret1
	overs=scoret1['Overs']
	score=scoret1['Score']
	string3="4:"+scoret1['Fours']+" 6:"+scoret1['Sixes']+"  RR:"+scoret1['R/R']
	string4="IInd Ings PENDING"
else:
	#print scoret1
	#print scoret2
	overs=scoret2['Overs']
	score=scoret2['Score']
	string3="4:"+scoret2['Fours']+"  6:"+scoret2['Sixes']+"  RR:"+scoret2['R/R']
	string4="I: "+scoret1['Score']+" of "+scoret1['Overs']

#print overs
draw.text((4,16),overs, font=font)
#print score
font1 = ImageFont.truetype('/home/pi/Minecraftia-Regular.ttf', 11)
draw.text((28,10), score, font=font1)
#print string3
draw.text((4,25),string3, font=font)
#print string4
font2 = ImageFont.truetype('/home/pi/Minecraftia-Regular.ttf', 7)
draw.text((4,34),string4, font=font2)
#print string5
draw.text((4,40),string5, font=font2)

disp.image(image)
disp.display()

#time.sleep(10)
