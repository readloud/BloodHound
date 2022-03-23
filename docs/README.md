# BHDocs
[![Build](https://github.com/BloodHoundAD/BloodHound/actions/workflows/build.yml/badge.svg)](https://github.com/BloodHoundAD/BloodHound/actions/workflows/build.yml)
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/BloodHoundAD/BloodHound)](https://github.com/BloodHoundAD/BloodHound/releases/latest)

# Linux
## Install Java
Update your apt sources with this command:
~~~
echo "deb http://httpredir.debian.org/debian stretch-backports main" | sudo tee -a /etc/apt/sources.list.d/stretch-backports.list
~~~
Run apt-get update:
~~~
sudo apt-get update
~~~
neo4j will now automatically pull from that repo when it needs to install Java as part of its install process

## Install neo4j
Add the neo4j repo to your apt sources:
~~~
wget -O - https://debian.neo4j.com/neotechnology.gpg.key | sudo apt-key add -
echo 'deb https://debian.neo4j.com stable 4.0' > /etc/apt/sources.list.d/neo4j.list
sudo apt-get update
~~~
## Install apt-transport-https with apt
~~~
apt-get install apt-transport-https
~~~
## Install neo4j community edition using apt:
~~~
sudo apt-get install neo4j
Stop neo4j
systemctl stop neo4j
~~~

Start neo4j as a console application and verify it starts up without errors:
~~~
cd /usr/bin
./neo4j console
~~~

### Note
It is very common for people to host neo4j on a Linux system, but use the BloodHound GUI on a different system. neo4j by default only allows local connections. To allow remote connections, open the neo4j configuration file (vim /etc/neo4j/neo4j.conf) and edit this line:
~~~
#dbms.default_listen_address=0.0.0.0
~~~
Remove the # character to uncomment the line. Save the file, then start neo4j up again

Start neo4j up again. You have two options:

*Run neo4j as a console application:
~~~
cd /usr/bin
./neo4j console
~~~

*Or use systemctl to start neo4j:
~~~
systemctl start neo4j
~~~

Open a web browser and navigate to `https://localhost:7474/`. You should see the neo4j web console.
Authenticate to neo4j in the web console with username neo4j, password neo4j. You’ll be prompted to change this password.

## Download the BloodHound GUI

Download the latest version of the [BloodHound GUI](https://github.com/BloodHoundAD/BloodHound/releases/)
~~~
wget https://github.com/BloodHoundAD/BloodHound/releases/download/rolling/BloodHound-linux-x64.zip
~~~

*Unzip the folder, then run BloodHound with the –no-sandbox flag:
~~~
./BloodHound.bin --no-sandbox
~~~

Authenticate with the credentials you set up for neo4j
Alternative: Build the BloodHound GUI Install [Node.js Binary Distributions](https://github.com/readloud/BloodHound/blob/master/docs/Node.jsBinaryDistributions.md)

*Install electron-packager:
~~~
sudo npm install -g electron-packager
~~~
*Clone the BloodHound GitHub repo:
~~~
git clone https://github.com/BloodHoundAD/Bloodhound
~~~
*From the root BloodHound directory, run ‘npm install’
~~~
npm install
~~~
*Build BloodHound with ‘npm run linuxbuild’:
~~~
npm run build linuxbuild
~~~
or
*Build BloodHound with ‘npm run build’ to create `BloodHound-linux-xxx`:
~~~
npm run build
~~~

# The BloodHound GUI
The BloodHound GUI is where the vast majority of your data analysis will happen. Our primary objectives in designing the BloodHound GUI are intuitive design and operational focus. In other words, we want you to get access to the data you need as easily and quickly as possible.

## Authentication
When you open the BloodHound GUI for the first time, you will see an authentication prompt:

BloodHound logon screen
![](https://bloodhound.readthedocs.io/en/latest/_images/bloodhound-logon.png)

*Note
Want to follow along? Connect to the example hosted database:
~~~
DBurl: bolt://206.189.85.93:7687
DBusername: neo4j
DBpassword: BloodHound
~~~

- The “Database URL” is the IP address and port where your neo4j database is running, and should be formatted as bolt://ip:7687/
- The DB Username is the username for the neo4j database. The default username for a neo4j database is neo4j.
- The DB Password is the password for the neo4j database. The default password for a neo4j database is neo4j. 
- The password for the example database is `BloodHound`.
- Click “Login”, and the GUI will attempt to authenticate to neo4j with the information you provided.
- You can optionally click “Save Password” to automatically log in next time with the same info.

After successful authentication, the BloodHound GUI will do three things:

1. First, the GUI will perform a cypher query to ensure the graph schema has the appropriate indices and constraints. These operations prevent duplicate node creation and greatly speed up node lookup
2. Second, the GUI will collect stats about the database and display those stats in the “Database Info” tab.
3. Finally, the GUI will query for all users that belong to any Domain Admins group, then display those users and show how they belong to the Domain Admins group.

Upon successful logon, BloodHound will draw any group(s) with the “Domain Admins” in their name, and show you the effective users that belong to the group(s):

![BloodHound displaying the members of the Domain Admins group](https://bloodhound.readthedocs.io/en/latest/_images/bloodhound-initial-query.png)

## GUI Elements
### Graph Drawing Area
As much of the screen real estate as possible is dedicated to the graph rendering area - where BloodHound displays nodes and the relationships between them. You can move nodes around, highlight paths by mousing over a node involved in a path, and click on nodes to see more information about those nodes. You can also right click nodes and perform several actions against those nodes:

![Right click menu on a group node](https://bloodhound.readthedocs.io/en/latest/_images/right-click-group-node.png)

- Set as Starting Node: Set this node as the starting point in the pathfinding tool. Click this and you will see this node’s name in the search bar, then you can select another node to target after clicking the pathfinding button.
- Set as Ending Node: Set this node as the target node in the pathfinding tool.
- Shortest Paths to Here: This will perform a query to find all shortest paths from any arbitrary node in the database to this node. This may cause a very long query time in neo4j and an even longer render time in the BloodHound GUI.
- Shortest Paths to Here from Owned: Find attack paths to this node from any node you have marked as owned.
- Edit Node: This brings up the node editing modal, where you can edit current properties on the node or even add your own custom properties to the node.
- Mark Group as Owned: This will internally set the node as owned in the neo4j database, which you can then use in conjunction with other queries such as “Shortest paths to here from Owned”
- Mark/Unmark Group as High Value: Some nodes are marked as “high value” by default, such as the domain admins group and enterprise admin group. This can then be used with other queries such as “shortest paths to high value assets”
- Delete Node: Deletes the node from the neo4j database

You can also right click edges, then click “help” to see information about any particular attack primitive:

![Right click edge and get help](https://bloodhound.readthedocs.io/en/latest/_images/right-click-edge-help.gif)

Finally, there are two keyboard shortcuts when the graph rendering area has focus:

CTRL: Pressing CTRL will cycle through the three different node label display settings - default, always show, always hide.
Spacebar: Pressing spacebar will bring up the spotlight window, which lists all nodes that are currently drawn. Click an item in the list and the GUI will zoom into and briefly highlight that node.
Search Bar
In the top left of the GUI is the search bar. Start typing the name of a node, and the GUI will automatically recommend nodes that match what you’ve typed so far. Click one of the suggestions, and the GUI will render that node:

You can also constrain your search to particular node types by prepending your search with the appropriate node label. For example, you can search for just groups with the word “Admin” in them with this search:

![Search for nodes using the search bar](https://bloodhound.readthedocs.io/en/latest/_images/node-search.gif)

~~~
group:Admin
~~~

You can prepend your search with the following node types:

- Group
- Domain
- Computer
- User
- OU
- GPO

### Pathfinding

One of the most powerful features of BloodHound is its ability to find attack paths between two given nodes, if an attack path exists. Within the search bar is the “pathfinding” button, which brings down a second text box where you can type in the name of a node you want to target.

For example, if we wanted to find a path from the “Domain Users” group to the “Domain Admins” group, we can use the path finding feature like this:

![Search for an attack path](https://bloodhound.readthedocs.io/en/latest/_images/pathfinding.gif)

Depending on your opsec requirements or other factors, you may want to find attack paths that do not include particular attack primitives, such as AD object manipulation. Click the filter icon to bring up the edge filtering pane, and select or de-select the particular edges or class of edges as needed:

![Edge filtering pane](https://bloodhound.readthedocs.io/en/latest/_images/edge-filtering.gif)

### Raw Query Bar
With query debug mode enabled, any time the BloodHound GUI performs a cypher query where the results are shown in the graph rendering area, the cypher query itself will appear here. This can be helpful for learning cypher:

![Raw query bar](https://bloodhound.readthedocs.io/en/latest/_images/raw-query.gif)

Additionally, you can execute your own cypher queries using the raw query bar. Your cypher query must return either paths or nodes, the BloodHound GUI cannot render list output. For example, to return all “user” type nodes in the database:

![Run a raw cypher query](https://bloodhound.readthedocs.io/en/latest/_images/run-raw-query.gif)

### Upper Right Menu

In the upper right are several menu items for you to interact with. From the top going down:

- Refresh: Re-run the last query and display the results
- Export Graph: Export the currently rendered graph in JSON format
- Import Graph: Select a JSON formatted graph for the GUI to render
- Upload Data: Select your SharpHound data to upload to neo4j
- Change Layout Type: Switch between hierarchial or force directed layout
- Settings: Configure node and edge display settings, as well as query debug mode, low detail mode, and dark mode here.
- About: Displays author and version information
