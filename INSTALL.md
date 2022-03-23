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

Download the latest version of the [BloodHound GUI](https://github.com/BloodHoundAD/BloodHound/releases/download/rolling/BloodHound-linux-x64.zip)
~~~
wget https://github.com/BloodHoundAD/BloodHound/releases/download/rolling/BloodHound-linux-x64.zip
~~~

*Unzip the folder, then run BloodHound with the –no-sandbox flag:
~~~
./BloodHound.bin --no-sandbox
~~~

Authenticate with the credentials you set up for neo4j
Alternative: Build the BloodHound GUI Install [odeSource Node.js Binary Distributions](DEB.md)

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

## The BloodHound GUI
The BloodHound GUI is where the vast majority of your data analysis will happen. Our primary objectives in designing the BloodHound GUI are intuitive design and operational focus. In other words, we want you to get access to the data you need as [easily and quickly](USAGE.md) as possible.
