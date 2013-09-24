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

**ETL**

As in many others data warehouse solutions, data is imported into the system through an ETL layer. In this case however there are two issues that might compromise the performance of the system and should be carefully considered.

*Lack of standards:* In contrast with well established industries where huge efforts have been devoted to standardize the way data is exchanged, the field of bioinformatics and also the clinical domain are full of different data exchange formats.

*Data size:* after the arrival of the “Omic era” the explosion of data in bioinformatics has been enormous. For instance each single NGS experiment produces data in the order of Terabytes. This data has to be processed, standardized and cleaned; all this demands a considerable amount of computational resources and CPU time.

Due to these two factors, the ETL layer has to be flexible enough to accommodate different kinds of data transformations and has to be efficient in order to manipulate all this data in a reasonable amount of time.  

During the ETL process, the data from different entities (patient registries, Biobanks, research groups, etc.) will be inspected to ensure compliance with the long-term supported standards adopted by the consortium. (See the Adopted Standards section for a complete list)  

**Data repository**

The data repository will store the data needed to: 1) search the data warehouse integrating parameters related with the different original sources, 2) provide access to the original information in the form of secured links.

To illustrate the case let’s assume a patient with a spinocerebellar ataxia (SCA):

The patient clinical data is stored in one of the registries listed in the Ataxia Foundation (http://www.ataxia.org/research/patient-registry.aspx). The patient has been sequenced searching for Ataxia related genes and the FastQ files containing the unaligned reads from the exome sequencing experiment were deposited in the European Genome Phenome Archive (EGA, https://www.ebi.ac.uk/ega/)   

The data warehouse should allow authorized researchers to search for the spinocerebellar ataxia related conditions and ultimately retrieve the exome data. To facilitate this process we store in the data repository a subset of the patient clinical data previously normalized using the adopted standards. The repository also stores the experiment metadata including the software version, the parameters that were used to make the analysis and the link to the original files in the EGA.

Authorized researchers then can query the data warehouse using a controlled vocabulary to retrieve the list of all the patients that match the ataxia conditions, can then filter this information by the pipeline used to get the mutation data and finally get the link to the original files for further analyses.   



Key enabling products/technologies
----------------------------------

- Storage and data management technologies considered, focusing on scalability, query capabilities and sparse data storage. Most of it was tested with public BS Seq methylation data from ICGC, coming from CLL:
  
  - BioMart 0.8: discarded on long term, it has serious scalability issues hit in ICGC and BLUEPRINT.
  - Sparse storage:
      - HDF5: very efficient at storage level, but discarded, very difficult to build queries as it does not have a query processor
  - Relational databases with SQL'99 ARRAY type (e.g. PostgreSQL): discarded, not so sparse on dumps or updates
  - NoSQL databases: Very scalable, but their query processor is not as fancy as the SQL ones, as they are usually focused on map ... reduce paradigm.

Many products (like Riak or HBase) are not considered because they do not allow (or it is difficult) building compound indexes, based on several attributes/columns, or their storage paradigm is bizarre.

  - Apache Cassandra: Tested one year ago, with a custom query language called CQL. Although it is rather scalable, it was also rather slow storing the methylation test data, so discarded.
  - MongoDB: It is based on binary JSON documents paradigm (i.e. a hierarchical model). Very scalable, very fast data loads, lots of specialized indexes. As it doesn't have a query language as such, but API's to build streamlined queries, there are some queries easy to be built in SQL (i.e., joins) which much be rewritten in map ... reduce style.
  - RethinkDB: A promising NoSQL database, which, as MongoDB and other NoSQL databases, is based on JSON documents paradigm. But as it is still buggy, it has been discarded (for now).
  - Easy storage/specialized management of tabular data:
    - MySQL: discarded, slow index builds
    - tabix tools: fastest than anyone, manages BED, VCF, GFF, SAM, focused on chromosomal coordinate searches; but discarded, very difficult to build queries as it does not have a query processor. 
    - Apache Solr: based on Lucene, very scalable and promising, but as it is focused on incremental results fetch (like in search engines), it is slow when you ask for all the results.


Adopted Standards
-----------------

- SDTM http://www.cdisc.org/sdtm

Implementation plan
-------------------

- 2012

  - 1S
      - [x] Determine information required from end-users (business cases)
      - [x] Create a list of the data resources available 
      - [x] Create a list of the standards available
      - [x] Create a compilation of the key enabling technologies

  - 2S
      - [x] Analysis of the datamodel requirements
      - [x] Analysis of the infrastructure requirements
      - [x] Selection of the adecuate technology 
      - [x] First datamodel proposal
      - [x] First arquitecture proposal

- 2013

  - 1S
      - [ ] First release of the system 

  - 2S

- 2014

  - 1S
      - [ ] Second release of the system   

  - 2S

- 2015

  - 1S
      - [ ] Third release of the system 

  - 2S

- 2016

  - 1S
      - [ ] Fourth release of the system 

  - 2S
 

Legacy data examples
--------------------

**Genomics**

```
Chondrodysplasia.
Vissers LE, et al; Am J Hum Genet. 2011 May 13;88(5):608-15.
Chondrodysplasia and abnormal joint development associated with mutations in IMPAD1, encoding the Golgi-resident nucleotide phosphatase, gPAPP.
```

**Transcriptomics**

```
Skeletal dysplasia. Lausch E, et al; Nat Genet. 2011 Feb;43(2):132-7. Genetic deficiency of tartrate-resistant acid phosphatase associated with skeletal dysplasia, cerebral calcifications and autoimmunity.
```

**Proteomics**

```
Toxic Oil Syndrome. Rodriguez C, et al; Proteomics. 2006 Apr;6 Suppl 1:S272-81. Proteotyping of human haptoglobin by MALDI-TOF profiling: Phenotype distribution in a population of toxic oil syndrome patients.
```

**Metabolomics**

```
Cerebellar ataxia. Mochel F, et al; Brain 2009, 132(Pt 3): 801-9. Cerebellar ataxia with elevated cerebrospinal free sialic acid (CAFSA)
```

References
----------

- http://bib.oxfordjournals.org/content/early/2013/05/14/bib.bbt031.full.pdf
- http://www.epirare.eu/
