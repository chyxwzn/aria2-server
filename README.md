# aria2-server
Manually build aria2 and setup webui-aria2 server

apt-get install git make gcc g++ autoconf pkg-config python3   
git clone https://github.com/aria2/aria2.git  
wget https://github.com/q3aql/aria2-static-builds/raw/master/build-scripts/gnu-linux-config/aria2-x86_64-gnu-linux-build-libs  
sh aria2-x86_64-gnu-linux-build-libs  
cd aria2  
wget https://github.com/q3aql/aria2-static-builds/raw/master/build-scripts/gnu-linux-config/aria2-x86_64-gnu-linux-config  
autoreconf -i  
sh aria2-x86_64-gnu-linux-config  
make -j4 install  
mv aria2.conf ~/.aria2/  
git clone https://github.com/ziahamza/webui-aria2  
cd webui-aria2  
python3 -m http.server 80 &  
# speed up aria2
we should unlimit max-connection-per-server  
vim OptionHandlerFactory.cc  
1. change  
```c
OptionHandler* op(new NumberOptionHandler(PREF_MAX_CONNECTION_PER_SERVER,
                                               TEXT_MAX_CONNECTION_PER_SERVER,
                                               "1", 1, 16, 'x'));
```
to  
```c
OptionHandler* op(new NumberOptionHandler(PREF_MAX_CONNECTION_PER_SERVER,
                                               TEXT_MAX_CONNECTION_PER_SERVER,
                                               "128", 1, -1, 'x'));
```
2. change  
```c
PREF_MIN_SPLIT_SIZE, TEXT_MIN_SPLIT_SIZE, "20M", 1_m, 1_g, 'k'));
```
to  
```c
PREF_MIN_SPLIT_SIZE, TEXT_MIN_SPLIT_SIZE, "4K", 1_k, 1_g, 'k'));
```
3. change  
```c
PREF_CONNECT_TIMEOUT, TEXT_CONNECT_TIMEOUT, "60", 1, 600));
```
to  
```c
PREF_CONNECT_TIMEOUT, TEXT_CONNECT_TIMEOUT, "30", 1, 600));
```
4. change   
```c
PREF_PIECE_LENGTH, TEXT_PIECE_LENGTH, "1M", 1_m, 1_g));
```
to  
```c
PREF_PIECE_LENGTH, TEXT_PIECE_LENGTH, "4k", 1_k, 1_g));
```
5. change  
```c
new NumberOptionHandler(PREF_RETRY_WAIT, TEXT_RETRY_WAIT, "0", 0, 600));
```
to  
```c
new NumberOptionHandler(PREF_RETRY_WAIT, TEXT_RETRY_WAIT, "2", 0, 600));
```  
6. change  
```c
new NumberOptionHandler(PREF_SPLIT, TEXT_SPLIT, "5", 1, -1, 's'));
```
to  
```c
new NumberOptionHandler(PREF_SPLIT, TEXT_SPLIT, "8", 1, -1, 's'));
```
