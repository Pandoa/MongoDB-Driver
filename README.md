# [MongoDB Driver](https://www.unrealengine.com/marketplace/en-US/product/mongodb-driver)
![MongoDB Driver features](https://cdn1.epicgames.com/ue/product/Screenshot/Slide2.PNG-1920x1080-43c0f67bd6e661c8ebb0ba6c791e1605.jpg)
# Table of Content
0. [Introduction](#1-introduction)</br>
  0.1. [Blueprints](#01-blueprints)</br>
  0.2. [C++](#02-c)
1. [Creating a Connector](#1-creating-a-connector)</br>
  1.1. [Client](#11-client)</br>
  1.2. [Pool](#12-pool)
2. [Getting Data](#2-getting-data)
3. [Updating Data](#3-updating-data)
4. [Other Functionalities](#4-other-functionalities)
5. [Troubleshoting](#5-troubleshoting)
6. [Support](#5-support)

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
You can then include the C++ files in your project:
```cpp
#include "MongoDB/Document.h"          // For the FDocumentValue structure.
#include "MongoDB/DatabaseConnector.h" // For the IDatabaseConnector interface. 
#include "MongoDB/Client.h"            // For the UMongoClient class.
#include "MongoDB/Pool.h"              // For the UMongoPool class.
```
### 0.2.2. Using the Plugin
The interface for C++ is the same as for Blueprints. C++ functions have a `Callback` instead of an execution pin as parameter with a boolean indicating if the operation succeeded and the data returned by the function if any. 

Here is a small example of how callbacks work:
```cpp
// Create a Pool.
IDatabaseConnector* const Connector = UMongoPool::CreatePool(TEXT("mongodb"), TEXT("127.0.0.1"), 27017, 5, 10);

// Or a client.
// IDatabaseConnector* const Connector = UMongoClient::CreateClient(TEXT("mongodb"), TEXT("127.0.0.1"), 27017);

if (Connector) // Pool creation fails if the URL is ill-formed.
{
    // With a lambda
    Connector->Ping(FMongoCallback::CreateLambda([](bool bSuccess) -> void 
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
    
    // Or with an UObject's UFUNCTION
    Connector->Ping(FMongoCallback::CreateUObject(this, &UMyClass::MyFunc));
}
```

Note that you can't expose directly `IDatabaseConnector` pointers to Blueprints. You must use the following syntax:
```cpp
UPROPERTY()
TScriptInterface<IDatabaseConnector> Connector;
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

|:information_source:| If you want to connect to Atlas, you can use the `CreatePoolForAtlas` node. If you want to directly use a custom URI, the `CreatePoolFromURI` node can be used. |
|:---:|:---

# 2. Getting Data
To get data, you can use either `Find` to get several entries or `FindOne` to get a single document.

![FindOne example](https://github.com/Pandoa/MongoDB-Driver/blob/main/Image/FindOne.png?raw=true)
|:warning:|The `Failed` exec is not executed when the document wasn't found but only if an error occured.|
|:---:|:---|

|:information_source:| To know exactly what is returned, you can call the `ToJson` node to convert a Document Value to JSON. You can then print it to see exatly what it contains. |
|:---:|:---|

# 3. Updating Data
To update existing data, you can use `UpdateOne` or `UpdateMany` depending on how many documents you want to update.
The `UpdateOne` node is used as followed:

![Update One Example](https://github.com/Pandoa/MongoDB-Driver/blob/main/Image/UpdateOne.png?raw=true)
# 4. Other Functionalities
The following nodes are available as well:
- `ListDatabases`: Lists all databases.
- `DropDatabase`: Drops the specified database.
- `CreateCollection`: Creates a new collection on the specified database.
- `DropCollection`: Drops a collection in the specified database.
- `InsertOne`: Inserts a document into the collection.
- `InsertMany`: Inserts many documents into the collection.
- `ListIndexes`: Lists the indexes in the collection.
- `RenameCollection`: Renames a collection.
- `ReplaceOne`: Replaces a single document matching the provided filter in the specified collection.
- `UpdateMany`: Updates multiple documents matching the provided filter in the specified collection.
- `UpdateOne`: Updates a single document matching the provided filter in the specified collection.
- `CountDocuments`: Counts the number of documents matching the criteria.
- `GetEstimatedDocumentCount`: Get the estimated count of documents in the collection. 
- `CreateIndex`: Creates an index over the collection for the provided keys with the provided options.
- `DeleteMany`: Deletes all matching documents from the collection.
- `DeleteOne`: Deletes a matching document from the collection.
- `Find`: Finds the documents in the collection which match the provided filter.
- `FindOne`: Finds the document in the collection which match the provided filter.
- `FindOneAndDelete`: Finds a single document matching the filter and deletes it.
- `FindOneAndReplace`: Finds a single document matching the filter and replaces it.
- `FindOneAndUpdate`: Finds a single document matching the filter and updates it.
- `RunCommand`: Runs a command against the database.
- `ListCollectionNames`: Lists the names of the collections in the database.
- `Ping`: Pings the database.

# 5. Troubleshoting
If you have a problem, the first thing to do is check the output log. Each time the `Failed` pin is fired, a meaningful message with a reason is printed to the logs.
## 5.1. Error `No suitable servers found`
This error means there was no server where your URI points to. The reason is most likely an invalid URI. Make sure the URI you are using allows you to connect from the device where you run the Engine.

# 6. Support
If you need help, have a feature request or experience troubles, please contact us at [pandores.marketplace@gmail.com](mailto:pandores.marketplace+MongoDBDriver@gmail.com?subject=MongoDBDriver%20-%20).
