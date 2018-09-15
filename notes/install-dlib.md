# Install dlib for python from source on Mac Pro 3.1

I'm still using an older Mac Pro 3.1. When installing dlib for python it is necessary to disable SSE4 even though the processor should support it. So, don't use pip to install dlib, instead install from the source:

    git clone https://github.com/davisking/dlib.git
    cd dlib
    python setup.py --no USE_SSE4_INSTRUCTIONS install

The build seems to work fine for python 2.7 and 3.6, using Anaconda.
