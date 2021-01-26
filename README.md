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
- `Additional Parameters`: Parameters added at the end of the URI. For example `a=b&c=d`.

## 1.2. Pool
To create a pool, use the `CreatePool` node:

![Blueprint Create Pool Example](https://github.com/Pandoa/MongoDB-Driver/blob/main/Image/CreatePool.png?raw=true)

The parameters of this node are the following:
- `Protocole`: should be either `mongodb` or `mongodb+server`.
- `Address`: The address of your MongoDB database.
- `Port`: The port your MongoDB database is on.
- `Min Pool Size`: The minimum size of the pool (i.e. the number of clients when there is no task in the pool).
- `Max Pool Size`: The maximum size of the pool (i.e. the number of clients when the pool is busy).
- `Additional Parameters`: Parameters added at the end of the URI. For example `a=b&c=d`.

|:information_source:| It is recommended to use a pool instead of a client. |
|:---:|:---|

# 2. Getting Data
To get data, you can use either `Find` to get several entries or `FindOne` to get a single document.

![FindOne example](https://github.com/Pandoa/MongoDB-Driver/blob/main/Image/FindOne.png?raw=true)
|:warning:|Failed is not executed when the document wasn't found but only if an error happened.|
|:---:|:---|
# 3. Updating Data
# 4. Finding Data
# 5. Support

