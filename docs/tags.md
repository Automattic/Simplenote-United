# Tags


## Tag hash
Tag hash is derived from the tag's name and is used to uniquely identify a tag.
Two tags are considered to be the same if their hashes are the same.

Steps to create a Tag hash:  

* Normalise tag name into its canonical composition form
* Lowercase it using `en-US` locale
* Percent-encode everything but the alphanumeric characters

Examples:  
```
hash("España") = "espa%C3%B1a"
hash("zero/one.two-3_four") = "zero%2Fone%2Etwo%2D3%5Ffour"
```


## Tag name
Constraints on a tag name:

* At least one character
* No spaces
* No commas
* Resulting hash can’t be longer than 256 characters


## Showing tags
We always show the canonical value of a tag as it is stored in the tag's `name` property, which is the lexical variation of a tag when it is first created. All other lexical variations are auto-corrected to the canonical value when shown. This ensures that we always show the same lexical variation of the name to the user.


## Adding a tag to a note
Notes store linked tags as an array of strings in the `tags` property. The values here may not match the representation of the tag name in the `name` property of the corresponding tag in the tag bucket since the lexical variation in the note's `tags` array may not be the canonical value in the tag's `name` property.

As you type we show tag suggestions that start with the entered text, excluding tags already added to the note.

Steps to add a tag to a note:

* Validate a tag name
* Check if note already has the same tag
* Check if the tag already exists in the tag bucket. If the tag doesn’t exist, create a new tag.
* Add the tag to the note

### Tag separators
When adding tags from a client the following should be used as separators:

* White space
* Comma


## Removing a tag from a note
Remove a tag from the `tags` property of the note.


## Deleting a tag
* Remove the tag from the notes containing this tag
* Remove the tag from the `tag` bucket

Unfortunately, it’s possible to delete a tag while some notes are unknown, having been created on another device. In this case, the tag will be removed from the tag bucket but remain inside another note’s tags list.


## Renaming a tag
First, validate a tag name.

If the new tag hash is the same as the old tag hash, all that needs to be done is to update the name of the existing tag.

If there is another tag with the same hash, we’ve got a collision. Please consult the “Tag collision” section.

If there is no known tag with the same hash:

* Create a new tag with the same `index` property as the old one
* Iterate over the notes replacing the old tag with the new one
* Delete the old tag


## Tag collision
A collision happens when we rename a tag and there is already a tag with the same hash. As an example, imagine the tags “cat” and “dog” exist. If you try to rename “dog” to a lexical variation of “cat” like “Cat” for instance, a Tag Conflict dialog will be shown asking to merge the renaming tag (i.e. “dog”) into the canonical representation (i.e. “cat”) or cancel the rename.

Steps to merge “dog” tag into "cat" tag:
* Iterate over the notes replacing "dog" tag with "cat" tag. If the note already has "cat" tag, remove "dog" tag from the note.
* Delete "dog" tag.

---

## Tag issues

### Tag entity stored with wrong entity id

Tag entities are referenced by their tag hash. The canonical tag name is one of many lexical variations that collapse to the same tag hash. In many accounts we will find inconsistencies in the database because the apps did not always properly store tags. These inconsistencies can lead to issues in the UI, such as showing the wrong subset of notes for a given tag.

Consider the tag `Bücher` whose entity id should be `b%C3%BCcher`. If stored as `Bücher` we might end up with two tag entities whose tag hashses collapse to the same value. This would appear as a tag collision.

In these cases we ought to resolve the inconsistency by removing the tag with the wrong entity id. We need to be careful when doing this to make sure that the stored tag name in the inconsistent tag entity is an actual lexical variation of the tag's canonical name. It's possible that through renaming a tag we could end up with a worse issue, which is that the name of the tag collapses to a tag hash for a different tag name.

#### Examples

|Entity Id|Tag Name|Tag Hash|Matches?|Action|
|---|---|---|---|---|
|`B%C3%BCcher`|`Bücher`|`b%C3%BCcher`|✅|Do nothing|
|`Bücher` | `Bücher` | `b%C3%BCcher` |❌|Ensure tag entity `B%C3%BCcher` exists and then remove tag entity `Bücher`|
|`Bücher`| `Books` | `books`|❌|Ensure tag entity `books` exists and then remove tag entity `Bücher`. Ensure tag entity `B%C3%BCcher` exists.|

These are lossy processes. It may be the case that note entities reference `Bücher` as the tag but because of the inconsistencies those notes have been appearing in the note list when the `Books` tag is selected. After the fix this will no longer be the case. This is because tags are stored referencing their names through the tag hash instead of being independent entities referenced by their unchanging id.
