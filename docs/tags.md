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
