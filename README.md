# Solr VN Tokenizer

A Vietnamese tokenizer/analyzer plugin for Solr 8.5.2

The plugin supports `VietnameseAnalyzer` and `VietnameseTokenizer`.

* `VietnameseAnalyzer` combines `VietnameseTokenizer`, `LowerCaseFilter` and `StopFilter`.

#### Install

1. Build the plugin
```text
git clone https://github.com/tuan-nng/solr-vn-tokenizer
cd solr-vn-tokenizer
mvn package
```

2. Install plugin

Copy following directories and files into solr server directory (e.g. `<solr dir>/server/lib/vn-analyzer/`):
 * `local-maven-repo/VnCoreNLP/VnCoreNLP/1.2/models`
 * `targets/solr-vn-analyzer-1.1.jar`
 * `target/lib/*.jar`

```
$ tree server/lib/vn-analyzer/
server/lib/vn-analyzer/
|-- activation-1.1.1.jar
|-- commons-io-2.7.jar
|-- jaxb-api-2.3.0.jar
|-- jaxb-core-2.3.0.jar
|-- jaxb-impl-2.3.0.jar
|-- lucene-analyzers-common-8.5.2.jar
|-- lucene-core-8.5.2.jar
|-- models
|   |-- dep
|   |   `-- vi-dep.xz
|   |-- ner
|   |   |-- vi-500brownclusters.xz
|   |   |-- vi-ner.xz
|   |   `-- vi-pretrainedembeddings.xz
|   |-- postagger
|   |   `-- vi-tagger
|   `-- wordsegmenter
|       |-- vi-vocab
|       `-- wordsegmenter.rdr
|-- solr-vn-analyzer-1.1.jar
`-- VnCoreNLP-1.2.jar
```

3. Create new type for Vietnamese text

Assuming that you already created an index named `test`. Use this command to create a new field type for Vietnamese text

```text
curl -X POST \
  http://localhost:8983/solr/test/schema \
  -d '{
  "add-field-type": {
    "name": "text_vn",
    "class": "solr.TextField",
    "indexAnalyzer": {
        "class": "org.apache.lucene.analysis.vi.VietnameseAnalyzer"
    },
    "queryAnalyzer": {
        "class": "org.apache.lucene.analysis.vi.VietnameseAnalyzer"
    }
  }
}'
```

Above command will create a new `fieldType` in `managed-schema` file as below.
```text
<fieldType name="text_vn" class="solr.TextField">
    <analyzer type="index" class="org.apache.lucene.analysis.vi.VietnameseAnalyzer"/>
    <analyzer type="query" class="org.apache.lucene.analysis.vi.VietnameseAnalyzer"/>
</fieldType>
```
