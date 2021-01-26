# MongoDB Driver
|:warning:|This documentation is still in construction|
|:---:|:----|

# Table of Content
0. [Introduction](#1-introduction)</br>
  0.1. [Blueprints](#01-blueprints)</br>
  0.2. [C++](#02-c)
1. [Creating a Connector](#1-creating-a-connector)</br>
  1.1. [Client](#11-client)</br>
  1.2. [Pool](#12-pool)
2. [Getting Data](#2-getting-data)
3. [Updating Data](#3-updating-data)
4. [Finding Data](#4-finding-data)
5. [Support](#5-support)

# 0. Introduction
## 0.1. Blueprints
The interface for Blueprints is the same as C++. You can also seemlessly mix both as the classes and structs are the same for both.

All Blueprints methods taking time to complete have several output execution pins instead of bindable delegates as it is easier to use.
## 0.2. C++
### 0.2.1. Setup
If you plan to use MongoDB-Driver with C++, add the following line to your `MyGame.Build.cs`:
```csharp
PublicDependencyModuleNames.Add("MongoDBDriver");
```
You can then includes the C++ files in your project:
```cpp
#include "MongoDB/Document.h"          // For the FDocumentValue structure.
#include "MongoDB/DatabaseConnector.h" // For the IDatabaseConnector interface. 
#include "MongoDB/Client.h"            // For the UMongoClient class.
#include "MongoDB/Pool.h"              // For the UMongoPool class.
```
### 0.2.2. Use the Plugin
The interface for C++ is the same as for Blueprints. C++ functions have a `Callback` instead of an execution pin as parameter with a boolean indicating if the operation succeeded and the data returned by the function if any. 

Here is a small example of how callbacks work:
```cpp
UMongoPool* const Pool = UMongoPool::CreatePool(TEXT("mongodb"), TEXT("127.0.0.1"), 27017, 5, 10);

if (Pool) // Pool creation fails if the URL is ill-formed.
{
    Pool->Ping(FMongoCallback::CreateLambda([](bool bSuccess) -> void 
    {
        if (bSuccess)
        {
            // Database responded to our ping !
        }
        else 
        {
            // Failed to ping database.
        }
    }));
}
```

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
|:warning:|The `Failed` exec is not executed when the document wasn't found but only if an error occured.|
|:---:|:---|
# 3. Updating Data
# 4. Finding Data
# 5. Support

