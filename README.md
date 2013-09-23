Clinical Data Warehouse
===
The purpose of this Project is to build an open-source, scalable data warehouse that will ensure the integration of multiple types of omics/bioinformatics data with deep clinical phenotype information obtained from clinical databases, registries and biobanks.

The European Project RD-Connect (http://rd-connect.eu/) supports this effort providing the resources and the uses cases that will drive the development of this data warehouse. In particular the system developed will be the main outcome of the project’s WP5 in which several groups participate. (for the full list of partners in the consortium see http://rd-connect.eu/about/partners/)

Due to the complexity of this task we would like to encourage all people to contribute with comments and issues/bug fixes requests. The project Wiki and the issue requests system will be the main channels for this communication.

Business cases
--------------

System arquitecture
-------------------
The initial software architecture is represented below:

![alt tag](https://raw.github.com/inab/cdw/develop/docs/imgs/infrastructure.png)

As in many others data warehouse solutions, data is imported into the system through an ETL layer. In this case however there are two issues that might compromise the performance of the system and should be carefully considered.

Lack of standards: In contrast with well established industries where huge efforts have been devoted to standardize the way data is exchanged, the field of bioinformatics and also the clinical domain are full of different data exchange formats.

Data size: after the arrival of the “Omic era” the explosion of data in bioinformatics has been enormous. For instance each NGS experiment produces data in the order of Terabytes. This data has to be processed, standardized and cleaned, all this demanding a considerable amount of computational resources and CPU time.

Due to these to factors the ETL layer has to be flexible enough to accommodate different kinds of data transformations and has to be efficient in order to manipulate all this data in a reasonable amount of time.  


Key enabling products/technologies
----------------------------------

- MongoDB http://www.mongodb.org/

Adopted Standards
-----------------

- SDTM http://www.cdisc.org/sdtm

Implementation plan
-------------------

- 2012

  - 1S
      - Determine information required from end-users (business cases)
      - Create a list of the data resources available 
      - Create a list of the standards available
      - Create a compilation of the key enabling technologies

  - 2S
      - Analysis of the datamodel requirements
      - Analysis of the infrastructure requirements
      - Selection of the adecuate technology 
      - First datamodel proposal
      - First arquitecture proposal

- 2013

  - 1S
      - First prototype of the system 

  - 2S

- 2014

  - 1S

  - 2S

- 2015

  - 1S

  - 2S

- 2016

  - 1S

  - 2S

References
----------

- http://bib.oxfordjournals.org/content/early/2013/05/14/bib.bbt031.full.pdf+html
