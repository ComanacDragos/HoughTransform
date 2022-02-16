# Hough transform 
<a href="https://github.com/ComanacDragos/HoughTransform/tree/main/threads">Threads implementation</a>

<a href="https://github.com/ComanacDragos/HoughTransform/tree/main/mpiProject">MPI implementation</a>

![result](https://user-images.githubusercontent.com/46956225/154142384-91c53c37-5c01-48b5-b60d-83861b793ad4.png)

The algorithm is composed of several stages.
<ol>
<li>
    Preprocessing: image is converted to grayscale and then Gaussian blur is applied which consists of convolving the grayscale image with the following filter:
   
2/159, 4/159, 5/159, 4/159, 2/159

4/159, 9/159, 12/159, 9/159, 4/159

5/159, 12/159, 15/159, 12/159, 5/159,

4/159, 9/159, 12/159, 9/159, 4/159

2/159, 4/159, 5/159, 4/159, 2/159

</li>
<li>
Edge map: to extract the edge map, the Sobel operator is used.

![image](https://user-images.githubusercontent.com/46956225/154141969-24c19548-6887-46be-92d5-2b470ed9d9dd.png)

Then the gradients are computed in the following way:

![image](https://user-images.githubusercontent.com/46956225/154141984-f5b761e9-e5cb-4f78-9e31-2ebc044be87e.png)

And if the gradients are greater than some preset threshold, then the pixel is an edge pixel.
</li>

<li>
Mapping of edge points to the Hough space and storage in an accumulator: For each edge point we convert it to polar coordinates and if it is within the image boundaries, the accumulator for that point is incremented.
</li>

<li>
Interpretation of accumulator to generate infinite lines: for each point in the accumulator, we check if it is larger then a preset threshold and if it is a peak in its neighborhood then the polar coordinates representing a line are converted back to cartesian coordinates.
</li>

<li>
Drawing the lines: for each point we check if it is on a detected line.
</li>
</ol>


To perform all these steps, multiple transformers are implemented which receive an image and return a transformed image.

Transformers used:
- Convolution and GaussianBlur simply uses a convolution
- GrayScale
- Sobel – uses 2 convolutions (vertical and horizontal Sobel filters) then it
  computes their gradients – the output is an edge map
- Hough – this takes an edge map, and it outputs the edge map with the infinite
  lines

In the parallel versions, each thread receives a list of positions in the image, split
  sequentially and performs some action required by the transformer.

The MPI version operates in a similar manner, only that the input image must be
  sent to each process at the start.

Implementation is an adapted version of https://github.com/davidchatting/hough_lines/blob/master/HoughTransform.java

Performance

Title of each plot has the following format Transformer_Size of the image

x-axis represents the number of threads

y-axis represents the time in milliseconds

![threads](https://user-images.githubusercontent.com/46956225/154141922-bb9a27be-c804-4d71-9748-f7cec678e465.png)

![threads_and_mpi](https://user-images.githubusercontent.com/46956225/154141927-6f56f7d2-43f7-4415-bd00-bff8b096e5e6.png)
