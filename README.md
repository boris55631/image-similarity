# README #

The problem of recognizing similar images is complicated to solve. What can be more complicated is to implement search among a significant amount of images within a web platform having relational database.

This project solves this problem to a certain extent. 

With a C# console tool you can generate hashes for specified images which will be available to be stored in the database. After this, hashes may be stored into a database and be searched with a specific SQL script (Microsoft SQL Server is used in demo SQL script).

If you want to know whether images are similar to each other or not, you need to compare hashes using SQL script provided in demo. Value 0.95 and higher means that images are 95%+ alike and may be considered quite similar.

This algorithm is resistant to:

* Gray-scaled images.
* Resize.
* Lowering quality.
* Reasonable adjustments of constrast and brightness.
* Images with monotonous frames (smart-crop is used).

SQL script is tested to find calculate similarity of an image against 150K stored images within less than 2-3 seconds on S2 SQL Azure database.

### Stages of generating a hash ###

* Flattern alpha-channel (use white background).
* Smart-crop (optional, if needed): removal of monotonous edge part of the image. Useful if you have images with a frame, but want to treat it as image without a frame (consider only meaningful part of the image). Therefore, image with a frame and without it will be considered the same.
* Hiqh-quality resize to 16x16 sample image.
* Binarization: per each color channel (RGB) calculate a median and per get a bit for each pixel (higher than median - 1, lower - 0).
* Per each color channel generate hash and split it into 4 longs (BIGINT in MS SQL). This will allow fast bitwise search throughout the database.
