# xds
XDS is an acronymn for X as in "unamed" and DS as in Database System. So in essence, XDS stands for the Unamed Database System.

It is a high-level, multi-faceted database architecture which provide client-side functionalities all the way to the server-side, allowing for built-in functionalities, such as:

CRUD-QX API
-----------------------------------------------------------------------------------------------------------
Provide support for CRUD functionality (Create, Read, Update and Delete functions), extended with a
Query and eXecute API.

Flexible Model Architecture
-----------------------------------------------------------------------------------------------------------
Each model is governed by a "Domain:ModelName" naming scheme allowing for a unique model namespace.

All models have a "named" set of properties where each property consists of a type and value. In addition,
meta-data provides additional information for the model and for properties (ie: indexing and enhabced string
support, etc). When creating models, you can also define default values for each data type. All model 
references are made via ID references. It is also very easy to associate a collection of references.

The simplistic approach here basically illiminates boilerplate code (DAO's and DTO's, etc).

Fine-Grained API Control
-----------------------------------------------------------------------------------------------------------
Every call to the CRUD-QX can be controlled for each invoking user. So for example, you may want to limit
"update" requests on a model family of "MyDomain:MyModel" to a maximum of 1000 calls per hour for the
given user or a set of users.

Or, you may want to limit "create" requests on the domain (all models) of "MyApp:MyDomain:\*" to a maximum
of 1000 per day for the given user or a set of users.

Revisioned Data
-----------------------------------------------------------------------------------------------------------
Provide optional data revisioning for each model. When enabled, will ensure every update is a new entry
and previous revisions are kept (depending on configured auto-purging policies).

Revision 0 always infers to the "latest" (most recent) instance of the model. Revision offset can also be 
used by using a negative number such as -1 (last revision), -2 (second last revision), and so on. Abssolute 
revision numbers can also be used, such as 1 (first every copy), 2 (second update), 33, 55 and so on.

Fine-grained Audit Trail
-----------------------------------------------------------------------------------------------------------
With a simple configuration, a detailed audit trail can be recorded for each model or on a per-property 
basis. So if an audit trail is configured for a model, every CRUD action on such model will record the
user, session, date, time, application context and action invoked on the model instance. Optionally, the
actual change can be recorded also (great to track actual changes when revisions are auto-purged). Audit
trails can also be triggered on specific properties instead of the entire model.

Auto-Sharding and Auto-Replication
-----------------------------------------------------------------------------------------------------------
With a simple configuration, any model can be sharded using a number of strategies (key, hash, range, etc.
or composite configurations). Any model can also easily be replicated n-times.

As part of the sharding/replication architecture, it is relatively simple to configure for "locality" of
the data.

Example replication configuration for the "MyDomain:SomeUser" model:

 * 1 replication in group "NorthAmerica" and in zone "Zone 1"
 * 1 replication in group "NorthAmerica" and in zone "Zone 2"
 * 1 replication in group "Europe" and in zone "\*" (meaning "any" zone, determined by the system)

ASYNC/Even-Driven API
-----------------------------------------------------------------------------------------------------------
Every call to the CRUD-QX API is ASYNC and fully event-driven, meaning that the call will return immediately
with a transaction ID. As the request is being fullfilled in-part or in-whole, previously registered events
will be invoked.

Powerful Event-Based Framework
-----------------------------------------------------------------------------------------------------------
Events can be registered on entire model domains, model families or specific model instances. This allows
for a very powerful event system. For example, should one want to know if/when a new instance is created
for the "Player" model, a "create" event can be registered on "SomeGame:Player:\*". Every instance created
(server side) would trigger all registered event handlers on every client.

Model Leasing Framework
-----------------------------------------------------------------------------------------------------------
Also provide support for "leasing" models for a given timeframe. So if client 1 is currently displaying
a user model ID 1234, they can register such model on the "lease request" event. If client 2 requests to
"lease" user model ID 1234, client 1 will receive the event and can either prevent the user from clicking
an edit button or provide a user feedback that the even is being edited by someone else. Should client 1
allow the user to click on the edit button, thus invoking a "lease request" on model 1234, the server
would refuse such request. Should the client implementation allow the user to edit model 1234 and send
an model "update" request, the server will reject and refuse such update because the model is currently
under a lease. A lease has a determined auto-expiry, either using default config or during a lease request.

RBAC
-----------------------------------------------------------------------------------------------------------
Full "Role Based Access & Control" support is built into the datbase framework, providing for fine-grained
permission control over who can access and do what to each domain, model family, model instance or even
a model's properties.

Flexible, Multi-Level Caching
-----------------------------------------------------------------------------------------------------------
Built-in per-model caching can be provided for each client configuration and easily configurable on the
server as well. This allows client code to fully optimize at the client-level and also allows for servers
to provide cache optimizations as required without having to provide any code.

API Options
-----------------------------------------------------------------------------------------------------------
Every CRUD-QX API can receive an optional "options" argument when making a call. These options are very simple key/value options which allow to fine-tune the call. This alone is very powerful and provides a ton of functionalities, such as (to only list a few):

Proxy model - When a model has one or more properties which have a one-to-many reference to another model, you can indicate to the server that the model can be sent back to the client without resolving the foreign ID's. This may be useful in cases where the actual reference ID's are not pertinent for the current purpose of the request in which case they will not be sent back to the client. Or in a case where they are required but can be lazy-loaded, the server will send the ID's to the client as a subsequent delayed response. This can dramatically speed up the application. When a model has not been fully resolved on the client-side, it is considered to be a "proxy model", meaning some parts of the model are missing.

Model Dependencies - When a model has one or more properties which references another model, by default only the ID (or ID's) are returned with the model. At times, one may wish to indicate to the server which property references are to be resolved as distinct and seperate models and sent back to the client. This can be configured on a per-property basis

resolve: property1:max=5:tx=atomic, property2:tx=delayed:max=5

* Request one or more model-dependencies, in an eager or lazy fasion
* Execute queries in an eager or lazy transaction
* Request a proxy model and lazy-load the remaining portions
* How creates and updates are processed (order)
* Etc.
