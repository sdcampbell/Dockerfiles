Credit: https://twitter.com/petergombos/status/1184793909911867392

Download binary release for your O.S. and unzip: https://github.com/BloodHoundAD/BloodHound/releases

Run neo4j:

```
docker run --rm -p 7474:7474 -p 7687:7687 --env NEO4J_AUTH=neo4j/BloodHound --name bloodhound -d peterhgombos/bloodhounddemo:1.0
```

Run Bloodhound binary.
