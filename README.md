# pylidar-tls-canopy

This is a lite version of [pylidar canopy tools](http://www.pylidar.org/en/latest/commandline_canopy.html) specifically for gap probability modeling using data acquired by [RIEGL VZ series](http://www.riegl.com/nc/products/terrestrial-scanning/) and [LEAF](https://www.sensingsystems.com.au/) instruments.


---

## Quick Installation Guide for JASMIN Users with GWS Access

### 1. Configure Environment Variables

Edit your *~/.bashrc* file:

`vi ~/.bashrc`

Add the following lines to set up the required RIEGL libraries:

```bash
export RIVLIB_ROOT=/gws/nopw/j04/nceo_generic/nceo_ucl/TLS/tools/rivlib-2_13_0-x86_64-linux-gcc11
export RDBLIB_ROOT=/gws/nopw/j04/nceo_generic/nceo_ucl/TLS/tools/rdblib-2.5.2-x86_64-linux
export PYLIDAR_CXX_FLAGS="-std=c++11"
```

Save and exit: 

`:wq`

Acitvate the changes:

`source ~/.bashrc`


### 2. Clone the repository and set up the conda env
Navigate to your desired dir, e.g.,

`cd ~`


Clone this repository:

`git clone git@github.com:wanxinyang/pylidar-tls-canopy.git`


Navigate into the repository:

`cd pylidar-tls-canopy`


Create and activate the conda env:

**Note**: If you don't have access to nceo_ucl gws on JASMIN, or you want to set up this env on another Linux server, you are required to download the RIEGL libraries from [RIEGL members area](http://www.riegl.com/members-area/software-downloads/libraries/) and update `RIVLIB_ROOT` and `RDBLIB_ROOT` accordingly in the environment.yml file.

`conda env create -f environment.yml`

`conda activate pylidar-lite`


Install the packages:

`pip install . -v`

### 3. Verify installation
Run any of the following commands to check if the tools are installed and functioning correctly. If they return help messages without errors, your installation was successfully.
```
pylidar_cartesiangrid -h
pylidar_scangrid -h
pylidar_sphericalgrid -h
pylidar_plantprofile -h
pylidar_voxelization -h
```
- All of these are applicable to RIEGL data. 
- Only pylidar_plantprofile is applicable to LEAF data.


### Additional resources
- See INSTALL.txt for more detailed installation instructions.

- See the Jupyter Notebooks for RIEGL gridding, RIEGL vertical profile, LEAF time-series, and RIEGL voxel analysis examples.
 
