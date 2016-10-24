# xds
XDS is an acronymn for X as in "unamed" and DS as in Database System. So in essence, XDS stands for the Unamed Database System.

It is a high-level, multi-faceted database architecture which should provide the following functionalities:

CRUD-QX Support
-----------------------------------------------------------------------------------------------------------
Provide support for CRUD functionality (Create, Read, Update and Delete functions), extended with a
Query and eXecute API.

Property Based Models
-----------------------------------------------------------------------------------------------------------
All models are a "named" set of properties where each property consists of a type and value. In addition,
meta-data provides additional information for the model and properties (ie: indexing and enhabced string
support, etc).

Each model is governed by defined "scope:domain" naming scheme allowing for a unique model namespace.

Revisioned Data
-----------------------------------------------------------------------------------------------------------
Provide optional data revisioning for each model. When enabled, will ensure every update is a new entry
and previous revisions are kept (depending on configured auto-purging policies).

Revision 0 always infers the "latest" (most recent) instance of the model. Revision offset can also be used
by using a negative number such as -1 (last revision), -2 (second last revision), and so on. Abssolute 
revision numbers can also be used, such as 1 (first every copy), 2 (second update), 33, 55 and so on.

ASYNC Even-Driven Framework
-----------------------------------------------------------------------------------------------------------
Every call to the CRUD-QX API is ASYNC and fully event-driven, meaning that the call will return immediately
with a transaction ID. As the request is being fullfilled in-part or in-whole, previously registered events
will be invoked.

Events can be registered on entire model domains, model families or specific model instances.

RBAC
-----------------------------------------------------------------------------------------------------------


