# ecto_howto
Cheat sheet for ecto queries

```elixir
def update_title(post, new_title) do
  query =
    from "posts",
      where: [id: ^post.id],
      update: [set: [title: ^new_title]]
    MyApp.Repo.update_all(query)
end
```

```elixir
def increment_page_views(post) do
  query =
    from "posts",
      where: [id: ^post.id],
      update: [inc: [page_views: 1]]
  MyApp.Repo.update_all(query)
end
```
```elixir
import Ecto.Query
def running_activities(start_at, end_at)
  query =
    from u in "users",
      join: a in "activities",
      on: a.user_id == u.id,
      where:
        a.start_at > type(^start_at, :naive_datetime) and
          a.end_at < type(^end_at, :naive_datetime),
      group_by: a.user_id,
      select: %{
        user_id: a.user_id,
        interval: a.end_at - a.start_at,
        count: count(u.id)
      }
  MyApp.Repo.all(query)
end
```

```elixir
MyApp.Repo.insert_all(
  Post,
  [
    [title: "hello", body: "world"],
    [title: "another", body: "post"]
  ]
)
```

```elixir
# Use the ID to trigger updates
post = from p in "posts", where: [id: ^id]
# Update the title for all matchihg posts
{1, _} = MyApp.Repo.update_all post, set: [title: "new title"]
# Delete all matching posts
{1, _} = MyApp.Repo.delete_all post
```

