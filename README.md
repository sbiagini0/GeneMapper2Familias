README
================

# *GeneMapper* to Multiple Families in Familias3

This repository contains a Python script designed for use with the
**DVI/MPI module in Familias3**. The script processes and structures
data exported from *GeneMapper*, preparing it for the **“Multiple
Families”** tool in Familias3. It facilitates semi-automated pedigree
construction, making it ideal for forensic analyses.

## Features

- **Data Loading**: Reads multiple *GeneMapper* text files in **vertical
  format** (where each row represents a single marker) from a specified
  directory and organizes the data into a structured DataFrame.
- **Data Cleaning, Transformation, and Standardization**:
  - Performs data cleaning by removing extra whitespace, handling
    missing values, and standardizing marker names.
  - Converts data from **vertical to horizontal format** after initial
    cleaning.
  - Constructs the **core pedigree structure** by ensuring the inclusion
    of primary family members (e.g., “1-PGF” for Paternal Grandfather,
    “2-PGM” for Paternal Grandmother, “3-MGF” for Maternal Grandfather,
    “4-MGM” for Maternal Grandmother, “5-FATHER” for Father, and
    “6-MOTHER” for Mother).
  - Adds a new column **`Relation`** to define each individual’s
    relationship within the pedigree (e.g., Paternal Grandfather,
    Mother), ensuring consistent structure for pedigree analysis.
  - Orders individuals by their `Individual ID` to maintain a coherent
    structure within each family pedigree.
- **Export**: Outputs the structured data to a text file in the
  appropriate format for **Familias3 DVI/MPI module**.

## Prerequisites

- `Familias3` software. To install the required software, go to this
  link: [Familias3](https://familias.no/)

- Python 3.x

  - Required libraries:
    - `glob`
    - `pandas`
    - `numpy`
    - `os`

Install the libraries using:

``` r
pip install glob pandas numpy os
```

## Data preparation

Each *GeneMapper* file should have *Sample Name* entries formatted as:
**`FamilyID-IndividualID-RelationshipID`**, allowing for easy
identification of relationships within each pedigree.

- Example: `FAM1-1-PGF`

  - **FAM1** is the family ID.
  - **1** is the individual ID.
  - **PGF** denotes the relationship ID related to paternal grandfather.

------------------------------------------------------------------------

------------------------------------------------------------------------

##### Family IDs

The `Family IDs` is a unique identifier assigned to each family and its
corresponding pedigree. This identifier helps distinguish each family
unit within the dataset, ensuring that all individuals and relationships
are organized under the correct family grouping for analysis.

##### Individual IDs

In this script, `Individual IDs` from 1 to 6 are reserved for the basic
pedigree structure, specifically for key family members such as parents
and grandparents. Beyond these initial IDs, numbers continue
sequentially to accommodate additional family members. This numbering
allows differentiation between multiple uncles, aunts, brothers, or
sisters, ensuring that each individual has a unique identifier within
the pedigree.

##### Relationship IDs

Below is the list of `Relationship IDs` supported by this script, all of
which are defined in relation to the **Missing Person (MP)**:

- Grandparents:
  - “PGF”: Paternal Grandfather
  - “PGM”: Paternal Grandmother
  - “MGF”: Maternal Grandfather
  - “MGM”: Maternal Grandmother
- Parents:
  - “FATHER”: Father
  - “MOTHER”: Mother
- Siblings:
  - “BROTHER”: Brother
  - “SISTER”: Sister
- Half Siblings:
  - “MHB”: Maternal Half Brother
  - “PHB”: Paternal Half Brother
  - “MHS”: Maternal Half Sister
  - “PHS”: Paternal Half Sister
- Uncles and Aunts:
  - “PU”: Paternal Uncle
  - “PA”: Paternal Aunt
  - “MU”: Maternal Uncle
  - “MA”: Maternal Aunt
- Children:
  - “SON”: Child
  - “DAUGHTER”: Child

**Note**: This *Familias3 tutorial* offers additional options for adding
relationship types not included in this example. Users can refer to the
[Familias3 Tutorial (page
95)](https://familias.name/tutorial/familias_tutorial_english.pdf) for
instructions on incorporating specific relationships as needed.

## Usage

1.  **Prepare Your Data:** Each *GeneMapper* file should include
    **Sample Name, Marker, Allele 1, and Allele 2** columns and the file
    naming convention.

##### Example input:

| Sample Name | Marker  | Allele 1 | Allele 2 |
|-------------|---------|----------|----------|
| FAM1-1-PGF  | D8S1179 | 10       | 13       |
| FAM1-1-PGF  | D12S391 | 18       | 23       |
| …           | …       | …        | …        |

2.  **Script Configuration:**
    - Place your files in the Input directory (or update the input_path
      variable in the script as needed).
3.  **Run the Script:** Execute the script to process the files.
4.  **Output:** The script returns a cleaned DataFrame, ready for import
    into **Familias3’s DVI/MPI module** for automatic pedigree
    construction.

##### Example output:

| Family id | Relation                 | Sample id | AMEL 1 | AMEL 2 | D8S1179 1 | D8S1179 2 | D12S391 1 | D12S391 2 |
|-----------|--------------------------|-----------|--------|--------|-----------|-----------|-----------|-----------|
| FAM1      | \[Paternal grandfather\] | 1-PGF     | X      | Y      | 10        | 13        | 18        | 23        |
| …         | …                        | …         | …      | …      | …         | …         | …         | …         |

## Example Dataset

An example dataset is provided in the **Example** folder, illustrating
the expected input and output format. The input file contains genetic
profiles from GeneMapper for multiple family members across different
pedigrees, formatted to include Sample Name, Marker, Allele 1, and
Allele 2 following the file naming convention. The output file
demonstrates the structured data ready for import into **module DVI/MPI
on Familias3**, with each individual’s relationship clearly defined
within their respective family pedigree. This example serves as a
reference for structuring your own datasets to ensure compatibility with
the script.

## Limitations

In the **Familias3** software, there is no built-in differentiation
between paternal and maternal uncles/aunts. The assignment of these
relationships in this script is automated based on the order in the
DataFrame: if paternal grandparents appear first, uncles/aunts will be
considered paternal. However, users can modify this ordering in the
script if they need to assign specific family relationships manually

In some cases, however, the relationship assignment is not automated,
and manual adjustment in **Familias3** may be necessary. Nonetheless,
this script greatly streamlines the setup process, especially when
working with large datasets, by pre-structuring most family
relationships.
