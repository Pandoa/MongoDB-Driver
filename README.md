# MongoDB Driver
|:warning:|This documentation is still in construction|
|:---:|:----|

# Table of Content
1. [Creating a Connector](#1-creating-a-connector)</br>
  1.1. [Client](#11-client)</br>
  1.2. [Pool](#12-pool)
2. [Getting Data](#2-getting-data)
3. [Updating Data](#3-updating-data)
4. [Finding Data](#4-finding-data)
5. [Support](#5-support)

# 1. Creating a Connector
## 1.1. Client
To create a client, use the `CreateClient` node:

![Blueprint Create Client Example](https://github.com/Pandoa/MongoDB-Driver/blob/main/Image/CreateClient.png?raw=true)

The parameters of this node are the following:
- `Protocole`: should be either `mongodb` or `mongodb+server`.
- `Address`: The address of your MongoDB database.
- `Port`: The port your MongoDB database is on.
- `AdditionalParameters`: Parameters added at the end of the URI. For example `a=b&c=d`.

## 1.2. Pool
![Blueprint Create Pool Example](https://github.com/Pandoa/MongoDB-Driver/blob/main/Image/CreatePool.png?raw=true)
# 2. Getting Data
# 3. Updating Data
# 4. Finding Data
# 5. Support

