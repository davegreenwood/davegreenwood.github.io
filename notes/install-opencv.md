
# Install openCV on Macintosh - using Homebrew

Installing openCV, with bindings for python 2 and 3 is easy using Homebrew.

    brew install opencv

It is no longer necessary to specify python version - or the contributors package.
Homebrew installs both python interpreters, however, I use an Anaconda python installation, along with conda environments. To use openCV it is necessary to link the openCV library.

First, find the installation of the libs

    find /usr/local/lib cv2 | grep cv2

    /usr/local/lib/python2.7/site-packages/cv2.so
    /usr/local/lib/python3.7/site-packages/cv2.cpython-37m-darwin.so

Now link to the python interpreter:

    ln -s /usr/local/lib/python2.7/site-packages/cv2.so \ /Users/Shared/anaconda/lib/python2.7/site-packages/cv2.so

If using a virtual environment, link to that too. For python 3, rename the link to `cv2.so`.

    ln -s /usr/local/lib/python3.7/site-packages/cv2.cpython-37m-darwin.so \
    /Users/Shared/anaconda/envs/dlib/lib/python3.6/site-packages/cv2.so

Note, brew installs python 3.7, but `cv2.cpython-37m-darwin.so` seems to work fine for python 3.6.
