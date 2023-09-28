Repository notes from [Tutorial Elasticsearch Dasar (Bahasa Indonesia)](https://youtu.be/JfW7tg0yWsc?si=-WyReWV3ZBTCBFfC) video by Programmer Zaman Now on Youtube.

# ElasticSearch
ElasticSearch is a popular **search engine database** currently in use, widely adopted by many e-commerce platforms in Indonesia. ElasticSearch is built from an Information Retrieval library named Apache Lucene, developed using the Java programming language.

## RDBMS Terms vs ElasticSearch
| Relational DB Term | Elasticsearch Term |
|--------------------|--------------------|
| Database           |                    |
| Table              |   Index |
| Column             | Field / Attribute  |
| Row / Record       | Document (JSON)    |
| Join Table         | Embedded Document, Reference |
| SQL                | JSON |

## Instalation
Download archive file from official website (for Linux), then extract
```bash
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.10.2-linux-x86_64.tar.gz
    tar -xzvf elasticsearch-8.10.2-linux-x86_64.tar.gz
```

Open the extracted folder then configure `config/elasticsearch.yml`
```yml
    cluster.name: diccode-cluster
    node.name: diccode-1
    xpack.security.enabled: false
    http.port:9200
    path.data: data
    path.logs: logs
```

## Running
ElasticSearch will run on port 9200.
```bash
    bin/elasticsearch
```
Below is the respons from `localhost:9200`.
```JSON
    {
    "name" : "diccode-1",
    "cluster_name" : "diccode-cluster",
    "cluster_uuid" : "mBppz7bVQcqzBS_CuhraXQ",
    "version" : {
        "number" : "8.10.2",
        "build_flavor" : "default",
        "build_type" : "tar",
        "build_hash" : "6d20dd8ce62365be9b1aca96427de4622e970e9e",
        "build_date" : "2023-09-19T08:16:24.564900370Z",
        "build_snapshot" : false,
        "lucene_version" : "9.7.0",
        "minimum_wire_compatibility_version" : "7.17.0",
        "minimum_index_compatibility_version" : "7.0.0"
    },
    "tagline" : "You Know, for Search"
    }
```

## Embedded Document
We store data in JSON format, using either the reference or embedded approach. Elasticsearch automatically converts the JSON document structure into a Lucene document, formatted as a `Map<String, List<T>>`. Below is the example of Embedded Document.

<table>
<tr>
<th>elasticsearch.json</th>
<th>lucene.doc</th>
</tr>

<tr>
<td>

```json
{
    "_id": "12345",
    "first_name" : "Eko",
    "last_name" :  "Khannedy",
    "email": "eko@example.com",
    "age": 25,
    "hobbies": [
        "Coding" ,
        "Reading" ],
    "addresses" : [
        {
            "street": "Jl. Belum Jadi",
            "city": "Jakarta",
            "country": "Indonesia"
        },
        {
            "street": "Jalan Belakang",
            "city": "Subang",
            "country": "Indonesia"
        }
    ]
}
```
</td>
<td>

```json
{
    "_id": ["12345"],
    "first_name" : ["Eko"],
    "last_name" : ["Khannedy"],
    "email": ["eko@example.com" ],
    "age": [25],
    "hobbies": ["Coding", "Reading"],
    "addresses.street": ["Jl. Belum Jadi", "Jalan Belakang"],
    "addresses.city" : ["Jakarta", "Subang"],
    "addresses.country": ["Indonesia" ]
}
```

</td>
</tr>
</table>