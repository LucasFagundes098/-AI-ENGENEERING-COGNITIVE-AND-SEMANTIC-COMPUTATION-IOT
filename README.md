# -AI-ENGENEERING-COGNITIVE-AND-SEMANTIC-COMPUTATION-IOT
Nac01 - Python e OpenCV


%matplotlib inline
import cv2
from matplotlib import pyplot as plt
import numpy as np
import math

img = cv2.imread('circulo.PNG')

img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
img_hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)


# Definindo as máscaras
#Bolinha 01
image_lower_hsv01 = np.array([86, 166, 226])  
image_upper_hsv01 = np.array([86, 166, 226])

mask_hsv01 = cv2.inRange(img_hsv, image_lower_hsv01, image_upper_hsv01)

#Bolinha 02
image_lower_hsv02 = np.array([0, 239, 177])  
image_upper_hsv02 = np.array([0, 239, 177])

mask_hsv02 = cv2.inRange(img_hsv, image_lower_hsv02, image_upper_hsv02)
#------------------------------------------------


#rev4.x
contornos01, _ = cv2.findContours(mask_hsv01, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
contornos02, _ = cv2.findContours(mask_hsv02, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
#_,contornos, _ = cv2.findContours(mask_hsv, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

#definindo contornos
mask_rgb01 = cv2.cvtColor(mask_hsv01, cv2.COLOR_GRAY2RGB)
mask_rgb02 = cv2.cvtColor(mask_hsv02, cv2.COLOR_GRAY2RGB) 

contornos_img = mask_rgb01.copy() 

cv2.drawContours(contornos_img, contornos01, -1, [255, 0, 0], 5);
cv2.drawContours(contornos_img, contornos02, -1, [255, 0, 0], 5);

## Valores do centro da massa
#1
cnt01 = contornos01[0]
M1 = cv2.moments(cnt01)

cx1 = int(M1['m10']/M1['m00'])
cy1 = int(M1['m01']/M1['m00'])

size = 20
color = (128,128,0)

cv2.line(contornos_img,(cx1 - size,cy1),(cx1 + size,cy1),color,5)
cv2.line(contornos_img,(cx1,cy1 - size),(cx1, cy1 + size),color,5)

#2
cnt02 = contornos02[0]
M2 = cv2.moments(cnt02)

cx2 = int(M2['m10']/M2['m00'])
cy2 = int(M2['m01']/M2['m00'])

cv2.line(contornos_img,(cx2 - size,cy2),(cx2 + size,cy2),color,5)
cv2.line(contornos_img,(cx2,cy2 - size),(cx2, cy2 + size),color,5)

# Definindo a fonte
font = cv2.FONT_HERSHEY_SIMPLEX
#1
text1 = cy1 , cx1
origem1 = (20,40)
#2
text2 = cy2 , cx2
origem2 = (710,380)

cv2.putText(contornos_img, str(text1), origem1, font,1,(200,50,0),2,cv2.LINE_AA)
cv2.putText(contornos_img, str(text2), origem2, font,1,(200,50,0),2,cv2.LINE_AA)
cv2.line(contornos_img, (cx1,cy1) ,( cx2,cy2), (255,0,0), 10)

# Angulo de inclinação

angulo_inclinacao = math.degrees(math.atan2((cx1 - cx2), (cy1 - cy2)))  


#plotando
plt.figure(figsize=(8,6))
plt.imshow(img_rgb)
plt.figure(figsize=(8,6))
#plotar
plt.imshow(contornos_img);
