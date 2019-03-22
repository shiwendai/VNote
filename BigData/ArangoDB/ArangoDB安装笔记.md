# ArangoDB安装笔记
* First add the repository key to apt like this: 
```
curl -OL https://download.arangodb.com/9c169fe900ff79790395784287bfa82f0dc0059375a34a2881b9b745c8efd42e/arangodb34/DEBIAN/Release.key
sudo apt-key add - < Release.key
```
* Use apt-get to install arangodb:
```
echo 'deb https://download.arangodb.com/9c169fe900ff79790395784287bfa82f0dc0059375a34a2881b9b745c8efd42e/arangodb34/DEBIAN/ /' | sudo tee /etc/apt/sources.list.d/arangodb.list
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install arangodb3e=3.4.4-1
```
* start stop restart arangoDB
```
/etc/init.d/arangodb3 start
sudo systemctl start arangodb3

/etc/init.d/arangodb3 stop 
sudo systemctl stop arangodb3

/etc/init.d/arangodb3 restart
sudo systemctl restart arangodb3
```
* Use the arangosh to create a new database and user.
```
arangosh> db._createDatabase("example");
arangosh> var users = require("@arangodb/users");
arangosh> users.save("root@example", "password");
arangosh> users.grantDatabase("root@example", "example");
```
* You can now connect to the new database using the user root@example.
```
shell> arangosh --server.username "root@example" --server.database example
```