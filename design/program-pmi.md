# program-pmi

program-pmi stands for Program Grant Management - PubMed Import. It is a web-based tool for importing a pubmed id (or hopefully a pmcid) into the PROGRAM-IN system (a REDCap project). Currently the system is a simple R/Shiny app but will be ported to a node.js containerized single-page webapp. Below is the design description for the node app.

# Zotero
The first thing to mention is that pubmed importing relies almost completely on the excellent zotero (http://zotero.org) project. If you haven't used it, I highly recommend it. We will be using it to scrape the pubmed record (they have robust "translators" to convert various web sources into references) and to format citations according to output styles. The wonderful zotero group has created various components which can be used stand-alone as web services for this purpose.

# Container components
There are three containers for the application: webui, translator, citeproc.

## webui
This is the web app that runs the process overall. The design for this app is as follows:

- Initial state
  - A text input box labelled PMID. 
  - A button1 labelled "Retrieve Pubmed Record"
- On button1 click
  - Call translator using the url form `https://pubmed.ncbi.nlm.nih.gov/NNNNNN` and save JSON return result
  - Call citeproc with the JSON return result, type is html and output style is (for now) MLA
  - Show citeproc output and enable button2 "Store Pubmed Record"
- On button2 click
  - Extract JSON fields from initial result and store in new JSON object mirroring the REDCap storage structure. Ideally, the REDCap format will be moved closer to Zotero format to make this easier (unify naming).
  - Post new JSON object to redcap server using predefined api uri and api key.
  - Show status of POST request on screen, disable button2, clear text in input text box to repeat process.

## translator
Zotero offers a docker container for a translator server (https://github.com/zotero/translation-server). Briefly, zotero can use the output of a web page to parse out a reference. This is brilliant, and in the context of the application means that you can click on browser button (using a plugin) when visiting a webpage and it will attempt to "translate" the page into a reference. They have packaged this functionality into a simple web service (in a docker container) so that you can send output from a page to this translator.

The process to start the server is
```
docker pull zotero/translation-server
docker run -d -p 1969:1969 --rm --name translation-server zotero/translation-server
```

Then you can pipe output from a pubmed search to the web service and get back a JSON API object. TODO: Find a link to their documentation of this format.

```
curl -d 'https://pubmed.ncbi.nlm.nih.gov/30894373' \
   -H 'Content-Type: text/plain' http://127.0.0.1:1969/web
```

## citeproc
Zotero also offers a citeproc server which allows you to convert (I think) the internal reference format to any number of formatted reference outputs (e.g., nature, science reference style). Again pretty cool. The repo is https://github.com/zotero/citeproc-js-server.

Note that it appears to require wrapping the node server in a docker container (I did not see a prebuilt one). 

Assuming the container is setup correctly, 
```
curl --header "Content-type: application/json" \
  --data @sampledata.json -X POST \
  'http://127.0.0.1:8085?responseformat=html&style=modern-language-association'
```
provides html output of MLA-formatted reference based on json input.


