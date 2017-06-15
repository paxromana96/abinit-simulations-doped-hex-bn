# Simulation of Graphite using Abinit
This project uses a program called [Abinit](http://www.abinit.org/) to simulate a mono-layer graphite lattice using ab-inito principles. The goals of this project are as follows:

- Gain familiarity with the Abinit program
- Determine the band energies of graphite
- Determine the charge density of graphite
- Determine the state density of graphite

This project was written for the laboratory of Jun Namakura (中村) at the University of Electrocommunications (電気通信大学) in Tokyo (東京都), Japan (日本).

## Contributing
The main point of this project is for me to do the work myself, but I'm always happy to get feedback. At present, I will not be accepting pull requests, but please feel free to submit an Issue on GitHub.

## License
See [LICENSE.md](LICENSE.md).

## Dependencies
This program uses the following programs:

- [Abinit](http://www.abinit.org/) to perform the simulation, assumed to be called `abinit`
- [python](https://www.python.org/) version 2 to execute various custom analysis scripts, assumed to the user's $PATH as `python`
    - `parse_band_eigenvalues.py` to convert `_EIG` files into more-easily readable [JSON](http://www.json.org/) form for analysis
- [Java](https://java.com) to execute various custom analysis scripts, assumed to be in the user's $PATH as `java`
    - `spacedToCSV.jar` to convert `.dat` files output by `cut3d` into more-easily computable [CSV](https://en.wikipedia.org/wiki/Comma-separated_values). Currently I'm targeting Java 1.7.
- [Wolfram Mathematica](https://www.wolfram.com/mathematica/) to execute various `.nb` files for analysis of the data
- [GNU Make](https://www.gnu.org/software/make/) to execute the Makefile that automatically runs the above programs. It is, of course, still possible to run this project without Make, but very tedious.

## Use
### Makefile
Most of the program has been set up to run via a [Makefile](Makefile).
#### Recipes
The following make recipes are used (note that `%` refers to a wildcard string):

- `geom` optimize the geometry of the graphite cell
- `band` determine the band energy eigenstates of the graphite cell
- `%.out` run the experiment `%.in` and output the results to `%.out`. Errors are reported to `log`
- `%.files` generate a file with the command-line user input to `abinit` for the experiment `%.in`
- `%_band_eigen_energy.json` use the python script `parse_band_eigenvalues.py` to parse `%_EIG` and output it in more-readable `JSON` format.
- `%_3d_indexed.dat` leverage `cut3d` from the Abinit suite to get 3d indexed data from `%_DEN`.
- `%_3d_indexed.csv` use the Java script `spacedToCSV.jar` to parse `%_3d_indexed.dat` and output it in more-readable `CSV` format
- `clean` execute `cleanLog`, `cleanTemp`, and `cleanOutput` to remove files generated by Abinit. This does not remove files generated by analysis scripts.
- `cleanTemp` remove all files with `.generic` in the file name
- `cleanLog` remove the log
- `cleanOutput` remove all files with `.out` in the file name

#### Customization
Some variables can be passed to the Makefile via `make <recipe> VARNAME=value ...`. Assigning these variables on the command line allows the user to change where Make will look for various paths.

- If the `VERBOSE` variable is specified, `abinit` experiment outputs will be copied to stdout
- `ABINIT_DIR_PATH` indicates the top-level folder where Abinit is installed. Default is `~/abinit`
- `CARBON_PSEUDO` indicates the file to give Abinit for carbon pseudopotentials. Only used to build `.files` files. Default is the LDA pseudopotential `abinit/tests/Psps_for_tests/6c_lda.paw`
- `PATH_TO_EIG_PARSER` indicates where to find `parse_band_eigenvalues.py`. Default is `~/bin/parse_band_eigenvalues.py`
- `PATH_TO_DEN_PARSER` indicates where to find `spacedToCSV.jar`. Default is `~/bin/spacedToCSV.jar`

### Mathematica
`*.nb` files are notebook files for Mathematica. Currently, the only one reads data about band energies and renders it as charts.

## To-do
- Calculate charge density
- Calculate state density
- Add link to `parse_band_eigenvalues.py`