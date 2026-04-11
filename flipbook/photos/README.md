# Photos folder

Drop family photos in this folder. Any common image format works (jpg, png, webp).

After adding a photo, open `index.html` and register it in the `PHOTOS` map
near the top of the `<script>` block, keyed by the page index (0 = George Barnes,
1 = Ellen Barnes Gerald, etc.).

Example:

```js
const PHOTOS = {
  0: 'photos/george-barnes.jpg',
  4: 'photos/lonnie-barnes.jpg',
};
```
