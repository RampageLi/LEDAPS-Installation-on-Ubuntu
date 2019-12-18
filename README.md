本教程将指导如何在Ubuntu上安装LEDAPS.
==================================

## 1 安装虚拟机及Ubuntu
### 1.1 虚拟机安装
本次使用VirtualBox虚拟机，下载地址为：https://www.virtualbox.org/
### 1.2 Ubuntu安装
本次使用的Linux操作系统为Ubuntu 18.04.3，下载地址为：https://ubuntu.com/download/desktop <br>
具体安装流程详见：https://blog.csdn.net/zcooa/article/details/80615743 <br>
为使Ubuntu可以全屏显示，需安装VirtualBox的增强功能，具体安装流程详见：https://blog.csdn.net/caoleiwe/article/details/78583676
## 2 安装LEDAPS

### 2.1 安装依赖库
1)	打开终端，输入`sudo apt-get update`更新软件列表；
2)	使用`sudo apt-get install`安装如下库 
* TIFF (libtiff, libtiff-dev)
* GeoTIFF (libgeotiff2, libgeotiff-dev)
* ZLIB(zlib1g, zlib1g-dev)
* XML2 (libxml2, libxml2-dev)
* 其他库：g++, gfortran, bison, byacc, flex, csh, make, nano, subversion, git, curl
3)	编译安装hdf4，具体代码为：
```
sudo mkdir /usr/local/hdf4
sudo chown $USERNAME /usr/local/hdf4
cd /usr/local/hdf4
wget https://support.hdfgroup.org/ftp/HDF/releases/HDF4.2.14/src/hdf-4.2.14.tar.gz
tar –xzvf hdf-4.2.14.tar.gz
cd hdf-4.2.14.tar.gz
./configure
make
make check 
make install
sudo ldconfig
```
4)	编译安装hdf-eos，具体代码为：
```
sudo mkdir /usr/local/hdf-eos
sudo chown $USERNAME /usr/local/hdf-eos
cd /usr/local/hdf-eos
wget https://observer.gsfc.nasa.gov/ftp/edhs/hdfeos/latest_release/HDF-EOS2.20v1.00.tar.Z
tar –xzvf HDF-EOS2.20v1.00.tar.Z
cd hdfeos
./configure -enable-install-include CC=/usr/local/hdf4/hdf-4.2.14/hdf4/bin/h4ccmake
make check 
make install
sudo ldconfig
```
5)	编译安装hdf5，具体代码为：
```
sudo mkdir /usr/local/hdf5
sudo chown $USERNAME /usr/local/hdf5
cd /usr/local/hdf5
wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.8/hdf5-1.8.21/src/hdf5-1.8.21.tar.gz
tar –xzvf hdf5-1.8.21.tar.gz
cd hdf5-1.8.21
./configure
make
make check 
make install
sudo ldconfig
```
6)	编译安装hdf-eos5，具体代码为：
```
sudo mkdir /usr/local/hdf-eos5
sudo chown $USERNAME /usr/local/hdf-eos5
cd /usr/local/hdf-eos5
wget https://observer.gsfc.nasa.gov/ftp/edhs/hdfeos5/latest_release/HDF-EOS5.1.16.tar.Z 
tar –xzvf HDF-EOS5.1.16.tar.Z
cd hdfeos5
./configure -enable-install-include CC=/usr/local/hdf5/hdf5-1.8.21/hdf5/bin/h5cc
make
make check 
make install
sudo ldconfig
```
7)	编译安装NetCDF，具体代码为：
```
sudo mkdir /usr/local/netcdf
sudo chown $USERNAME /usr/local/netcdf
cd /usr/local/netcdf
wget https://www.unidata.ucar.edu/downloads/netcdf/ftp/netcdf-c-4.7.3.tar.gz
tar –xzvf netcdf-c-4.7.3.tar.gz
cd netcdf-c-4.7.3
CPPFLAGS=-I/usr/local/hdf5/hdf5-1.8.21/hdf5/include LDFLAGS=-L/usr/local/hdf5/hdf5-1.8.21/hdf5/lib ./configure --prefix=/usr/local/netcdf --disable-dap
make
make check 
make install
sudo ldconfig
```
### 2.2 配置环境变量
在终端输入`sudo gedit/etc/profile`，进行环境变量配置，具体设置如下：
```
export HDFEOS_GCTPINC="/usr/local/hdf-eos/hdfeos/hdfeos2/include/"
export HDFEOS_GCTPLIB="/usr/local/hdf-eos/hdfeos/hdfeos2/lib/"
export TIFFINC="/usr/include/x86_64-linux-gnu/"
export TIFFLIB="/usr/lib/x86_64-linux-gnu/"
export GEOTIFF_INC="/usr/include/geotiff/"
export GEOTIFF_LIB="/usr/lib/"
export HDFINC="/usr/local/hdf4/hdf-4.2.14/hdf4/include/"
export HDFLIB="/usr/local/hdf4/hdf-4.2.14/hdf4/lib/"
export HDF5INC="/usr/local/hdf5/hdf5-1.8.21/hdf5/include/"
export HDF5LIB="/usr/local/hdf5/hdf5-1.8.21/hdf5/lib/"
export HDFEOS_INC="/usr/local/hdf-eos/hdfeos/include/"
export HDFEOS_LIB="/usr/local/hdf-eos/hdfeos/hdfeos2/lib/"
export HDFEOS5_INC="/usr/local/hdf-eos5/hdfeos5/include"
export HDFEOS5_LIB="/usr/local/hdf-eos5/hdfeos5/hdfeos5/lib/"
export NCDF4INC="/usr/local/netcdf/include/"
export NCDF4LIB="/usr/local/netcdf/lib/"
export JPEGINC="/usr/include/"
export JPEGLIB="/usr/lib/x86_64-linux-gnu/"
export XML2INC="/usr/include/libxml2/"
export XML2LIB="/usr/lib/x86_64-linux-gnu/"
export ZLIBINC="/usr/include/"
export ZLIBLIB="/usr/lib/x86_64-linux-gnu/"
export JBIGINC="/usr/include/"
export JBIGLIB="/usr/lib/x86_64-linux-gnu/"
export ESPA_LAND_MASS_POLYGON="$PREFIX/static_data/land_no_buf.ply"
export ESPAINC="/usr/local/espa-tools/include/"
export ESPALIB="/usr/local/espa-tools/lib/"
export PREFIX="/usr/local/espa-tools/"
export PATH=$PATH:"/usr/local/espa-tools/bin/"
export LEDAPS_AUX_DIR="/media/sf_Ubuntu_file/ledaps_aux.1978-2017/" (此处根据你安装辅助文件的具体位置进行设置！！！)
```
### 2.3 安装ESPA-PRODUCT-FORMATTER
具体代码如下：
```
sudo mkdir /usr/local/espa-tools
sudo chown $USERNAME /usr/local/espa-tools
cd /usr/local/espa-tools
mkdir static_data 
cd static_data
wget http://edclpdsftp.cr.usgs.gov/downloads/auxiliaries/land_water_poly/land_no_buf.ply.gz
cd /usr/local/
sudo git clone https://github.com/USGS-EROS/espa-product-formatter.git
sudo chown $USERNAME /usr/local/espa-product-formatter
cd /usr/local/espa-product-formatter/raw_binary
sudo –E env make
make install
sudo ldconfig
```
### 2.4 安装ESPA Python Library
具体代码如下：
```
pip install --upgrade git+https://github.com/USGS-EROS/espa-python-library.git@v1.1.0#espa
```
### 2.5 安装LEDAPS
具体代码如下：
```
cd /usr/local
sudo wget http://edclpdsftp.cr.usgs.gov/downloads/auxiliaries/ledaps_auxiliary/ledaps_aux.1978-2017.tar.gz
sudo tar –xzvf ledaps_aux.1978-2017.tar.gz
sudo git clone https://github.com/USGS-EROS/espa-surface-reflectance.git
sudo chown $USERNAME espa-surface-reflectance
cd /usr/local/espa-surface-reflectance/ledaps/ledapsSrc/src
sudo –E env make (编译时须在Makefile文件中-IGctp后添加 –L$)(ZLIBLIB) -lz)
make install
cd /usr/local/espa-surface-reflectance/ledaps/ledapsAncSrc
make
make install
sudo cp /usr/local/espa-surface-reflectance/ledaps/ledapsSrc/scripts/do_ledaps.py $BIN
sudo cp /usr/local/espa-surface-reflectance/ledaps/ledapsSrc/scripts/ncdump $BIN
```
### 2.6 安装中遇到的问题
1)	需要新建一个/usr/tmp
```
sudo mkdir /usr/tmp
sudo chown $USERNAME /usr/tmp
```
2)	需要在/usr/local/espa-surface-reflectance/scripts进行编译
```
make install
```
3)	需要安装Numpy
```
sudo apt-get install python-numpy
```
4)	需要安装osgeo
```
sudo apt install python-gdal
```
