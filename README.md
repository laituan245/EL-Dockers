# Entity Linking (EL)

This repo contains instructions for running the Wikidata EL system of UIUC's [Blender Lab](https://blender.cs.illinois.edu/).

Related paper:
```
Improving Candidate Retrieval with Entity Profile Generation for Wikidata Entity Linking
Tuan Manh Lai, Heng Ji, and ChengXiang Zhai
Annual Meeting of the Association for Computational Linguistics (ACL 2022) Findings
```

### news
- (December 8 2021) We added some minor updates to our EL system. Please pull the latest Docker `docker pull laituan245/wikidata_el_demo`.

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
After the ES container is started, you can then freely use our EL system to link mentions to Wikidata entities. The command is:
```
git clone https://github.com/laituan245/EL-Dockers.git
docker run --net=host --gpus '"device=0"' --rm -v ./EL-Dockers/samples:/shared laituan245/wikidata_el_demo --input_fp /shared/test_inputs.jsonl --output_fp test_outputs.jsonl
```

We provided a sample [input file](https://github.com/laituan245/EL-Dockers/blob/main/samples/test_inputs.jsonl) and the model's corresponding [output file](https://github.com/laituan245/EL-Dockers/blob/main/samples/test_outputs.jsonl). Please refer to the files to see the formats. For example, the second input in the file is:
```
Introduced in 2018 <m> BERT </m> is a powerful deep learning model
```
Our model's top candidates and their probability scores are:
```
Q61726893 (Bidirectional Encoder Representations from Transformers) - 0.9999063014984131
Q840922 (bit error rate) - 0.0036758678033947945
Q17087180 (Torch) - 2.56186837077621e-07
Q11589914 (Bert Kersey) - 8.861724154485273e-08
Q2890642 (Beit Berl College) - 2.688107514359217e-09
...
```

## Contact

If you have any feedback or question, please send me an email at *tuanml2@illinois.edu* or open a Github issue here.
