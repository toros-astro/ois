.. _methods:

Methods
=======


Alard & Lupton
--------------

Alard and Lupton [alard1997]_ introduced a method to simultaneously fit a background and a convolution kernel that will minimize the difference between a reference image and an another science image.

This method assumes that the convolution kernel can be approximated by a linear combination of fixed Gaussians, where the linear coefficients are left free to vary.
The centre, width and orientation of the Gaussians are fixed beforehand as well as the number of Gaussians to use.

In addition, each Gaussian is modulated with a polynomial of an arbitrary degree.

In OIS this is specified with a list of dictionaries, one for each gaussian we want to use.
Below is an example::

    gausslist = [{center: (5, 5),
                  sx: 2.
                  sy: 2.
                  modPolyDeg: 3},
                 {sx: 1.0
                  sy: 2.5
                  modPolyDeg: 1},
                 {sx: 3.0
                  sy: 1.0},
                ]

In this example ois will return the kernel with the optimal linear combination of the three Gaussians provided.

Not all the keys need to be present. The dictionary keys are as follows:

**center:** the pixel x, y coordinates of the center relative to the kernel origin (column=0, row=0).
For a 11 rows by 13 columns kernel, the center of the kernel would be at x, y = (6, 5)
If not provided, ois will assume the kernel center calculated from its shape.

**sx:** sigma in the x direction

**sy:** sigma in the y direction

**modPolyDeg:** The degree of the modulating polynomial.
If not provided, it will default to 2.


Bramich
-------

If no list of Gaussians is provided, ois will default to use the method for image subtraction developed by Bramich [bramich2010]_.

The method consists on the simultaneous fit of a polynomial background and a convolution kernel.
Unlike Alard & Lupton method, in Bramich, each pixel of the kernel is left to vary independently
and nothing is assumed about the general shape of the kernel.

While more accurate, this method is computationally more expensive than Alard & Lupton's.

.. warning::

  Since each pixel is treated independently, a 11 by 11 kernel will have 121 free parameters just for the kernel. 
  It grows quadratically with the kernel side. This needs to be taken in consideration for large kernels.


Other arguments
---------------

**bkgdegree:** The degree of the background polynomial to fit. If not provided, it will default to 3.

**kernelshape:** The kernel shape. If not provided, it will default to (11, 11).


.. [alard1997] "A method for optimal image subtraction" - C. Alard, R. H. Lupton, 1997.
.. [bramich2010] "A New Algorithm For Difference Image Analysis" - D.M. Bramich, 2010.