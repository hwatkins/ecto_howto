# ecto_howto
Cheat sheet for ecto queries

Most of these were pulled from [the-little-ecto-cookbook]

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
```elixir
import Ecto.Query
from p in Post,
  where: p.author == "José" and p.category == "Elixir",
  where: p.published_at > ^minimum_date,
  order_by: [desc: p.published_at]
```

```elixir
import Ecto.Query
Post
|> where([p], p.author == "José" and p.category == "Elixir")
|> where([p], p.published_at > ^minimum_date)
|> order_by([p], desc: p.published_at)
```

```elixir
defp get_or_insert_tag(name) do
  %Tag{}
  |> Ecto.Changeset.change(name: name)
  |> Ecto.Changeset.unique_constraint(:name)
  |> Repo.insert
  |> case do
    {:ok, tag} -> tag
    {:error, _} -> Repo.get_by!(MyApp.Tag, name: name)
  end
end
```

   [the-little-ecto-cookbook]: <https://dashbit.co/ebooks/the-little-ecto-cookbook>
