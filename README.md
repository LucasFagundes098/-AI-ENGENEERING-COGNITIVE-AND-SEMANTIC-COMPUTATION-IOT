# -AI-ENGENEERING-COGNITIVE-AND-SEMANTIC-COMPUTATION-IOT
Nac01 - Python e OpenCV

------------------------------------------------------------------------------


%matplotlib inline
import cv2
from matplotlib import pyplot as plt
import numpy as np
import math

img = cv2.imread('circulo.PNG')

img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
img_hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)


## Definindo as máscaras
#Bolinha 01<br />
image_lower_hsv01 = np.array([86, 166, 226])  
image_upper_hsv01 = np.array([86, 166, 226])

mask_hsv01 = cv2.inRange(img_hsv, image_lower_hsv01, image_upper_hsv01)

#Bolinha 02<br />
image_lower_hsv02 = np.array([0, 239, 177])  
image_upper_hsv02 = np.array([0, 239, 177])

mask_hsv02 = cv2.inRange(img_hsv, image_lower_hsv02, image_upper_hsv02)<br />
#------------------------------------------------<br />


#rev4.x<br />
contornos01, _ = cv2.findContours(mask_hsv01, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)<br />
contornos02, _ = cv2.findContours(mask_hsv02, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)<br />
#_,contornos, _ = cv2.findContours(mask_hsv, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)<br />

## definindo contornos<br />
mask_rgb01 = cv2.cvtColor(mask_hsv01, cv2.COLOR_GRAY2RGB)<br />
mask_rgb02 = cv2.cvtColor(mask_hsv02, cv2.COLOR_GRAY2RGB) <br />

contornos_img = mask_rgb01.copy() <br />

cv2.drawContours(contornos_img, contornos01, -1, [255, 0, 0], 5);<br />
cv2.drawContours(contornos_img, contornos02, -1, [255, 0, 0], 5);<br /><br /><br />

## Valores do centro da massa<br /><br />
#1<br /><br />
cnt01 = contornos01[0]<br /><br />
M1 = cv2.moments(cnt01)<br /><br />

cx1 = int(M1['m10']/M1['m00'])<br /><br />
cy1 = int(M1['m01']/M1['m00'])<br /><br />

size = 20<br /><br />
color = (128,128,0)<br /><br />

cv2.line(contornos_img,(cx1 - size,cy1),(cx1 + size,cy1),color,5)<br /><br />
cv2.line(contornos_img,(cx1,cy1 - size),(cx1, cy1 + size),color,5)<br /><br />

#2<br />
cnt02 = contornos02[0]<br />
M2 = cv2.moments(cnt02)<br />

cx2 = int(M2['m10']/M2['m00'])<br />
cy2 = int(M2['m01']/M2['m00'])<br />

cv2.line(contornos_img,(cx2 - size,cy2),(cx2 + size,cy2),color,5)<br />
cv2.line(contornos_img,(cx2,cy2 - size),(cx2, cy2 + size),color,5)<br />

## Definindo a fonte<br />
font = cv2.FONT_HERSHEY_SIMPLEX<br />
#1<br />
text1 = cy1 , cx1<br />
origem1 = (20,40)<br />
#2<br />
text2 = cy2 , cx2<br />
origem2 = (710,380)<br />

cv2.putText(contornos_img, str(text1), origem1, font,1,(200,50,0),2,cv2.LINE_AA)<br />
cv2.putText(contornos_img, str(text2), origem2, font,1,(200,50,0),2,cv2.LINE_AA)<br />
cv2.line(contornos_img, (cx1,cy1) ,( cx2,cy2), (255,0,0), 10)<br />

## Angulo de inclinação<br />

angulo_inclinacao = math.degrees(math.atan2((cx1 - cx2), (cy1 - cy2))) <br />

#plotando<br />
plt.figure(figsize=(8,6))<br />
plt.imshow(img_rgb)<br />
plt.figure(figsize=(8,6))<br />
#plotar<br />
plt.imshow(contornos_img);<br />
