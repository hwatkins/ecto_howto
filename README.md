# ecto_howto
Cheat sheet for ecto queries

```elixir
def increment_page_views(post) do
  query =
    from "posts",
      where: [id: ^post.id],
      update: [inc: [page_views: 1]]
  MyApp.Repo.update_all(query)
end
```
