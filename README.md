# WeIO IDE API Documentation
The WeIO IDE API allows developers to connect their platform to WeIO IDE Frontend editor. It uses (JSON-RPC)[http://json-rpc.org] to communicate between server (pc or some specific electronics like rPI, WeIO, Micropython,...) and WeIO Frontend code editor. In that way editing and running files in network becomes semsless.  

##getInfo
Information about connected electronic board. When server requests getInfo, client will respond with json configuration file that is in the root path of the board

request | args example | description
------- | ------------ | -----------
getInfo | "" | Get information about electronics 

####Board responds with json structure
key | description | example(s)
---- | ----------- | ---------
name | Name of computer | name: "WeIO 2.0" (rPI, ESP8266,...)
processor | CPU name | processor: "ESP8266" (AR9331, BCM2837,...)
platform | Software platform running on electronics | platform:"micropython" (linux, nuttix, rtos, bare metal,...)
security | Security protocols supported by platform | to be defined
electrolink | Electrolink version | electrolink:"v1.0"
rootDirectory | Path to the main runnable program | rootDirectory:"/root"
storage | Available storage on the board in Mb | storage: {free: "0.8Mb", total:"2.0Mb"}
network.server | Address of Minflux server to connect to | to be defined
network.worldIp | Mainflux ip address that will represent your board | to be defined
network.lanIp | Ip address in LAN | lanIp: "10.0.0.10"
network.wireless | Wireless type if available | wireless: "wifi" (null if not available)
network.rssi | Strengts of wireless network if available 1-5 | rssi: 5 (null if not available)

##getFileTree
File tree structure with all names of files and directories. This request don't need path as argument because it will refer directly to it's internal json configuration key rootDirectory

request | args example | description
------- | ------------ | -----------
getFileTree | "" | Get file tree of project. Path is known by board. See getInfo rootDirectory 

####Board responds

```
[
{type: "file", label: "main.py", language: "python"},
{type: "file", label: "projectConfig.toml", language: "toml"},
{label: "etc", type: "folder", children: [{type: "file", label: "circle5.svg", language: null}]}
]
```

key | description | example(s)
---- | ----------- | ----------
type | Describes if it's a file or directory | type: "file"
label | File/directory name | label: "main.py"
language | File programming language, null if not recognized | language: "python"
children | Subdirectory content | {label: "etc", type: "folder", children: [{type: "file", label: "circle5.svg", language: null}]}

##getFile

request | args example | description
------- | ------------ | -----------
getFile | "somefile.py" | Get file. It will get /root/somefile.py

##saveFile
request | args example | description
------- | ------------ | -----------
saveFile | "somefile.py" | Save file. It will save /root/somefile.py

##removeFile
request | args example | description
------- | ------------ | -----------
removeFile | "somefile.py" | Remove file. It will remove /root/somefile.py

##createDirectory
request | args example | description
------- | ------------ | -----------
createDirectory | "/someFolder" | Create directory. It will create /root/someFolder

##removeDirectory
request | args example | description
------- | ------------ | -----------
removeDirectory | "/someFolder" | Remove directory. It will remove /root/someFolder

##run
request | args example | description
------- | ------------ | -----------
run | "" | Run main user program

##stop
request | args example | description
------- | ------------ | -----------
stop | "" | Stop main user program

##status
request | args example | description
------- | ------------ | -----------
status | "" | Get running user program status

##console
push | description
---- | ----------- 
["someData", "stdout"] | Standard output from console
["someData", "stderr"] | Standart error output from console
