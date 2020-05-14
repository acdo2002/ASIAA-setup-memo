# ASIAA-setup-memo
Memo for setting up the frontend and backend

# Frontend setup
[CARTA frontend webpage](https://github.com/CARTAvis/carta-frontend)

Ming-yi's note for Mac:
directly type `emcc` 

if you include `git clone --recursive https:...`

It will download the submodule simutaneously, then you can skip to type 
`cd protobuf `<br />
`git submodule init `: setting "ready to update submodule". So far, the submodule directories are empty. <br /> 
`git submodule update `:letting submodule update <br />
`git checkout master `:# using git checkout dev when using the dev branch of carta-frontend<br /> 

    Final step to open the frontend window: npm start

`git pull` in protobuf, you need to `./build_proto.sh`
builds the static JavaScript code, as well as the TypeScript definitions, and symlinks to the node_modules/carta-protobuf directory.

# Backend setup 
[CARTA backend webpage](https://github.com/CARTAvis/carta-backend)

## Universal notes:
1. Going to the 'target' github webpage and copy the link (https:....)
2. `sudo git clone https:....` under /usr/local/src
3. cd 'target' folder `mkdir build`
4. `cd build`
5. `cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local/'target'/ `
產生編譯的檔案 並且指定安裝到 `/usr/local/'target'/`
如果沒有指明 (`-DCMAKE_INSTALL_PREFIX=/usr/local/'target'/ `) 他就會安裝在 `usr/local/lib/` 裡面 名稱會是：libXXX
6. 在`/usr/local/src/build/` 下面type `make` 這個步驟是在編譯，並且放置在build裡面
7. 在`/usr/local/src/build/` 下面type `make intall` 這個步驟才會把source code完整安裝到`/usr/local/'target'/`

----
## Ming-yi's note for Mac: <br />
#### start install from zfp, fmt, protobuf, hdf5, uWebSockets, and tbb <br />
>* uWebSockets
>
>沒有cmake只有Makefile時，直接type <br />

    make
    make install
>* tbb

    git clone https://github.com/wjakob/tbb.git
    cd tbb/build
    cmake ..
    make -j
    sudo make install

>* protobuf <br />
>[C++ Installation Instructions for protobuf](https://github.com/protocolbuffers/protobuf/blob/master/src/README.md) <br />
    
    make check
    sudo make install
>works fine with my Mac. No errors with `make check` =

#### then install casacore <br />
>* casacore

    cmake .. -DCMAKE_INSTALL_PREFIX=/usr/local/casacore
    -DUSE_FFTW3=ON
    -DUSE_HDF5=ON
    -DUSE_THREADS=ON
    -DUSE_OPENMP=ON
    -DCMAKE_BUILD_TYPE=Release
    -DBUILD_TESTING=OFF
    -DBoost_NO_BOOST_CMAKE=1
    -DBUILD_PYTHON=OFF
    -DDATA_DIR=/usr/share/casacore/data
    
>* casacore (2019/12/10 casacore6)


    https://open-bitbucket.nrao.edu/projects/CASA/repos/carta-casacore/browse
    cmake -DUseCcache=1 -DHAS_CXX11=1 -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_Fortran_COMPILER=/usr/local/bin/gfortran -DBoost_NO_BOOST_CMAKE=1 -DBUILD_TESTING=OFF -DCMAKE_INSTALL_PREFIX=/usr/local -DCMAKE_CXX_FLAGS="-I /Library/Python/2.7/site-packages/numpy/core/include" -DCMAKE_BUILD_TYPE=Debug ..
    
#### finally install carta-protobuf <br />
>* carta-protobuf <br />
> -I means include <br />
> -L means lib <br />

    $cmake -DCMAKE_CXX_FLAGS="-I /usr/local/casacore/include 
    -I /usr/local/fmt/include 
    -I /usr/local/Cellar/tbb/2019_U8/include 
    -I /usr/local/include 
    -I /usr/local/Cellar/openssl/1.0.2s/include 
    -I /usr/local/zfp/include" 
    -DCMAKE_CXX_STANDARD_LIBRARIES="-L /usr/local/casacore/lib 
    -L /usr/local/lib 
    -L /usr/local/Cellar/openssl/1.0.2s/lib 
    -L /usr/local/fmt/lib 
    -L /usr/local/zfp/lib -luWS" ..
    $make
> 如果要跟新 要在build下面 <br />

    $git pull
    ($cmake xxx)
    $make
    
    Final step to open the backend server: 
    $LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/casacore/lib
    $export LD_LIBRARY_PATH
    $./carta_backend 
    
##### 2019/08/26 New casacode installation (Linux)
>[casacore installation](https://docs.google.com/document/d/1290lnvp9fDShOKuipJUmusAfcuqBmSmPwaEbRUVOM28/edit) <br />
Once it has been installed(/usr/local/), the carta-backend should re-build again. 
So far it works on Ubuntu system:

    Final step to open the backend server: 
    
    $LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/casacore/lib:/usr/local/zfp/lib:/usr/local/lib <br />
    $export LD_LIBRARY_PATH
    
    $./carta_backend

##### 2019/11/04 Update casacode installation (Linux)
Problem: 

`sudo echo 'code/*' > .git/info/sparse-checkout`

terminal returns Permission denied!

[Solution](https://unix.stackexchange.com/questions/4830/how-do-i-use-redirection-with-sudo) <br />
`>>` happens before the actual command execution and does not run with the elevated `sudo` privileges.

An alternative way to do this, is to wrap the entire command in another bash command shell:

`sudo bash -c "echo 'code/*' > .git/info/sparse-checkout"`

##### 2019/11/05 Update CARTA-backend installation (Linux)
Problem:

1. make: cannot find fmt

solution:

`sudo apt-get install libfmt-dev`  (not libfmt3-dev for Ubutntu 18)

2. final cmake version:

`$ cmake .. -DCMAKE_CXX_FLAGS="-I/usr/local/include/casacode -I/usr/local/include/casacore -I/usr/local/casacore/include -I/usr/local/casacore/include/casacore/ -I/usr/local/zfp/include/" -DCMAKE_CXX_STANDARD_LIBRARIES="-L/usr/local/lib -L/usr/local/casacore/lib -L/usr/local/zfp/lib -L/usr/local/protobuf/lib -limageanalysis"`

`$ make`

##### 2020/05/13 Update ARTA-backend installation (Linux)
Problem: 


   [100%] Linking CXX executable carta_backend

   /usr/lib/gcc/x86_64-linux-gnu/7/../../../../lib/libuWS.so: undefined reference to `SSLv23_client_method'

   /usr/lib/gcc/x86_64-linux-gnu/7/../../../../lib/libuWS.so: undefined reference to `SSL_library_init'

   /usr/lib/gcc/x86_64-linux-gnu/7/../../../../lib/libuWS.so: undefined reference to `SSLv23_server_method'

   collect2: error: ld returned 1 exit status

   CMakeFiles/carta_backend.dir/build.make:787: recipe for target 'carta_backend' failed

   make[2]: *** [carta_backend] Error 1

   CMakeFiles/Makefile2:67: recipe for target 'CMakeFiles/carta_backend.dir/all' failed

   make[1]: *** [CMakeFiles/carta_backend.dir/all] Error 2

   Makefile:83: recipe for target 'all' failed

   make: *** [all] Error 2
   
Solution:

apt install libssl1.0-dev

https://stackoverflow.com/questions/5593284/undefined-reference-to-ssl-library-init-and-ssl-load-error-strings
