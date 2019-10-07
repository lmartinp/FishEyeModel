# FishEyeModel
Python project based on OpenCV module to model FishEye camera and undistort acquired images

## Dependencies
Python 3.7 or higher  
OpenCV 4.0 or higher

## Calibration
OpenCV calibration algorithm for FishEye camera enables to model intrinsic and/or extrinsic parameters from a set of images acquired by the camera.
To use the function, you have to acquire images of known pattern from which it is possible to extract distortions (chessboard for example).
A chessboard pattern can be downloaded following this link: 
[Chessboard pattern](https://drive.google.com/file/d/0B3EXsqKhWPfdVk9NRmIzNE1RQms/view?usp=sharing).

Then you have to acquire several images at different positions to integrate as much distorsion information as possible into the model. 
In our tests, a set of 25-30 images can be sufficient to have good undistort results.

We give a Python script to launch the calibration

```console
python calibrate.py "ImageFolder" [-o "OutputFile"] [-s "True/False"]
```

**ImageFolder (mandatory)**: folder where acquired images for calibration are stored.  
**OutputFile (optional)**: path of the saved model file. By default, file *calibration.txt* is saved into the current folder.  
**True/False (optional)**: boolean value to indicate if chessboard detection images have to be saved.  

The main steps of the algorithm are:
1. Detection of all inner corners of the chessboard for each image.
2. Improvement of corner positions thanks to super-resolution algorithm.
3. Valid detections storage.
4. FishEye modelization on valid detections.
5. Save calibration parameters to file (extrinsic or intrinsic).

**Note:** calibration can failed even if chessboard detection is valid.
Error can occured during the model computation (because of *cv2.fisheye.CALIB_CHECK_COND* flag).
You have to remove the corresponding chessboard image to ensure good calibration.

## Undistort
We give a Python script to launch the correction:

```console
python undistort.py "SourceImage" [-c "CalibrationFile"] [-o "OutputImage"]
```

**SourceImage (mandatory)**: path of the image to undistort.  
**CalibrationFile (optional)**: path of the model file generated by the calibration. 
If not set, the script will search the default file *calibratio.txt" into the current folder and fail if not found.  
**OutputImage (optional)**: path of the saved undistorted image. 
By default, undistorted image is saved in the file *undistort.png* into the current folder.
