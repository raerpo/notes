# React Query

- React query default are stale by default so every time you change the tab the request will be made again.
  We can change this value with config property `staleTime: 5000` to stale the results after 5 seconds.
- Stale queries will be refetch on the background by default. You can change this with the property `refetchOnWindowFocus`
- Failed queries will be automatically retried 5 times before setting error to `true`. You can change this with `retry` and `retryDelay`.
- By default the query results will be deep compared to invalidate or not the cache.

## Queries

- a query is an async dependency on some source of remote data.
- `useQuery` requires a value to identify and retreive the cache and a then-able function.
- the result of useQuery is an object that you can destructure and use according to your needs.
  `{ data, isLoading, isFetching, error, status }`
- query keys can be arrays especially useful for requests with query params or ids. for example:
  `"todos"`, `["todos", 5]`, `["todos", { resolved: true }]`
- We can also make parallel and serial requests. For parallel requests we don't need to do anything. For serial request we can use
  the `enabled` config property.

  ```js
   // Get the user
    const { data: user } = useQuery(['user', email], getUserByEmail)

    // Then get the user's projects
    const { isIdle, data: projects } = useQuery(['projects', user.id], getProjectsByUser,
        {
            // `user` would be `null` at first (falsy),
            // so the query will not execute until the user exists
            enabled: user,
        }
    )
  ```
- Difference between `isLoading` and is `isFetching`.
  isLoading happens on the initial data loading and isFetching happens when the data is being retreive on the background
- There's a `useFetching` hook (`const isFetching = useFetching()`) to show a global indication when a fetching is being made on any  level
