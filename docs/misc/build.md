# Building from source

(Adam Lyon, October 2015, ParaView 4.4)

While binaries of the ParaView application are available at [http://www.paraview.org](http://www.paraview.org), you may find that you need to build ParaView from source yourself. A build from source will be required if you want to have a special non-default build option turned on or off, build against a different Open MPI library, or want the latest changes in ParaView from git. Otherwise, you should definitely use the pre-compiled binaries as described in [Obtaining ParaView](../intro/intro.md#obtaining-paraview).

## Obtaining ParaView Source

See [https://gitlab.kitware.com/paraview/paraview/blob/master/Documentation/dev/git/download.md](https://gitlab.kitware.com/paraview/paraview/blob/master/Documentation/dev/git/download.md) for information on how to `git clone` the ParaView repository. A summary is,

```bash
cd /path/to/development/paraview
git clone --recursive https://gitlab.kitware.com/paraview/paraview.git ParaView
# Your source code will be in /path/to/development/paraview/ParaView
```

If you need to update the repository (e.g. pull), then do

```bash
cd /path/to/development/paraview/ParaView
git pull
git submodule update --init
```

## Installing Qt

You need to install Qt v4 (4.7.0 or later, but not v5) on your system. See [http://download.qt.io/archive/qt/4.8/4.8.6/](http://download.qt.io/archive/qt/4.8/4.8.6/). ParaView will be able to find it. 

## Building MPI

You will also need an installation of MPI. There are many MPI implementations. I typically use *Open MPI*, see [http://www.open-mpi.org/](http://www.open-mpi.org/). If you are on a machine with infiniband interconnects, you may want to use the [MPICH](http://www.mpich.org/) implementation. Here's how I download and build Open MPI (using openmpi-1.10.0) on my Mac.

```bash
cd /path/to/development/paraview
curl -O http://www.open-mpi.org/software/ompi/v1.10/downloads/openmpi-1.10.0.tar.gz
tar xvzf openmpi-1.10.0.tar.gz  # Makes openmpi-1.10.0 directory
cd openmpi-1.10.0

./configure --prefix=/path/to/development/paraview/openmpi
make install -j 4  # build with 4 cpus

# Open MPI is installed in /path/to/development/paraview/openmpi
```

Remember the location of the installation directory. You will need it if you ever run `mpiexec` or similar. 

## Downloading and building cmake

You need a late verison of `cmake` to build ParaView. See [https://cmake.org/download/](https://cmake.org/download/) to download it (note that `cmake` is written by Kitware too!). Download the file appropriate for your platform. For example, on my Mac I do,

```bash
cd /path/to/development/paraview
curl -O https://cmake.org/files/v3.4/cmake-3.4.0-rc1-Darwin-x86_64.tar.gz
tar xvzf cmake-3.4.0-rc1-Darwin-x86_64.tar.gov
```
For the Mac, this tar file has the necessary binary. For another platform, you may need to build form source. See the `cmake` instructions. 

## Building ParaView

We should now have the necessary pieces to build ParaView. Note that if you want to make animation files directly in ParaView, you need to have `ffmpeg` installed in your system. You should probably have `ffmpeg` installed anyway, because the movie files produced by ParaView may need re-encoding to display properly in HTML5 or your on iPad. 

`cmake` likes "out of source" builds, so we make a build directory.

```bash
cd /path/to/development/paraview
mkdir build
cd build
```

Now we run `cmake` with necessary options. If you are building on Linux, then the `cmake` path in the example below will be different. It is handy to set an environment variable with the location of your built MPI libraries. Note that if you used an MPI implementation other than *Open MPI*, then the library names may be different (e.g. `mpi_cxx` may be `mpicxx`). Also, on Linux change `.dylib` to `.so`. Lastly, I build using my Mac's system `python`. 

If you are on  Mac, the application will eventually (after `make install`) be installed in `/Applications/paraview.app` (it will not work if installed elsewhere). On Linux, the default installation will be `/usr/local`. To change that, below set `-DCMAKE_INSTALL_PREFIX=/path/to/paraview`. 

```bash
# In the build directory
export MYBASE=/path/to/development/paraview # Replace with your location
export MYMPI=$MYBASE/openmpi  # Replace with your MPI installation directory 

../cmake-3.4.0-rc1-Darwin-x86_64/CMake.app/Contents/bin/cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_TESTING=OFF \
    -DPARAVIEW_USE_MPI=ON \
    -DMPI_C_LIBRARIES="$MYMPI/lib/libmpi.dylib;$MYMPI/lib/libmpi_cxx.dylib" \
    -DMPI_C_INCLUDE_PATH="$MYMPI/include" \
    -DMPI_CXX_INCLUDE_PATH="$MYMPI/include" \
    -DMPI_C_COMPILER="$MYMPI/bin/mpicc" \
    -DMPI_CXX_COMPILER="$MYMPI/bin/mpicxx" \
    -DMPIEXEC="$MYMPI/bin/mpiexec" \
    -DPARAVIEW_BUILD_PLUGIN_GMVReader=OFF \
    -DPARAVIEW_BUILD_PLUGIN_ForceTime=ON \
    -DPARAVIEW_BUILD_PLUGIN_TemporalParallelismScriptGenerator=ON \
    -DPARAVIEW_BUILD_CATALYST_ADAPTORS=ON \
    -DPARAVIEW_ENABLE_PYTHON=ON \
    -DPYTHON_INCLUDE_DIR=/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7 \
    -DPYTHON_EXECUTABLE=/usr/bin/python \
    ../ParaView/
```

If you wanted to build so that ParaView uses OpenGL2 (faster, but buggy), then add `-DVTK_RENDERING_BACKEND=OpenGL2 \`. 

Note that `-DBUILD_TESTING` is set to `OFF`. Turning it on will cause the build to download a large number of small files from Kitware. Turning it off makes the build much faster and you can build without a network connection.

Now build and install,

```bash
make -j 4
make install
```

And it's done. 


