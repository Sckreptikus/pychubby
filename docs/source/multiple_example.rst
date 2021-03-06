Multiple Faces
==============
So far we assumed that there is a single face in the image. However, the real power of :code:`pychubby`
lies in its ability to handle multiple faces. Let's start with the following image.

.. image:: https://i.imgur.com/TTHS1VR.jpg
  :width: 500
  :alt: Original image
  :align: center

If more than one face is detected in an image :code:`pychubby` uses the :code:`LandmarkFaces` class rather than 
:code:`LandmarkFace`. :code:`LandmarkFaces` is essentially a container containing :code:`LandmarkFace`
instances for each face in the image.

.. code-block:: python

	import matplotlib.pyplot as plt
	from pychubby.detect import LandmarkFace

	img = plt.imread("path/to/the/image") 
	lfs = LandmarkFace.estimate(img) # lfs is an instance of LandmarkFaces
	lfs.plot(show_landmarks=False, show_numbers=True)


.. image:: https://i.imgur.com/CgmwO8Q.jpg
  :width: 500
  :alt: Original image
  :align: center

Each face is assigned a unique integer (starting from 0). This ordering is very important since it
allows us to specify which action to apply to which face.

In order to apply actions we use the metaaction :code:`Multiple`. It has two modes:

1. Same action on each face
2. Face specific actions


Same action 
-----------
The first possibility is to apply exactly the same action to each face in the image.
Below is an example of making all faces more chubby.

.. code-block:: python

	from pychubby.actions import Chubbify, Multiple

	a_single = Chubbify(0.2)
	a = Multiple(a_single)
	new_lfs, df = a.perform(lfs)  
	new_lfs.plot(show_landmarks=False, show_numbers=False)

.. image:: https://i.imgur.com/mxsLqll.jpg
  :width: 500
  :alt: Single action image
  :align: center

.. code-block:: python

	from pychubby.visualization import create_animation

	ani = create_animation(df, img)

.. image:: https://i.imgur.com/qaxuHMs.gif
  :width: 500
  :alt: Single action gif
  :align: center

Different actions
-----------------
Alternetively, we might want to apply different action to each face (or potentially no action).
Below is an example of face specific actions.


.. code-block:: python

	from pychubby.actions import Chubbify, LinearTransform, Multiple, OpenEyes, Pipeline, Smile 

	a_0 = LinearTransform(scale_x=0.9, scale_y=0.9)
	a_1 = Smile(0.14)
	a_2 = None
	a_3 = Pipeline([OpenEyes(0.05), LinearTransform(scale_x=1.02, scale_y=1.02), Chubbify(0.2)])

	a = Multiple([a_0, a_1, a_2, a_3])	
	new_lfs, df = a.perform(lfs)  
	new_lfs.plot(show_landmarks=False, show_numbers=False)

.. image:: https://i.imgur.com/45NxDyI.jpg 
  :width: 500
  :alt: Multiple actions image
  :align: center

.. code-block:: python

	from pychubby.visualization import create_animation

	ani = create_animation(df, img)

.. image:: https://i.imgur.com/ZWYEIZN.jpg
  :width: 500
  :alt: Single action gif
  :align: center
