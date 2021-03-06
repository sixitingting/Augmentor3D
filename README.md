
## 3D Augmentation module examples

A module implemented for the purpose of augmenting 3D volumes.<br>
This class uses transformations in SimpleITK library to perform augmentations. 


```python
from augmentation3DUtil import Augmentation3DUtil
from augmentation3DUtil import Transforms
import SimpleITK as sitk
import matplotlib.pyplot as plt
%matplotlib inline
```

### Reading image using SimpleITK module


```python
img = sitk.ReadImage(fr"***Path to input image***")
```

### Defining the object 


au = Augmentation3DUtil(imgs,masks) <br>
imgs : list of images (here 3D volumes) to be augmented. Same transformation will be applied to each of these volumes. <br>
masks : Their corresponding binary segmentation masks (None if no masks to be augmented or provide them as list)

### Performing single transformations 

Use the Enum class Transforms for defining transforms. See examples below

#### Rotation
parameters
probability : probability of the executing this particular transformation <br>
degrees : degree of rotation. 


```python
au = Augmentation3DUtil([img],masks=None)
au.add(Transforms.ROTATE2D,probability = 1, degrees = 45)

imgs, augs = au.process(1)

arr = sitk.GetArrayFromImage(imgs[0][0])
augarr = sitk.GetArrayFromImage(augs[0][0][0])

plt.subplot(121)
plt.imshow(arr[10])
plt.axis('off')
plt.subplot(122)
plt.imshow(augarr[10])
plt.axis('off')
```




    (-0.5, 383.5, 383.5, -0.5)




![png](output_files/output_7_1.png)


#### Rotation with specified binary segmentation masks.
parameters
probability : probability of the executing this particular transformation <br>
degrees : degree of rotation. 


```python
maskImg = sitk.ReadImage(fr"****Path to binary segmentation mask****")
au = Augmentation3DUtil([img],masks=[maskImg])
au.add(Transforms.ROTATE2D,probability = 1, degrees = 45)

imgs, augs = au.process(1)

arr = sitk.GetArrayFromImage(imgs[0][0])
augarr = sitk.GetArrayFromImage(augs[0][0][0])
maskarr = sitk.GetArrayFromImage(imgs[1][0])
maskaugarr = sitk.GetArrayFromImage(augs[0][1][0])


fig = plt.figure(figsize=(10,10))
plt.subplot(221)
plt.imshow(arr[10])
plt.axis('off')
plt.subplot(222)
plt.imshow(augarr[10])
plt.axis('off')
plt.subplot(223)
plt.imshow(maskarr[9])
plt.axis('off')
plt.subplot(224)
plt.imshow(maskaugarr[9])
plt.axis('off')
```




    (-0.5, 383.5, 383.5, -0.5)




![png](output_files/output_9_1.png)


#### Flip horizontal 
parameters
probability : probability of the executing this particular transformation <br>


```python
au = Augmentation3DUtil([img],masks=None)
au.add(Transforms.FLIPHORIZONTAL,probability = 1)

imgs, augs = au.process(1)

arr = sitk.GetArrayFromImage(imgs[0][0])
augarr = sitk.GetArrayFromImage(augs[0][0][0])


plt.subplot(121)
plt.imshow(arr[10])
plt.axis('off')
plt.subplot(122)
plt.imshow(augarr[10])
plt.axis('off')
```




    (-0.5, 383.5, 383.5, -0.5)




![png](output_files/output_11_1.png)


#### Flip vertical 
parameters
probability : probability of the executing this particular transformation <br>


```python
au = Augmentation3DUtil([img],masks=None)
au.add(Transforms.FLIPVERTICAL,probability = 1)

imgs, augs = au.process(1)

arr = sitk.GetArrayFromImage(imgs[0][0])
augarr = sitk.GetArrayFromImage(augs[0][0][0])

plt.subplot(121)
plt.imshow(arr[10])
plt.axis('off')
plt.subplot(122)
plt.imshow(augarr[10])
plt.axis('off')
```




    (-0.5, 383.5, 383.5, -0.5)




![png](output_files/output_13_1.png)


#### Translation 
parameters
probability : probability of the executing this particular transformation <br>
offset : 3D cordinates in mm ex. (5,5,0)


```python
au = Augmentation3DUtil([img],masks=None)
au.add(Transforms.TRANSLATE,probability = 1, offset = (15,15,0))

imgs, augs = au.process(1)

arr = sitk.GetArrayFromImage(imgs[0][0])
augarr = sitk.GetArrayFromImage(augs[0][0][0])

plt.subplot(121)
plt.imshow(arr[10])
plt.axis('off')
plt.subplot(122)
plt.imshow(augarr[10])
plt.axis('off')
```




    (-0.5, 383.5, 383.5, -0.5)




![png](output_files/output_15_1.png)


#### Shear 
parameters
probability : probability of the executing this particular transformation <br>
magnitude : 3D cordinates 


```python
au = Augmentation3DUtil([img],masks=None)
au.add(Transforms.SHEAR,probability = 1, magnitude = (0.1,0.1))

imgs, augs = au.process(1)

arr = sitk.GetArrayFromImage(imgs[0][0])
augarr = sitk.GetArrayFromImage(augs[0][0][0])

plt.subplot(121)
plt.imshow(arr[10])
plt.axis('off')
plt.subplot(122)
plt.imshow(augarr[10])
plt.axis('off')
```




    (-0.5, 383.5, 383.5, -0.5)




![png](output_files/output_17_1.png)


### Random sampling of transformations (combination)
specify the number of samples to be obtained using "process" method<br>
specify the likelihood of a particular transform as probabilities.  


```python
au = Augmentation3DUtil([img],masks=None)
au.add(Transforms.SHEAR,probability = 0.25, magnitude = (0.05,0.05))
au.add(Transforms.TRANSLATE,probability = 0.75, offset = (15,15,0))
au.add(Transforms.ROTATE2D,probability = 0.75, degrees = 10)
au.add(Transforms.FLIPHORIZONTAL,probability = 0.75)

imgs, augs = au.process(5)

for i in range(len(augs)):
    arr = sitk.GetArrayFromImage(imgs[0][0])
    augarr = sitk.GetArrayFromImage(augs[i][0][0])

    plt.subplot(121)
    plt.imshow(arr[10])
    plt.axis('off')
    plt.subplot(122)
    plt.imshow(augarr[10])
    plt.axis('off')
    plt.show()

```


![png](output_files/output_19_0.png)



![png](output_files/output_19_1.png)



![png](output_files/output_19_2.png)



![png](output_files/output_19_3.png)



![png](output_files/output_19_4.png)

