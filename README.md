# hello-world-kong
This is a hello world plugin for kong

Adding a Hellow-World plugin
======================================================
#!/bin/bash

echo "Downloading KongAPIGatewayPOC repository"
echo "========================================"

git clone https://github.com/KalpanaSastry123/hello-world-kong.git

cd KongAPIGatewayPOC

mv hello-world /usr/local/share/lua/5.1/kong/plugins/

vi /usr/local/lib/luarocks/rocks/kong/0.10.1-0/kong-0.10.1-0.rockspec

# add the following lines at the bottom of the file in modules section
 
["kong.plugins.hello-world.handler"] = "kong/plugins/hello-world/handler.lua",
["kong.plugins.hello-world.access"] = "kong/plugins/hello-world/access.lua",
["kong.plugins.hello-world.schema"] = "kong/plugins/hello-world/schema.lua",

Run kong reload
#make sure reload was successful


Run the following:

#Adding Mockbin to the APIs managed by Kong
curl -i -X POST --url http://localhost:8001/apis/ --data 'name=mockbin' --data 'upstream_url=http://mockbin.com' --data 'hosts=mockbin.com'

#Configuring API to use the helloworld plugin
curl -i -X POST --url http://localhost:8001/apis/mockbin/plugins/ --data 'name=hello-world'

#Test API to check if header contains the Hello World message
curl -i -X GET --url http://localhost:8000/ --header 'Host: mockbin.com'
