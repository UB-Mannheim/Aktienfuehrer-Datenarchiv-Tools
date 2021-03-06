![dfg](docs/img/dfg.jpg)
# ![Aktienführer-Datenarchiv-Tools](docs/img/akf_logo.png)

![license](https://img.shields.io/badge/license-Apache%20License%202.0-blue.svg)

## Overview

The [Aktienführer-Datenarchiv][archiv-link] include company data from
the ["Hoppenstedt Aktienführer"][hoppenstedt-link], a yearly publication about
companies traded at German stock markets. The database contains data from 1956-2016
and was created by different workflows using tools, which we will describe further here:
* Until 1999, the data was published in printed books, which were first digitized. Then:
  * The data from 1956-1975 were recorded automatically by OCR and subsequent
  parsing, see the [workflow for the books](#workflow-for-the-books) for more details.
  * The data from 1976-1999 were recorded manually by a "Double Keying" method.
  (This was done in the first project phase.)
* The CDs/DVDs from 2000-2016 were recorded by analyzing and parsing the HTML pages
from the discs, see the [workflow for the CDs](#workflow-for-the-cds) for more details.

The extracted data together with the tools were created in a project funded by
the Deutsche Forschungsgemeinschaft (DFG).


### Workflow for the Books

![process](docs/img/Arbeitsschritte_mit_Logos.PNG "Book workflow")

[crass][crass-link]
is a command line driven post-processing tool for scanned sheets of paper.
It crops segments, based on separator lines, and
splices them afterwards together in a certain order to gain standalone,
single-column text sections. This simplifies the layout analysis
for following OCR steps and bundles related text sections.
In an additional pre-processing step, which is called deskewing,
crass can also correct the rotation of the page.

[mocrin][mocrin-link]
is a command line driven processing tool for multiple OCR-engines.
The main purpose is to handle multiple OCR-engines with one interface for
a clean and uniform workflow. Another use is to serve as part of a self-configuration
process to extract the best settings for different OCR-engines (not fully implemented yet).
At the moment you can store multiple configuration files for the OCR-engines,
which can be modified manually. So you can have a big collection of preconfigured settings
for different input data.
Furthermore, it can cut areas out of images using some regular expressions,
which can be used as training datasets for NN-models.

[ocromore][ocromore-link] 
unites the best parts of the individual OCR-output to produce an optimal outcome.
In the first step, it parses the different OCR-output-files to a SQLite-database.
The purpose of this database is to serve as an exchange and store platform using
pandas as handler. Combining pandas and the dataframe-objectifier offers a
wide-range of high-performance use-cases like multi segment alignment (msa).
The combined output can be stored as a new OCR-result or in plain text file format.
To evaluate the result you can either use the common standard ISRI tool
to generate an accuracy report or do visual comparison with a diff-tool (default "meld").

[docxstruct][docxstruct-link]
analyses, categorizes and segments the data from text-based documents.
At the moment it is specialized to the `hocr` fileformat.
The contained data in each segment will be
parsed in a structured manner into a `json` file.

[akf-dbTools][dbTools-link]
is a collection of little scripts,
which can modify and update certain parts of the akf-database.
The scripts are customized to the requirements of the project.
Amongst other things it loads the database with json-files
from `docxstruct`, normalizes and deduplicates the data in the database.


### Workflow for the CDs

![process](docs/img/Arbeitsschritte_mit_Logos_CD.PNG "CD workflow")

[akf-cdparser][cdparser-link]
analyses, categorizes and segments the `HTML` files from the Aktienführer CDs.
The contained data in each segment will be
parsed in a structured manner into a `json` file.

[akf-dbTools][dbTools-link]
is a collection of little scripts,
which can modify and update certain parts of the database.
Amongst other things, it loads the database with json-files
from `akf-cdparser`, normalizes and deduplicates the data in the database.

Note that the automatic processing will sometimes need some manual adjustments.

### Additional tools & libraries

This sections provides additional tools that are also created or modified in the Aktienführer-Datenarchiv project.

[akf-corelib][corelib-link]
is a library for core functionalites, which get used by multiple projects in the Aktienführer-Datenarchiv work process.

[Betrial][betrial-link]
is a tool that reduces the amount of effort to perform a Bernoulli Trial for OCR-Validation. Given a pool of recognized pages it randomly selects characters, cuts the whole line and the corresponding, recognized text and generates a validation html-page.

[hocr-parser][hocrparser-link]
is a modified third party library for modules of the Aktienführer-Datenarchiv work process, but can also be used independently. Originally written by Athento.

## Copyright and License

Copyright (c) 2017 Universitätsbibliothek Mannheim

Author: 
 * [Jan Kamlah](https://github.com/jkamlah)
 * [Johannes Stegmüller](https://github.com/Hyper-Node)

The tools are Free Software. You may use them under the terms of the Apache 2.0 License.
See [LICENSE](./LICENSE) for details.

[archiv-link]: https://digi.bib.uni-mannheim.de/aktienführer/data/
[hoppenstedt-link]: https://de.wikipedia.org/wiki/Hoppenstedt_Aktienf%C3%BChrer
[crass-link]: https://github.com/UB-Mannheim/crass
[mocrin-link]: https://github.com/UB-Mannheim/mocrin
[ocromore-link]: https://github.com/UB-Mannheim/ocromore
[docxstruct-link]: https://github.com/UB-Mannheim/docxstruct
[dbTools-link]: https://github.com/UB-Mannheim/akf-dbTools
[cdparser-link]: https://github.com/UB-Mannheim/akf-cdparser
[corelib-link]: https://github.com/UB-Mannheim/akf-corelib
[hocrparser-link]: https://github.com/UB-Mannheim/hocr_parser
[betrial-link]: https://github.com/UB-Mannheim/BeTrial
