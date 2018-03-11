# aria2-server
Manually build aria2 and setup webui-aria2 server

apt-get install git make gcc g++ autoconf pkg-config python3  
git clone https://github.com/aria2/aria2.git  
wget https://github.com/q3aql/aria2-static-builds/raw/master/build-scripts/gnu-linux-config/aria2-x86_64-gnu-linux-build-libs  
sh aria2-x86_64-gnu-linux-build-libs  
wget https://github.com/q3aql/aria2-static-builds/raw/master/build-scripts/gnu-linux-config/aria2-x86_64-gnu-linux-config  
autoreconf -i  
sh aria2-x86_64-gnu-linux-config  
make -j4 install  
mv aria2.conf .aria2/  
git clone https://github.com/ziahamza/webui-aria2  
python3 -m http.server 80 &  
