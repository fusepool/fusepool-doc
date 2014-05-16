


# Prerequisites

## RDF Setup

NER will be done on all content in digest.rdf graph of your dataset which fullfils the following criteria:

* <myuri> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://fusepool.eu/ontologies/ecs#ContentItem> .
* <myuri> <http://rdfs.org/sioc/ns#content> "Some plain text where it will do the NER on"

## Creating the Dictionary Annotator

* Go to [http://localhost:8080/admin/graphs/](http://localhost:8080/admin/graphs/) and click on "UploadGraph"
* Create a new "Graph Name", the name does not really matter but you need to remember it later so copy it
* Upload the dictionary which meets the following criteria

<http://example.org/mytermxy> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://www.w3.org/2002/07/owl#Class> .
<http://example.org/mytermxy> <http://purl.org/dc/elements/1.1/subject> "MySubjectToFind" .

So the dictionary expects two triples per subject to find as of now

* Go to the [OSGi configuration console](http://localhost:8080/system/console/configMgr)
* Search for "Fusepool Enhancer Engine: Dictionary Annotator", click on the `+ sign
* Configure it based on the screenshot & Save
* Go to "Apache Stanbol Enhancer Chain: Weighted Chain", find the "Default" chain by randomly clicking in the available chains
* Add the *exact name* of the chain you just created to the default chain by clicking on the `+` sign


## Executing in the DLC

* Go to [DLC](http://localhost:8080/sourcing/)
* Create a new dataset
* Go to "Load RDF data", give the URL to your real data (not the dictionary)
* There is a fair chance this will not work so go to [http://localhost:8080/admin/graphs/](http://localhost:8080/admin/graphs/) and upload it to the graph from `urn:x-localinstance:/dlc/YourDataSetName/digest.graph`, you should see a triple count above > 0 hopefully
* Go to "Select a task", select "Compute enhancements"
* Now the CPU should create some load and go through the default chain where we added our NER as well
* When it returns, select "Smush" and "Publish"
* At the end of the "Publis" process it should show up in "content" graph in the [http://localhost:8080/admin/graphs/](http://localhost:8080/admin/graphs/) interface
* This is it from a NER perspective