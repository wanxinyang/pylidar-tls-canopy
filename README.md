# pylidar-tls-canopy

This is a lite version of [pylidar canopy tools](http://www.pylidar.org/en/latest/commandline_canopy.html) specifically for gap probability modeling using data acquired by [RIEGL VZ series](http://www.riegl.com/nc/products/terrestrial-scanning/) and [LEAF](https://www.sensingsystems.com.au/) instruments.


---

## Part A: Quick Installation Guide for JASMIN Users with GWS Access

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

---

## Part B: Run pylidar-lite on JASMIN to get plant vertical profile

### Create a dir named *pai* inside the project. The dir structure is like:

```
Country/
└── PLOT/
└── Project (XXXX-XX-XX.PROJ)/
├── raw/
│   ├── ScanPos001.SCNPOS/
│   ├── ScanPos002.SCNPOS/
│   └── ScanPosXXX.SCNPOS/
│       └── scans/
│           └── *.rxp
│
├── matrix/
│   ├── ScanPos001.DAT
│   ├── ScanPos002.DAT
│   └── ScanPosXXX.DAT
│
└── pai/  # This is the output directory
    ├── ScanPos001.csv
    ├── ScanPos002.csv
    └── ScanPosXXX.csv
```

### Example to test for a single scan from Alice Holt PROJ
`conda activate pylidar-lite`

`cd /gws/nopw/j04/nceo_generic/nceo_ucl/TLS/uk/alice_holt/AH2/2023-07-18-AHL2.PROJ/pai`

```bash
pylidar_plantprofile -i ../raw/ScanPos001.SCNPOS/scans/230718_043106.rxp \
  -o pai_test.csv \
  -m WEIGHTED \
  --height_resolution 0.5 \
  -t ../matrix/ScanPos001.DAT \
  --max_height 30 \
  --min_zenith -30 \
  --max_zenith 70 \
  --min_azimuth 0 \
  --max_azimuth 360
```

### Submit jobs on JASMIN to run over all scans 
Activate pylidar-lite env

`conda activate pylidar-lite`

Navigate to the pai dir

`cd /PATH/TO/PROJ/pai`

Subimt batch processing job on JASMIN LOTUS2 clusters
```bash
for rxp in $(ls ../raw/ScanPos*/scans/??????_??????.rxp); do \
  sbatch --export=rxp=${rxp} \
  /gws/nopw/j04/nceo_generic/nceo_ucl/TLS/wx_test/jobs_LOTUS2/run_pylidar; \
done
```



