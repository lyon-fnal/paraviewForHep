# pvOSPRay

(Adam Lyon, October 2015, ParaView 4.4)

`pvOSPRay` is a ParaView plugin that uses the Intel OSPRay ray tracing framework. See [http://tacc.github.io/pvOSPRay/index.html](http://tacc.github.io/pvOSPRay/index.html). OSPRay is supposed to be a performant and enhanced volume rendering system. It seems to make very pretty pictures. Let's try it!

## Building

See [their instructions](http://tacc.github.io/pvOSPRay/getting_pvospray.html#source) for reference to build from source. We will follow them in this section here to build it ourselves. 

We need to download and build [OSPRay](https://ospray.github.io/) first. See OSPray build [instructions](http://ospray.github.io/getting_ospray.html) for reference. We will be using the same build infrastructure we made in our [ParaView build instructions](build.md). So go to your `/path/to/development/paraview` directory where you have `ParaView`, `MPI`, and `cmake` ready to go and that the `MYBASE` and `MYMPI` environment variables are set as per those instructions. You do not need to have ParaView built, just checked out. MPI should be built and installed. You also need to have Qt installed on your system. 

Get the OSPray source (note that we're changing their instructions a little to make building pvOSPRay easier),

```bash
cd $MYBASE
git clone https://github.com/ospray/ospray.git OSPRay
git checkout devel  # Need latest from devel branch (tried on 2015-Oct-29)
```

We need the [Intel SPMD compiler](http://ispc.github.io/) (ISPC). We'll [get](http://ispc.github.io/downloads.html) the Mac binary (not the AVX-512 variant - as far as I know the Mac CPU doesn't support that set of advanced vector extensions - though there is a Mac version of the code). 

The download is from Sourceforge, so hard to script. Just download it and move it to the right place. Or this [direct link](http://downloads.sourceforge.net/project/ispcmirror/v1.8.2/ispc-v1.8.2-osx.tar.gz?r=http%3A%2F%2Fispc.github.io%2Fdownloads.html&ts=1445008175&use_mirror=iweb) may work. Unpack the tar file in a sibling directory to OSPray. That is `/path/to/development/paraview` so that you have `/path/to/development/paraview/ispc-v1.8.2-osx` (or the directory appropriate for your platform and desired version). 

Now follow the build instructions,

```bash
cd $MYBASE
cd OSPRay
mkdir build
cd build
../../cmake-3.4.0-rc1-Darwin-x86_64/CMake.app/Contents/bin/cmake \
   -DCMAKE_BUILD_TYPE=Release \
   -DOSPRAY_BUILD_MPI_DEVICE=ON \
   -DMPI_C_LIBRARIES="$MYMPI/lib/libmpi.dylib;$MYMPI/lib/libmpi_cxx.dylib" \
   -DMPI_C_INCLUDE_PATH="$MYMPI/include" \
   -DMPI_CXX_INCLUDE_PATH="$MYMPI/include" \
   -DMPI_C_COMPILER="$MYMPI/bin/mpicc" \
   -DMPI_CXX_COMPILER="$MYMPI/bin/mpicxx" \
   -DMPIEXEC="$MYMPI/bin/mpiexec" \
   ..
make -j 4
# We won't make install
```

Try playing with some of the [examples](http://ospray.github.io/demos.html). 

Now we go back to `pvOSPRay` installation.  We apparently alter some of the ParaView source.

```bash
cd /path/to/development/paraview
cd ParaView/Plugins
git clone https://github.com/TACC/pvOSPRay.git
git checkout 4.4   # Needed to correspond to ParaView 4.4
cd ../../build
# If there's stuff in build, delete everything there
```

Now we run cmake as [instructed](build.md) for building ParaView, but add the following to the list of flags (before the last line of the `cmake` command) 

```bash
    -DOSPRAY_BUILD_DIR=../OSPRay/build \
    -DPARAVIEW_BUILD_PLUGIN_pvOSPRay=ON  \
```

Follow the rest of the build steps. 

