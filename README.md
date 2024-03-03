# Pongalytics
## Table Detection
In order to determine when and where the ball bounces, we will need to know the location of the table.

In order to detect the table I have taken the following steps:
1. Decrease the size of the image to improve performance, but not too much since the ball is already very small in frame
    - TODO: Find the best balance between the two
1. We need an unobstructed frame of the table to detect its boundaries, thus we randomly sample n frames so we can see the whole table accross the n images
1. For each of these n images:
    1. Apply colour thresholding to the image to look for the blue of the table, the table is almost always the same colour
    1. Use morphological operations to clean the thresholded image
    1. Find contours in the cleaned threshold image
    1. Filter contours by size and position (near the center) to find the contour of the table
    1. Apply Hough Lines to the contour of the table to find the lines corrisponding to the 4 edges of the table
1. All lines are aggregated accross the n samples and outliers are filtered out
1. KMeans with a K = 4 is used to cluster the lines into 4 classes where each class represents an edge of the table
1. These four lines are then sorted based on gradients and split into pairs of parallel edges
1. Use these lines to find the intersection points corrisponding to each corner
1. Finally, with the 4 coordinates, find the Homography Transformation that maps the four coordinates onto the actual dimension of the table, which will be used to indicate ball bounce positions

See `table_detection.ipynb` for the code implementing what I just described.