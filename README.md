# Knowledge Graph Construction using a low-code ETL approach

## Training Materials for Life Sciences Applications



Research fields generate a massive amount of data on a daily basis. However, without proper organization and analysis, researchers and data scientists are unable to efficiently search through the data to address research inquiries. Additionally, the rise of the FAIR principles in recent years places greater emphasis on making research data findable, accessible, interoperable, and reusable. Constructing knowledge graphs (KG) that depict the connections between existing data and knowledge entities can expedite the process of answering critical questions while adopting open standards and semantic web technologies to make the data FAIR and machine-readable. While semantic web and knowledge graphs have many benefits, including making data more FAIR, facilitating data integration, and enabling data sharing and reuse, they have a steep learning curve that requires a significant investment of time and effort by researchers. This can be a major barrier to the wider adoption of these technologies by the scientific community. 
To address this challenge, this training materials focus on bridging the gap between complex technology and scientific research by providing researchers with the knowledge and skills they require to construct knowledge graphs using a low-code workflow approach. The training materials will focus primarily on use cases from the life sciences domain. It will also include use cases from other disciplines obtained from the different. Moreover, it will explain how research questions are translated to knowledge graph queries and how they are executed against the available KGs. All the materials is available on GitHub under CC-By 4.0 license. 



### Knowledge Graph ETL

Knowledge graph ETL (Extract, Transform, Load) construction is a fundamental process in the realm of data integration and knowledge representation. This intricate procedure is designed to create and maintain knowledge graphs, which serve as powerful tools for organizing and connecting (semi)structured data from various sources into a coherent and insightful network of information.
The first step in knowledge graph ETL is extraction. This phase involves gathering data from diverse sources, such as databases, APIs, text documents, or even the web. The collected data can be in various formats, including structured databases, unstructured text, or semi-structured JSON or XML. During extraction, the data is often cleansed and transformed into a common format to ensure consistency and compatibility across the knowledge graph.
The transformation step is where a series of operations is applied to the data to  harmonize it and make it suitable for inclusion in the knowledge graph. This process may involve data normalization, data mapping, semantic annotation, and enrichment. The goal is to ensure that the data can be connected, linked, and queried effectively within the knowledge graph.
Finally, the loaded data is integrated into the knowledge graph. This involves creating nodes to represent entities and relationships to represent connections between them. The schema design plays a crucial role in this step, as it defines the structure of the knowledge graph and how different pieces of data relate to each other. Once the data is loaded, the knowledge graph becomes a rich repository of interconnected information, ready for querying and analysis.
The construction of a knowledge graph through ETL is not a one-time endeavor; it's an ongoing process. As new data becomes available or existing data evolves, the knowledge graph must be updated and expanded to reflect these changes. Additionally, maintaining data quality and ensuring the integrity of the graph is essential to derive meaningful insights and make informed decisions.



### Knowledge Graph Data Modeling

Knowledge graph data modeling is a pivotal aspect of creating structured and interconnected representations of information, fostering a deeper understanding of relationships among entities, concepts, and data points. At its core, knowledge graph data modeling involves designing the schema and defining the ontology that will govern the organization and storage of data within the knowledge graph.

- One of the key principles of knowledge graph data modeling is the use of nodes and edges. Nodes represent entities or concepts, while edges denote relationships between them. These relationships can be diverse, encompassing hierarchical, associative, or even complex semantic connections. By modeling data in this graph-like structure, it becomes possible to capture and represent rich, contextual information that goes beyond traditional tabular or relational databases.
- The schema design is a critical element of knowledge graph data modeling. It defines the types of entities, their attributes, and the relationships between them. This schema provides a blueprint for organizing data consistently and facilitates effective querying and analysis. It's essential to strike a balance between defining a comprehensive schema that captures the nuances of the domain and maintaining flexibility to accommodate evolving data needs.
- Ontology plays a crucial role in knowledge graph data modeling as well. It defines the semantics and vocabulary used in the knowledge graph, ensuring that data is consistently annotated and interpreted. Ontological modeling/reuse enables the graph to capture not only explicit relationships but also implicit connections and contextual information, allowing for more nuanced and insightful queries.
- Another aspect of knowledge graph data modeling is the incorporation of external knowledge sources. By integrating external ontologies or vocabularies, knowledge graphs can tap into broader knowledge ecosystems and enhance their own data with additional context. This enrichment process enables knowledge graphs to provide a more comprehensive and holistic view of the domain they represent.



