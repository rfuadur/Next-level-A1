# Combining Union and Intersection Types in TypeScript

A union type uses `|` and means a value can be one of a few shapes. An intersection type uses `&` and means a value has all the shapes together. We are allowed to mix both in the same type, and that is very useful for real API data.

Say we have a feed coming from a server. Every item has the same two fields, and then each kind of item has its own extra fields.

```ts
type BaseEntity = {
  id: string;
  createdAt: string;
};

type Article = { kind: "article"; title: string; body: string };
type Video = { kind: "video"; title: string; durationInSeconds: number };

type FeedItem = BaseEntity & (Article | Video);
```

A `FeedItem` now always has `id` and `createdAt`. On top of that it is either an article or a video, never a mix of both.

The brackets around `Article | Video` matter. Without them, `&` is applied first and we get a type we did not ask for.

Both shapes have a `kind` field with a fixed text value. That lets TypeScript work out which one we are holding:

```ts
function describeItem(item: FeedItem): string {
  if (item.kind === "video") {
    return `${item.title} (${item.durationInSeconds}s)`;
  }

  return `${item.title}: ${item.body.slice(0, 40)}`;
}
```

Inside the `if` block TypeScript knows the item is a video, so `durationInSeconds` is safe to use. If we try to read `item.body` there, we get an error while writing the code instead of a bug later.

So the rule is simple. We use `&` for the fields that every item shares. We use `|` for the parts that are different. We add one field like `kind` so we can tell them apart. Our types stay short and mistakes show up early.
