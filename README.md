# 3D Reconstruction from 2D Images  #
December 16, 2020

## Abstract ##
Image reconstruction in any form has always been a huge area of study for image understanding by revealing more information about a scene than just what is contained in the pixels of an image. The most popular algorithm recently primarily relies on using deep learning as a method for reconstruction to train models to read large amounts of image data and learn images rather than deal with the complex camera calibration process. However, due to the scarcity of large training data sets and the limited amount of processing power, semi-global block matching (SGBM) serves as a much more efficient algorithm for reconstruction. SGBM combined with point cloud plotting requires only two similar images capturing the same object to give an accurate representation of the object in 3D. 

## Introduction ##
3D reconstruction is a process of building up a three dimensional reconstruction with a given amount of 2D images. This helps provide depth to otherwise flat images and likely give a more realistic view of an object in its real life state as opposed to on camera. The problem includes having multiple pictures of objects that need to be reconstructed in 3D. The images must be taken on different angles and the more images there are of the subject, the more data we can work with creating a more reliable model. 

## Existing Methods ##
A few researchers have investigated ways to complete 3D reconstruction. Tomasi and Kanade (1992) proposed using a rank theorem to factorize the measurement matrix (a matrix representing an image stream tracked through multiple frames) into shape and motion under orthography while taking advantage of the linear algebraic properties of orthographic projection [1]. The downfall of this method, however, is that it makes an assumption about the orthographic projection. The algorithm is robust in many situations but not for perspective effects (three dimensional objects projected on a picture plane). This factorization method has subsequently been followed by Poelman and Kanade (1997) introducing the paraperspective factorization method which can be applied to a much wider range of motion scenarios, including “image sequences containing motion toward the camera and aerial image sequences of terrain taken from a low-altitude airplane” [2]. Recently, people have started using deep learning as a method for reconstruction, redefining the ill-posed 3D reconstruction problem as a recognition problem by training deep learning models to learn images rather than deal with the complex camera calibration process in the past [3]. 

## Challenges ##
Although accurate, the effectiveness of deep learning models come at a price. First of all, there must be an availability of large training data sets. Secondly, the device that the model is trained on must possess strong processing power. Furthermore, alternative methods require a rigorous camera calibration process. This means that the camera must be tuned, setup and images must be taken at the right height and angles. The intrinsic and extrinsic camera parameters must also be calculated and retrieved successfully which served as huge obstacles for us during the brainstorming phase. We needed an algorithm that was not only efficient and can be combined with point cloud reconstruction, but also does not require large amounts of image data and training. For these reasons, semi-global block matching was discovered. 


## Methodology ##
Semi-global block matching (SGBM) is an extension of the algorithm, semi-global matching (SGM), originally created by Heiko Hirschmuller. SGBM creates an estimation of the disparity map from a pair of stereo images (two offset images that represent the views of what is seen by the left eye and the right eye). An easier way to understand this is, when someone covers different sides of their eyes, the vision in front of them is portrayed at different angles. By combining these two visions, the brain automatically triangulates the 3D coordinates of the scene that is observed and that is how humans perceive depth. The same thing works with cameras too. When two pictures are taken by two cameras at the same height but at different angles on the scene (call it left-image and right-image), the left-image will contain some points of the right-image along an epipolar line (the line that contains X in figure 1) since the two images are just slightly horizontally offset from each other [5]. 

The algorithm works by matching similar features between these two stereo images by looking for the same pixels in chunks (block sizes) [4]. The goal here is to subtract the maximum and minimum allowed disparity (i.e. the offset), a way to specify the acceptable range for which pixels can move in the blocks, along the epipolar line to get the number of disparities. 

As Hirschmuller (2008) puts it, though it is not required that the input images are rectified as this may not always be possible, image rectification does help the stereo matching process by altering the input images such that they appear to have only been taken with a slight horizontal displacement [6]. As a result, the images are assumed to work with epipolar geometry which is the geometric relation between a projected 2D image and their 3D points when the cameras portray the same scene. 

During image selection, it was discovered that many 3D movies use a similar approach to make images “pop” from screens. They would combine 2 images that were slight offsets which would later be combined by the glasses that are worn to the movies. Using this principle, the examples pulled and used for the project were largely taken from google images. The searches used to find the images were rather simple such as “3D side by side images”.  Once the proper block sizes and number of disparities were calculated based on image detail and depth, the reconstructions were visualized effectively. 

Once a depth map for a set of images has been found, it can then be transformed into a triangle mesh. This is useful because many algorithms in geometry processing use triangle meshes.

To reconstruct a triangle mesh we first start with the depth map, retrieved via block matching, from the two images. Next we need to estimate normals on each of the points in space. This is done with covariance analysis. Once we have the points and their estimated normals we can pass the pairs to the Poisson Surface reconstruction algorithm which by solving a poisson equation and then using the marching cubes algorithm returns a triangle mesh. Once a triangle mesh has been retrieved, we can then use it for different things, like finding a tetrahedral mesh and then using it for finite element analysis or modifying the triangle mesh using boolean operations.   


## References ##
[1] Tomasi, C., & Kanade, T. (1992). Shape and motion from image streams under orthography: A factorization method. International Journal of Computer Vision, 9(2), 137-154. doi:10.1007/bf00129684

[2] Poelman, C., & Kanade, T. (1997). A paraperspective factorization method for shape and motion recovery. IEEE Transactions on Pattern Analysis and Machine Intelligence, 19(3), 206-218. doi:10.1109/34.584098

[3] Han, X., Laga, H., & Bennamoun, M. (2019). Image-based 3D Object Reconstruction: State-of-the-Art and Trends in the Deep Learning Era. IEEE Transactions on Pattern Analysis and Machine Intelligence, 1-1. doi:10.1109/tpami.2019.2954885

[4] Dall’Asta, E., & Roncella, R. (2014). A COMPARISON OF SEMIGLOBAL AND LOCAL DENSE MATCHING ALGORITHMS FOR SURFACE RECONSTRUCTION. The International Archives of the Photogrammetry, Remote Sensing and Spatial Information Sciences, XL-5. doi:https://www.int-arch-photogramm-remote-sens-spatial-inf-sci.net/XL-5/187/2014/isprsarchives-XL-5-187-2014.pdf

[5] Padierna, O. (2019, January 2). Tutorial: Stereo 3D reconstruction with OpenCV using an iPhone camera. Part III. [Web log post]. Retrieved December 4, 2020, from https://medium.com/@omar.ps16/stereo-3d-reconstruction-with-opencv-using-an-iphone-camera-part-iii-95460d3eddf0

[6] Hirschmuller, H. (2008). Stereo Processing by Semiglobal Matching and Mutual Information. IEEE Transactions on Pattern Analysis and Machine Intelligence, 30(2), 328-341. doi:10.1109/tpami.2007.1166\

[7] Epipolar Geometry. (n.d.). Retrieved December 16, 2020, from https://docs.opencv.org/3.4/da/de9/tutorial_py_epipolar_geometry.html