### Development Environment

A portable environment is used in this training material to construct and query knowledge graphs using the Docker containerization technology. The containerized nature of the environment will allow researchers to quickly start their training process while wrapping all the technical complexities of installing and setting up the used tools. The technology stack to be used is composed of three pieces of software:

- LinkedPipes ETL: an open-source Extract, Transform, Load (ETL) tool that enables researchers and data scientists to create linked data workflows that transform data from various sources into Resource Description Framework (RDF) format. It provides a visual, drag-and-drop interface for creating ETL workflows that integrate heterogeneous data sources, such as spreadsheets, databases, and web services, into a single RDF knowledge graph. The low-code approach of this platform enables researchers to leverage the power of open standards like RDF and SPARQL to make their data more interoperable and reusable. 
- Virtuoso triple store: an open source database system that specializes in storing and managing RDF data and provides advanced query processing and inference capabilities to facilitate knowledge discovery and integration.
- SNORQL: a SPARQL explorer tool developed in-house by the applicant. It provides a browser-based query interface integrated with GitHub API to facilitate the storage, search and reuse of SPARQL queries residing in GitHub repositories.

Below is the docker-compose file needed to start the development environment (more details will follow up on the use of this stack):

```yaml
version: "3"
services:
  storage:
    image: linkedpipes/etl-storage:${LP_VERSION-main}
    volumes:
      - data-storage:/data/lp-etl/storage
      - configuration:/data/lp-etl/configuration
      - data-logs:/data/lp-etl/logs
    environment:
      - LP_ETL_DOMAIN
    restart: always
  frontend:
    image: linkedpipes/etl-frontend:${LP_VERSION-main}
    volumes:
      - configuration:/data/lp-etl/configuration
      - data-logs:/data/lp-etl/logs
    ports:
      - ${LP_ETL_PORT-8080}:8080
    environment:
      - LP_ETL_DOMAIN
    restart: always
  executor-monitor:
    image: linkedpipes/etl-executor-monitor:${LP_VERSION-main}
    volumes:
      - data-execution:/data/lp-etl/executor
      - data-logs:/data/lp-etl/logs
      - configuration:/data/lp-etl/configuration
    environment:
      - LP_ETL_DOMAIN
    restart: always
  executor:
    image: linkedpipes/etl-executor:${LP_VERSION-main}
    volumes:
      - data-execution:/data/lp-etl/executor
      - data-logs:/data/lp-etl/logs
      - configuration:/data/lp-etl/configuration
    environment:
      - LP_ETL_DOMAIN
    restart: always
  virtuoso-httpd:
    image: aammar/virtuoso-httpd:latest
    restart: always
    container_name: virtuoso-httpd
    environment:
      - DBA_PASSWORD=PASSWORD_HERE
      - SPARQL_UPDATE=true
      - SNORQL_ENDPOINT=ENDPOINT_URL
      - SNORQL_EXAMPLES_REPO=EXAMPLE_REPO_URL
      - SNORQL_TITLE=INTERFACE_TITLE
    ports:
      - "8899:8890"
      - "1133:1111"
      - "8088:80"
      - "4430:443"
volumes:
  data-logs:
  data-execution:
  data-storage:
  configuration:
```



### Life Sciences Knowledge Graphs:

[Bio2RDF](kg/bio2rdf.md)

[DisGeNet](kg/disgenet.md)

[UniProt](kg/uniprot.md)

[ChEMBL](kg/chembl.md)

[Wikipathways](kg/wikipathways.md)