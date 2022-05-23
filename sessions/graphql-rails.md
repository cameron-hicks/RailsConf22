# GraphQL and Rails: Beyond HTTP

interesting points I didn't know about GraphQL:

- bulk operations: for efficiently querying large numbers of fields and nested objects over a large collection that will need to be paginated; an alternative to opening a million connections, as would be necessary to resolve the nested queries
- some advanced webhook subscription features
