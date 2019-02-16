# Detrending Periodic Signals (dips)

_dips_ is an algorithm for detrending timeseries of strictly periodic signals. It does not assume any functional form for the signal or the background or the noise; it disentangles the strictly periodic component from everything else. We use it in astronomy for detrending _Kepler_, _K2_ and _TESS_ timeseries of periodic variable stars, eclipsing binary stars, exoplanets etc. The algorithm is described in detail in Prsa et al. (2019), PASP, in review -- the reference will be updated shortly.

This repository contains a python 3 implementation of _dips_.

Pre-requisites
--------------

To run _dips_, you will need:

* python 3
* numpy
* scipy

Installation
------------

Given the simplicity of _dips_ (a single source file), there is no dedicated install script. It may be added in the future though if there is sufficient interest.

To install _dips_ locally, download `dips.py` in a directory of your choice, make it executable by issuing `chmod 755 dips.py` and run it with `./dips.py` from that directory.

To install _dips_ globally, copy `dips.py` to a directory in the system path, i.e. `/usr/local/bin`, make it executable by issuing `chmod 755 dips.py` and run it with `dips.py` from anywhere.

Running _dips_
--------------

The _dips_ program is run from the command line. It takes a filename with the timeseries as input and computes the disentangled synchronous and asynchronous components of the signal as output. The disentangling process is iterative and might take an appreciable amount of time, depending on data length and pdf bin size.

Run _dips_ with:

`dips.py [-h] [-b BINS] [-t0 ORIGIN] [-P PERIOD] [-l LOGFILE] [-eta TOLERANCE] [-dxk DIFFERENCE] [-xi STEP_SIZE] [-af ATTENUATION] [--cols COLS [COLS ...]] [--disable-mp] [--initial-pdf INITIAL_PDF] [--jitter JITTER] [--save-interim SAVE_INTERIM] [--yonly] finput`

The arguments are summarized in the table below.

| Argument | Usage | Type | Default value |
|----------|-------|------|---------------|
| -h, --help | print out the help message and exit | n/a | n/a |
| -b BINS, --bins BINS | assign the number of synchronous pdf bins | int | 200 |
| -t0 ORIGIN, --origin ORIGIN | the zero-point of the timeseries | float | 0.0 |
| -P PERIOD, --period PERIOD | period of the synchronous signal | float | 1.0 |
| -l LOGFILE, --logfile LOGFILE | log file to send output to instead of screen | str | None |
| -eta TOLERANCE, --tolerance TOLERANCE | tolerance for convergence | float | 1e-8 |
| -dxk DIFFERENCE, --difference DIFFERENCE | finite difference size for computing slopes | float | 2e-5 |
| -xi STEP_SIZE, --step-size STEP_SIZE | initial down-step multiplier | float | 1e-3 |
| -af ATTENUATION, --attenuation ATTENUATION | attenuation factor for xi | float | 0.9 |
| --cols COL1 COL2 \[COL3\] | a list of input columns to be parsed, starting from 0 | list of ints | 0 1 |
| --disable-mp | disable multiprocessing (force serial computation) | bool | False |
| --initial-pdf | choice of pdf initialization ('flat', 'mean', 'median', 'random', or external filename) | str | 'median' |
| --jitter JITTER | add jitter to the computed gradients | float | 0.0 |
| --save-interim STEP | save intering solutions every STEP iterations | int | 0 |
| --yonly | use only y-distance instead of full euclidian distance | bool | False |

Distributed with _dips_ are three example input files, `synthetic.data`, `kic2445134_sap.data` and `kic3547874_sap.data`. For example, to run _dips_ on `kic3547874_sap.data`, issue:

```bash
./dips.py -t0 54989.4209 -P 19.6921722 -b 200 --cols 0 2 --yonly --initial-pdf mean kic3547874_sap.data
```

The values for `t0` and `P` were taken from the [Kepler EB catalog](http://keplerEBs.villanova.edu/overview/?k=3547874).
