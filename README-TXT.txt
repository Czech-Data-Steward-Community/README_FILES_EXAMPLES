LICENSE Notice for the README file template and examples

This README template containing examples is authored by the Collective of Data Stewards of the Czech Data Stewards Community and is licensed under a Creative Commons Attribution 4.0 International License (CC BY 4.0) (https://creativecommons.org/licenses/by/4.0/). The full list of authors can be found here: https://github.com/Czech-Data-Steward-Community/README_FILES/blob/main/Collective_datastewards.md (https://github.com/Czech-Data-Steward-Community/README_FILES/blob/main/Collective_datastewards.md)

This template was based on the README Template Suite for Archaeology created by Tobiáš Kolmačka (ORCID: 0009-0006-7760-7320) and developed to support researchers at the Institute of Archaeology of the Czech Academy of Sciences (IAP = ARÚ).

This template aims to help you create a README for your data. Fill in only the sections that are relevant to your dataset; delete the rest.

GUIDELINES

- Anything inside brackets [ ] is an example and should be replaced with real answers or be deleted.
- The symbol > signifies comments to the user, and should be deleted after the README is filled.
- The symbols - - - signify separation of sections and do not need to be deleted.
- The README file name starts with an underscore _ in order to make it appear first in the list of files, when sorted alphabetically.
- This file is in simple text format. For markdown you can use the file here: https://github.com/Czech-Data-Steward-Community/README_FILES/blob/main/README-MD-txt.md
- After filling in the README you can delete the first page with instructions. 

- - -  YOUR TEMPLATE STARTS HERE - - -

Specific discipline examples of this file: [add discipline]

DATASET NAME: [Data name]

BASIC INFORMATION

Author(s):  
[First name, last name, ORCID;  
First name, last name, ORCID]

Main contact: email@example.cz (mailto:email@example.cz)  
Dataset license: [CC-BY 4.0]  
README version: [_README_v1-0]  
PID: https://doi.org/xxxxxxxx (https://doi.org/xxxxxxxx)  
Project website: https://bestproject.cz (https://bestproject.cz)

DESCRIPTION

Brief Description

> Write 2–3 sentences describing what the dataset contains and what it is used for.

[discipline specific example here]

Research Context

> Write a brief description of the research project in which the data were produced. What were the goals? What questions was the research intended to answer?

[discipline specific example here]

Related Publications

> List of publications (articles and data) where the data were used or presented, or if they are relevant to understanding the dataset.

- [(Annotation, e.g. Data retrieval) Citation of publication 1]
- [(Annotation, e.g. Methodology instructions) Citation of publication 2]

DATASET/COLLECTION CONTENTS

Directory Structure

> Describe the directory structure. If the structure is complex, you may describe it in text and/or visually using a directory tree, like the one below.

[

- data/
  - raw/ : Original, unprocessed data
  - processed/ : Processed data
  - final/ : Final data for analysis
- _README_v1-0 : This file
- documentation/ : Additional documentation
- results/ : Analysis results
- scripts/ : Processing scripts  

]

File Descriptions
  
> List the data files showing what each file contains. For large datasets, refer to an accompanying file description. If the content is evident from the file name, describe only the directory structure.

[

- File name.format
- Description
- Instructions for use
- xxxxx
- File name.format
- Description
- Instructions for use
- xxxxx  

]

DATA STRUCTURE

Encoding

> Fill in according to the nature of the data,

(e.g. for tabular data)  

[  

Text file encoding: UTF-8  
CSV delimiter: semicolon (;) or comma (,)  
Decimal separator: period (.) or comma (,)  

]

Variable Descriptions

> List the variables of the data. Include the variable name, data type, unit (if relevant), a brief description, and examples or value ranges where applicable.

[
  
- Variable, type, unit
- Description
- Examples / value range
- Variable, type, unit
- Description
- Examples / value range  
  
]

Special Values

> Explain how special values are represented in the data, such as missing data, unknown values, infinity, etc. Specify which concrete values or codes are used for these purposes.

[discipline specific example here]

METHODOLOGY-PROVENANCE

Collection/Creation of Data

> Describe how the data were obtained: methods, tools, time period, collection location, etc.

- Collection date: [from YYYYMMDD to YYYYMMDD]
- Collection location: [geographic location, or online]
- Tools for collection:  

[

- software
- hardware
- questionnaires
- etc. used  

]

- Original data:

> Describe the original data including DOI, if available

[discipline specific example here]

- Method of collection/production:

> Describe the method of data collection/creation

[discipline specific example here]

DATA PROCESSING

Processing

> If the data were produced by transforming other data, describe the original data and the transformation applied. Describe the data processing steps: cleaning, transformation, anonymisation, formatting etc.

Method of Data Processing

> Add the software names (with version) used for the data processing. You can also simply describe the processing workflow, if no software was used.

[

- Software 1, version  
Description
- Software 2, version  
Description  

]

Data Completeness

> Describe data quality, known limitations, missing values, etc. How was the quality tested? Did you perform an integrity check?

[discipline specific example here]

REUSE

License and Terms of Use

> Include the license text or link where it can be found, any copyright notice and any specific conditions for use of the data, for example if individual files are licensed differently.

[discipline specific example here]

Restrictions and Recommendations

> State any additional restrictions or recommendations for use.  

[discipline specific example here]

Software and Hardware Requirements

> Describe the software and tools required to work with the data.

[

- Software:
- Hardware:  

]

PROPOSED CITATION

> You may also include a dataset citation of a specific style, or use the template below:  
[Creator (Publication Year): Title. Publisher. (Resource Type). PID. Version]  
OR  
> Please provide the dataset citation metadata in .bib and/or .cff format and reference the corresponding file(s) in the README.  
[Citation for this dataset, compatible with Zotero and similar managers, are provided in the _citation.bib and/or _citation.cff file.]

ETHICS AND DATA PROTECTION

> Fill in only if the data contain personal data or sensitive information. If the data contain no personal data, delete this section.

Personal and Sensitive Data Protection

> Describe how the protection of personal/sensitive data was ensured, if the data contains sensitive information.

Ethical Approval

> If ethical approval was required, state:  

[Approved by the ethics committee name, approval number: xxx, date: YYYYMMDD.]

Informed Consent

> Provide information on whether and how informed consent was obtained from participants.

[discipline specific example here]

ACKNOWLEDGEMENTS

> You may thank collaborators, research participants, funding providers, etc.

This README was based on the template/examples authored by the Collective of Data Stewards of the Czech Data Stewards Community (https://github.com/Czech-Data-Steward-Community/README_FILES/blob/main/Collective_datastewards.md) and is licensed under a Creative Commons Attribution 4.0 International License (CC BY 4.0) (https://creativecommons.org/licenses/by/4.0/).

README FILE CHANGE HISTORY

[  

_README_v1-0 (YYYYMMDD) Initial publication of the dataset.  
_README_v1-1 (YYYYMMDD)  Describe changes from the previous version.  

]
