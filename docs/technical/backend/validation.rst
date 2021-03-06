Validation & Persistence
========================

Validation runs on a full subtree *before any data is saved to the
graph* and has three stages:

-  check data against type-based constraints
-  generate identifiers, using type-based rules
-  check integrity and uniqueness

Data validation
---------------

Currently, data validation is mainly concerned with ensuring that
certain mandatory properties are not absent or blank. For example,
Documentary Unit items require an ``identifier`` property. Their
descriptions likewise require both a ``languageCode`` and a ``name``
property.

Data validation must be done prior to generating IDs because IDs can be
derived from the data itself in some circumstances. If the data is not
valid, ID generation would then fail.

ID generation
-------------

(See the `identifiers <identifiers.html>`_ page for more on the rationale.)

Graph IDs are generated in differently depending on the type of item.
There are currently two types of ID generators:

-  data and scope-based
-  random UUID

For most 'first class' node types, graph IDs are generated by combining
the ID of the item's parent scope(s) with a particular data value
belonging to the item itself. For example, a documentary unit will
typically belong to two scopes: a repository, and the country in which
that repository resides. Its eventual graph ID therefore forms a path:

::

    [country-id]-[repository-id]-[doc-id]

There are two main reasons for generating graph IDs in this manner: they
are to some extent human-readable and reproducible, and it makes it
possible to ensure the uniqueness of identifiers within scopes without
necessitating complex graph scans.

Item types which are not 'first class', i.e. dependent on some other
item, are typically assigned random UUIDs.

Integrity checking
------------------

The integrity checking runs after ID generation and does the following:

-  ensures there are no ID collisions, which would indicate an
   identifier is not unique within a particular scope.
-  checks uniqueness constraints for properties which must be globally
   unique. The facility is not currently used.
