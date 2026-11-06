# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

## Program:
```
# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load the Face Image
faceImage = cv2.imread('pragatheeshraaj.jpeg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")
```

<img width="442" height="432" alt="image" src="https://github.com/user-attachments/assets/b11926f9-1685-4106-968e-9ceed54911bf" />


```
faceImage.shape
```
(1600, 907, 3)
```
# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
glassPNG = cv2.imread('sunglass-png-aviator-sunglass-png-clipart-3381.png',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")
```

<img width="623" height="282" alt="image" src="https://github.com/user-attachments/assets/07a80840-e487-4faa-8499-5a52b5daf63d" />


```
# Resize the image to fit over the eye region
glassPNG = cv2.resize(glassPNG,(720, 800))
print("image Dimension ={}".format(glassPNG.shape))
```
image Dimension =(800, 720, 4)
```
# Separate the Color and alpha channels
glassBGR = glassPNG[:,:,0:3]
glassMask1 = glassPNG[:,:,3]

# Display the images for clarity
plt.figure(figsize=[13,40])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');
```
<img width="851" height="457" alt="image" src="https://github.com/user-attachments/assets/5db064b4-ef57-4915-a462-9abe463ea912" />


```
# Make a copy of the face image
faceWithGlassesNaive = faceImage.copy()

# Resize sunglasses to a bigger size
glass_resized = cv2.resize(glassBGR, (320, 80))

# Position sunglasses over both eyes
# Position sunglasses over both eyes
x = 295
y = 325

h, w = glass_resized.shape[:2]

# Overlay sunglasses on the face
faceWithGlassesNaive[y:y+h, x:x+w] = glass_resized

# Display the result
plt.figure(figsize=(6, 8))
plt.imshow(cv2.cvtColor(faceWithGlassesNaive, cv2.COLOR_BGR2RGB))
plt.title("Face with Big Sunglasses")
plt.axis("off")
plt.show()
```
<img width="576" height="608" alt="image" src="https://github.com/user-attachments/assets/816bc981-330f-497c-b7d7-17a1c3bd8754" />


```


import cv2
import numpy as np
import matplotlib.pyplot as plt

# Set bigger sunglasses size and position
x, y = 295, 325
w, h = 320, 80

# Resize sunglasses and mask
glass_resized = cv2.resize(glassBGR, (w, h))
mask_resized = cv2.resize(glassMask1, (w, h))

# Convert mask to binary values: 0 or 1
mask_resized = np.uint8(mask_resized > 0)

# Create a 3-channel mask
glassMask = cv2.merge((mask_resized, mask_resized, mask_resized))

# Make a copy of the face image
faceWithGlassesArithmetic = faceImage.copy()

# Extract the eye region
eyeROI = faceWithGlassesArithmetic[y:y+h, x:x+w]

# Check dimensions
assert eyeROI.shape == glass_resized.shape == glassMask.shape

# Mask the eye region
maskedEye = cv2.multiply(eyeROI, 1 - glassMask)

# Mask the sunglasses
maskedGlass = cv2.multiply(glass_resized, glassMask)

# Combine both regions
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Place sunglasses on the face
faceWithGlassesArithmetic[y:y+h, x:x+w] = eyeRoiFinal

# Display results
plt.figure(figsize=(15, 8))

plt.subplot(131)
plt.imshow(cv2.cvtColor(maskedEye, cv2.COLOR_BGR2RGB))
plt.title("Masked Eye Region")
plt.axis("off")

plt.subplot(132)
plt.imshow(cv2.cvtColor(maskedGlass, cv2.COLOR_BGR2RGB))
plt.title("Masked Sunglasses")
plt.axis("off")

plt.subplot(133)
plt.imshow(cv2.cvtColor(faceWithGlassesArithmetic, cv2.COLOR_BGR2RGB))
plt.title("Face with Big Sunglasses")
plt.axis("off")

plt.show()

```

<img width="837" height="455" alt="image" src="https://github.com/user-attachments/assets/bfc980f1-6c1e-46bb-a98d-d982a4905326" />




```
# Replace the eye ROI with the output from the previous section
faceWithGlassesArithmetic = faceImage.copy()

# Use the same coordinates as the previous section
x, y = 295, 325
h, w = eyeRoiFinal.shape[:2]

# Place sunglasses over both eyes
faceWithGlassesArithmetic[y:y+h, x:x+w] = eyeRoiFinal

# Display the final result
plt.figure(figsize=(12, 8))

plt.subplot(121)
plt.imshow(cv2.cvtColor(faceImage, cv2.COLOR_BGR2RGB))
plt.title("Original Image")
plt.axis("off")

plt.subplot(122)
plt.imshow(cv2.cvtColor(faceWithGlassesArithmetic, cv2.COLOR_BGR2RGB))
plt.title("Face with Big Sunglasses")
plt.axis("off")

plt.tight_layout()
plt.show()
```
<img width="847" height="693" alt="image" src="https://github.com/user-attachments/assets/a277e3f1-eac7-4190-b50a-1f481106ef44" />

## Output:

<<img width="847" height="693" alt="image" src="https://github.com/user-attachments/assets/f6389b8d-9b31-462d-a39b-50babb13aac1" />



## Result:

Thus Adding Sunglasses to Your Passport Photo Using OpenCV is done successfully.
