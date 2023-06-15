### Query planner plugins ([Issue #3150](https://github.com/apollographql/router/issues/3150))

We may need to modify a query between query plan caching and the query planner. This leads to the requirement to provide a query planner plugin capability. This capability is private to the router for now.
The plugins need an ApolloCompiler instance to perform useful work on the query, so the caching layer, in case of cache miss, will generate a compiler instance and transmit it as part of the request going through query planner plugins. At the end of the chain, the query planner extracts the modified query from the compiler, uses it to generate a query plan, and generates the selections of both the original and filtered query for response formatting. This is done to ensure that the response does not leak data removed in the filtered query, but still keeps a shape expected by the original query, using the null propagation.

By [@Geal](https://github.com/Geal) in https://github.com/apollographql/router/pull/3177