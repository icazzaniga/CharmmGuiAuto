# CharmmGuiAuto

CharmmGuiAuto is a tool designed to automate interactions with CHARMM-GUI, a web-based graphical user interface for CHARMM (Chemistry at HARvard Macromolecular Mechanics), a widely used software for molecular simulations. This tool simplifies the process of preparing input files for molecular dynamics simulations.

## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Configuration parameters](#configuration-parameters)
- [Known Issues](#known-issues)
- [How to Contribute](#how-to-contribute)
- [Citation](#citation)
- [License](#license)

## Introduction

CHARMM-GUI is a powerful tool for preparing complex molecular systems for simulation, but its web interface can be cumbersome for repetitive tasks. CharmmGuiAuto aims to automate these interactions, streamlining the preparation process and reducing manual input errors.

## Features

- Automates the generation of MD input files using CHARMM-GUI.
- Simplifies the preparation of molecular dynamics simulations.
- Provides a set of example YAML files for easy customization.
- Integrates seamlessly with CHARMM-GUI web interface.

## Installation

To install CharmmGuiAuto, clone the repository and install the required dependencies:

```sh
git clone https://github.com/AmandaStange/CharmmGuiAuto.git
cd CharmmGuiAuto
micromamba create -n charmmauto -f requirements.txt -c conda-forge
micromamba activate charmmauto
```

## Usage

Here is a basic example of how to use CharmmGuiAuto:

1. Prepare your input YAML file. You can start with one of the examples provided in the `Example_input_yaml` directory.
2. Run the script with your YAML file as input.

```sh
python CharmmGuiAuto.py -i Example_input_yaml/MembraneProtein.yaml
```

## Example Input Files

The `Example_input_yaml` directory contains sample YAML files.
You can also create a new one or adapt the existing ones using the following parameters:

There are 3 different sections, and 3 required options in the input file. The sections are 'system_type', 'system_info', and 'details'. The first two required options are the email and password combination belonging to
the CHARMM-GUI account that is going to be used. This is specified in the input file under 'details':

```sh
details:
  email: ME@MAIL.com
  password: PASSWORD
```

The last required option is the system type. The script can handle 7 different types of systems:
1. Protein in solution (SP)
2. Protein in membrane (MP)
3. Membrane without protein (M)
4. Retrieval of a finished job (R)
5. Reading and manipulation of PDBs (PR)
6. Force field converter (FC)
7. Reading and manipulation of a PDB with an immediate conversion into a different FF (5+6) (RC)

This needs to be specified in the input file under 'system_type'. For a protein in solution that would be:


```sh
system_type:
  system_type: SP
```

The 'system_info' sectionis used to specify if the browser window should be visible(headless: false) or hidden (default, headless: true), and the path where the output should be placed after downloading (path_out)

```sh
system_info:
  headless: true
  path_out: /home/USER/Downloads/
```


The parameters of the 'details' section can be divided into parameters concerning:
- Universal
    - download_now (bool): Whether to download the output immediately (default is True).

- PDB manipulation
    - pdb_id (str): PDB ID for fetching the file 
    - model (any): Model selection options 
    - chains (list): List of chains to be patched 
    - het (str): Type of non-protein molecule 
    - pH (float): Desired pH value 
    - preserve (dict): Preserve hydrogen options (default is {'option': None}).
    - mutations (list): List of mutations to add 
    - protonations (list): List of protonations to add 
    - disulfides (list): List of disulfide bonds to add 
    - phosphorylations (list): List of phosphorylations to add 
    - gpi (dict): GPI anchor options (default is {'GRS': None}).
    - glycans (list): List of glycans to add 

- Solvation
    - waterbox (dict): Waterbox configuration (default is {'dis': 10.0}).
    - ion_method (str): Ion method 

- Membrane
    - boxtype (dict): Box type options 
    - lengthXY (dict): Length in XY directions options (default is "ratio", same as in CG)
    - lengthZ (any): Length in Z direction options 
    - lipids (list): List of lipids to add 
    - naas (list): List of N-acylated amino acids to add 
    - pegs (list): List of PEG lipids to add 
    - glycolipids (list): List of glycolipids to add 
    - size (int): Size of the system (default is 100).

- Membrane protein
    - orientation (str): Orientation option (default is 'PDB').
    - position (dict): Position options (default is {'option': None}).
    - area (dict): Area options (default is {'option': None}).
    - projection (dict): Projection options (default is {'option': None}).

- MD options
    - ff (str): Force field type (default is 'c36m').
    - engine (str): Simulation engine (default is 'gmx').
    - temp (float): Temperature in Kelvin (default is 310).

- Retrieval
    - jobid (str): Job ID to retrieve.

- Conversion
    - path (str): Directory path where the PSF file is located 
    - file_name (str): Name of the PSF file 
    - PBC (bool): Whether to set up periodic boundary conditions (default is False).
    - PBC_x (float): Size of the PBC box (default is 10).

While this might appear overwhelming it is less so in reality as many of these can be left at their default. For the parameters that have multiple options (such as the protonations) the values should match those written in the dropdown menus of the GUI.

There is an example input file for all system types with each containing comments on the parameters chosen, and if you need help with a specific setup then please just send a message.

It is recommend to setup your first input file by clicking along in the browser of CHARMM-GUI, and adding the parameter to the file everytime you change anything from the default.

 



## Known Issues

- Only works with Firefox.
- Cannot be used to continue retrieved jobs, but can download jobs that are finished but not yet downloaded if you have the job ID.
- Firefox binary (`firefox_binary.cpython-37.pyc` - possible original path `miniconda3/lib/python3.7/site-packages/selenium/webdriver/firefox/__pycache__/firefox_binary.cpython-37.pyc`) must be placed in the `bin` of your environment.

## How to Contribute

Contributions are welcome! 

If you would like to contribute:

1. **Fork** this repository.
2. **Clone** your fork locally.
3. **Create a branch** for your changes.
4. **Make your changes**, following PEP 8 style guidelines.
5. **Commit and push** your changes.
6. **Open a pull request**.

For detailed contribution guidelines, please see [CONTRIBUTING.md](CONTRIBUTING.md).

If you plan to contribute **a new feature** or **significant changes**, please open an issue first to discuss your idea.

## Citation

If you use CharmmGuiAuto in your research, please cite the following article:

Stange, A.D., Zuzic, L., Schiøtt, B., & Berglund, N.A. (2025). Exploring insulin-receptor dynamics: Stability and binding mechanisms. *Structure*, 33(8), 1–11. https://doi.org/10.1016/j.str.2025.04.022

BibTeX entry is provided in [citation.bib](citation.bib).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.