# Elastic Search install on CentOS7
#### https://www.howtoforge.com/tutorial/how-to-install-elastic-stack-on-centos-7/
------------------------------------------------------------------------------------------------------------------------------------
# Elastic Search install on Ubuntu

### Set up port forwarding in the network section of VMBox.
### Add ports:
### Elasticsearch, TCP, 127.0.0.1, 9200, blank, 9200
### Kibana, TCP, 127.0.0.1, 5601, blank, 5601
### SSH, TCP, 127.0.0.1, 22, blank, 22

### Elasticsearch runs on top of java

### We need to update repositories so that ubuntu/centos can find it
`wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -`

### safety install
`sudo apt-get install apt-transport-https`

### Add these packages to to this
`echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list`

### Update repos and install elasticsearch
`sudo apt-get update && sudo apt-get install elasticsearch`

### Update default configuration
`sudo vi /etc/elasticsearch/elasticsearch.yml`

### Scroll down to network.host and uncomment it.  Change to 0.0.0.0 so that it opens up to other things like your pc or host computer

### Need to run elasticsearch and boot everytime we open it
`sudo /bin/systemctl daemon-reload`

`sudo /bin/systemctl enable elasticsearch.service`

### Start elasticsearch with
`sudo /bin/systemctl start elasticsearch.service`

### Verify it's running
`curl -X GET "localhost:9200/"`

### Test data William shakespeare's works
`wget http://media.sundog-soft.com/es6/shakes-mapping.json`

### Tells elastic search how to store this stuff on the backend
`curl -H "Content-Type: application/json" -XPUT '127.0.0.1:9200/shakespeare' --data-binary @shakes-mapping.json`

### Retrieve shakespeare data
`wget http://media.sundog-soft.com/es6/shakespeare_6.0.json`

### Take a look at it
`less shakespeare_6.0.json`

### Submit all the works of shakespeare into our database, this will take a few minutes.
`curl -H 'Content-Type: application/json' -XPOST 'localhost:9200/shakespeare/doc/_bulk?pretty' --data-binary @shakespeare_6.0.json`

### Time to query that data.
`curl -H "Content-Type: application/json" -XGET '127.0.0.1:9200/shakespeare/_search?pretty' -d '`

### Enter this information to match to phrase
`
> {
> "query": {
> "match_phrase": {
> "text_entry": "to be or not to be"
>}
>}
>}'
`
