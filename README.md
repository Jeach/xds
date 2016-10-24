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
support, etc). When creating models, you can also define default values for each data type.

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

RBAC
-----------------------------------------------------------------------------------------------------------
Full "Role Based Access & Control" support is built into the datbase framework, providing for fine-grained
permission control over who can access and do what to each domain, model family, model instance or even
a model's properties.

Flexible, Multi-Level Cacheing
-----------------------------------------------------------------------------------------------------------
Built-in per-model caching can be provided for each client configuration and easily configurable on the
server as well. This allows client code to fully optimize at the client-level and also allows for servers
to provide cache optimizations as required without having to provide any code.

API Options
-----------------------------------------------------------------------------------------------------------
Every CRUD-QX API can receive an optional "options" argument when making a call. These options are very
simple key/value options which allow to fine-tune the call. This alone is very powerful and provides a ton
of functionalities, such as (to only list a few):

* Request one or more model-dependencies, in an eager or lazy fasion
* Execute queries in an eager or lazy transaction
* Request a proxy model and lazy-load the remaining portions
* How creates and updates are processed (order)
* Etc.
