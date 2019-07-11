# ASIAA-setup-memo
Memo for setting up the frontend and backend

# Frontend setup
[CARTA frontend webpage](https://github.com/CARTAvis/carta-frontend)

Ming-yi's note for Mac:
directly type `emcc` 

if you include `git clone --recursive https:...`

It will download the submodule simutaneously, then you can skip to type 
`cd protobuf `<br />
`git submodule init `<br />
`git submodule update `<br />
`git checkout master `<br />

    Final step to open the frontend window: npm start

# Backend setup 
[CARTA backend webpage](https://github.com/CARTAvis/carta-backend)

Universal notes:
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
Ming-yi's note for Mac: <br />
* start install from zfp, fmt, protobuf, hdf5, uWebSockets, and tbb <br />
>uWebSockets

沒有cmake只有Makefile時，直接type <br />
`make` <br />
`make install`

* then install casacore <br />
ddd
* finally install carta-protobuf <br />
ddd

