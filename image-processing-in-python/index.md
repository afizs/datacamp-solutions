# Image Processing in Python
### Reducing Noise from the image
```python
# Import the module and function
from  skimage.util  import  random_noise

# Add noise to the image
noisy_image = random_noise(fruit_image)

# Show original and resulting image
show_image(fruit_image,  'Original')
show_image(noisy_image,  'Noisy image')
```
2. Reducing noise
```python
# Import the module and function
from  skimage.restoration  import  denoise_tv_chambolle

# Apply total variation filter denoising
denoised_image = denoise_tv_chambolle(noisy_image,
                                      multichannel=True)

# Show the noisy and denoised images
show_image(noisy_image,  'Noisy')
show_image(denoised_image,  'Denoised image')
```
3. Reducing noise while preserving edges

```python
# Import bilateral denoising function

from  skimage.restoration  import  denoise_bilateral

# Apply bilateral filter denoising
denoised_image = denoise_bilateral(landscape_image,
multichannel=True)

# Show original and resulting images
show_image(landscape_image,  'Noisy image')
show_image(denoised_image,  'Denoised image')
```

4. Unsupervised Segmentation 
```python
# Import the slic function from segmentation module
from skimage.segmentation import slic

# Import the label2rgb function from color module
from skimage.color import label2rgb

# Obtain the segmentation with 400 regions
segments = slic(face_image, n_segments= 400)

# Put segments on top of original image to compare
segmented_image = label2rgb(segments, face_image, kind='avg')

# Show the segmented image
show_image(segmented_image, "Segmented image, 400 superpixels")
```
5. Contouring shapes
```python
# Import the modules
from skimage import data, measure

# Obtain the horse image
horse_image = data.horse()

# Find the contours with a constant level value of 0.8
contours = measure.find_contours(horse_image, 0.8)

# Shows the image with contours found
show_image_contour(horse_image, contours)
```
6. Find contours of an image that is not binary
```python
# Make the image grayscale
image_dice = color.rgb2gray(image_dice)

# Obtain the optimal thresh value
thresh = filters.threshold_otsu(image_dice)

# Apply thresholding
binary = image_dice > thresh

# Find contours at a constant value of 0.8
contours = measure.find_contours(binary, 0.8)

# Show the image
show_image_contour(image_dice, contours)
```
7. Count the dots in a dice's image
```python
# Create list with the shape of each contour
shape_contours = [cnt.shape[0] for cnt in contours]

# Set 50 as the maximum size of the dots shape
max_dots_shape = 50

# Count dots in contours excluding bigger than dots size
dots_contours = [cnt for cnt in contours if np.shape(cnt)[0] < max_dots_shape]

# Shows all contours found 
show_image_contour(binary, contours)

# Print the dice's number
print("Dice's dots number: {}. ".format(len(dots_contours)))
```
