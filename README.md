# Enhancing rare disease research with semantic integration of environmental and health data 
[IJCKG2021: The 10th International Joint Conference on Knowledge Graphs](https://iswc2021.semanticweb.org/in-use-call)

## Authors
Albert Navarro-Gallinad, Fabrizio Orlandi, and Declan O’Sullivan

ADAPT Centre for Digital Content, Trinity College Dublin, Dublin, Ireland
School of Computer Science and Statistics, Trinity College Dublin, Dublin, Ireland

## Abstract
In rare disease research, Health Data Researchers (HDR) require additional types of data outside the health sector, like environmental data, to better understand the extrinsic factors that influence health outcomes. Knowledge Graph (KG) approaches are increasingly being used for data integration processes by combining clinical data with other data sources. However, using and directly navigating the combined data in the KG can be an obstacle for HDRs. To address this problem, the Semantic Environmental and Rare Disease data Integration Framework (SERDIF) was designed to hide the complexities for these researchers when exploring linked environmental observations with clinical data using a KG approach. The framework was evaluated by HDRs for a case study on Anti-neutrophil cytoplasm antibody (ANCA)-associated vasculitis (AAV) in Ireland, and promising usability and effectiveness results were observed. HDRs studying AAV were able to access, explore and export environmental related data to be used as input for their statistical models. SERDIF has the potential to be a solution for HDRs, who require a flexible methodology to integrate environmental data with longitudinal and geospatial diverse clinical data, in their hypothesis validation of environmental factors for rare disease research.

## The SERDIF approach
The framework is a combination of a methodology, a Knowledge Graph and a dashboard.
* **Methodology** is a series of steps that should be taken to define the necessary spatio-temporal data structures to combine clinical and environmental data. The methodology is divided into six main processes exemplified below.
* **Knowledge Graph**: hosted in a [GraphDB triplestore](https://graphdb.ontotext.com/)
* **Dashboard**: build using [Dash from Python](https://plotly.com/dash/)

### 1. Data Collection
This process requires gathering clinical, environmental and geometry data. In this case, the `data/raw/` [folder](https://github.com/navarral/iswc2021-in-use-paper/tree/main/data/raw) contains the necessary data (a sample) to reproduce the paper.

* Clinical data: simulated events within the Republic of Ireland
* Environmental data: weather and pollution sample data from [Met Éireann](https://www.met.ie//climate/available-data/historical-data) and [Environmental Protection Agency (EPA)](http://www.epa.ie/)
* Geometry data: counties and electoral district geometries for the Republic of Ireland

### 2. Semantic Uplift
This process designs a mapping to uplift the data gathered from the data collection process into the Knowledge Graph.

* Mappings: R2RML mappings for the data sources are stored in `data/mapping/` [folder](https://github.com/navarral/iswc2021-in-use-paper/tree/main/data/mapping). The R2RML-F engine used in this paper was provided by [chrdebru](https://github.com/chrdebru/r2rml)
* Uplift data (convert CSV -> RDF): open a terminal in the `data` folder of the project and run `./GenerateRDF.sh` to uplift the tabular data to RDF. This script will generate approximately 1GB of data, please make sure you have enough space available.
* Download triplestore: register to GraphDB and download the free version **standalone server** from their [website](https://www.ontotext.com/products/graphdb/graphdb-free/) (e.g. graphdb-free-9.7.0-dist.zip file)
* Locate the triplestore: unzip the .zip into the project's main folder (e.g. graphdb-free-9.7.0/)
* Open a terminal in the project's main folder
* Preload repository with generated RDF: run

`graphdb-free-9.7.0/bin/preload -f -c preloadSerdifToy.ttl data/rdf/EpaAirQDataHly.trig data/rdf/EpaAirQMetadata.trig data/rdf/MetDataHly.trig data/rdf/MetMetadata.trig data/rdf/rndEvIre.trig data/rdf/NGraph_GeohiveData.trig`

(NB: this is an example for graphdb-free-9.4.1, please edit according to the downloaded version)
* Run triplestore to load files: `graphdb-free-9.7.0/bin/graphdb` (NB: edit the version of the triplestore as in the previous step)
* Leave the triplestore running: open you browser and type in `http://localhost:7200/` to check if it is running

### 3. Data querying and filtering
This process defines a spatio-temporal query as a SPARQL template.
Optional: if you have expertise on SPARQL language, then you can inspect the content of the the `serdif_AppQueries.py` (otherwise skip this step)

### 4. Data visualization
This process designs an initial visual tool to grant meaningful access to domain experts.
Run `venv/bin/python3.6 serdif_App.py` in a new terminal on the main project's folder. If this does not work for you, please use

`source venv/bin/activate` 

`pip install -r requirements.txt`

`python serdif_App.py`

Then, type `http://0.0.0.0:5000/` in your browser to access the dashboard.

To explore the dashboard plese read the information on the main panel, proceed to input Query options from the left panel and then click the submit botton ad the end of the panel. Environmental-patient associated data is ready to be explored in the new generated tab 'Q1'!

### 5. Data exporting/downlift
This process exports combined and/or aggregated data from the Knowledge Graph in tabular format for analysis. The results from the SPARQL query can be exported as a table (CSV), which is the preferred input format for data analysis, through the dashboard.

### 6. Usability evaluation
The usability evaluation is provided as a [jupyter notebook](https://github.com/navarral/iswc2021-in-use-paper/blob/main/figures/FiguresNotebook_ISWC2021.ipynb).
If Github does not reder correctly the previous link from this repository, try the following from [nbviewer](https://nbviewer.jupyter.org/github/navarral/iswc2021-in-use-paper/blob/main/figures/FiguresNotebook_ISWC2021.ipynb).
