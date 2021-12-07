# Entity Linking (EL)

## Running Instructions

### Knowledge Base (Wikidata)
The very first step is to start an ElasticSearch (ES) Docker container that has information of Wikidata entities. 
```
docker run --net=host  -e "discovery.type=single-node" laituan245/wikidata-es
```
It may take few minutes for ES to be fully loaded. To check whether ES has been fully loaded, you can run:
```
curl localhost:9200/en_wikidata/_count
```
The result should be 
```
{"count":40239259,"_shards":{"total":1,"successful":1,"skipped":0,"failed":0}}
```
Since there are 40,239,259 indexed Wikidata entities in our ES container.

### Entity Linking System
After the ES container is started, you can then freely use our EL system to link mentions to Wikidata entities.

## Contact

If you have any feedback or question, please send me an email at *tuanml2@illinois.edu* or open a Github issue here.
