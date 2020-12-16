# Tags Test Cases

## Submission/validation

1. Tap any note in list.
2. Enter text in **_Add tag..._** field.
3. Tap **_Space_** key.
4. Notice tag is saved with text from Step 2.
5. Enter text in **_Add tag..._** field.
6. Tap **_Enter_**/**_Return_** key.
7. Notice tag is saved with text from Step 5.
8. Enter `🏴󠁧󠁢󠁥󠁮󠁧󠁿🏴󠁧󠁢󠁥󠁮󠁧󠁿🏴󠁧󠁢󠁥󠁮󠁧󠁿🏴󠁧󠁢󠁥󠁮󠁧󠁿` in **_Add tag..._** field.
9. Tap **_Space_** key.
10. Notice **_Error_** notification is shown.
11. Notice space is trimmed from **_Add tag..._** field.
12. Tap **_Enter_**/**_Return_** key.
13. Notice **_Error_** notification is shown.
14. Notice newline is trimmed from **_Add tag..._** field.


## Canonical representation

1. Tap any note in list.
2. Enter any value in **_Add tag..._** field not matching an existing tag.
3. Tap **_Space_** key to submit tag.
4. Notice tag is added to note as entered.
5. Enter any variation of value from Step 4 with different lowercase or uppercase characters.
6. Tap **_Space_** key to submit tag.
7. Notice tag is not added to note as entered.
8. Tap tag chip from Step 4.
9. Tap _**x**_ in tag chip.
10. Notice tag is removed from note.
11. Enter any variation of value from Step 2 with different lowercase or uppercase characters.
12. Tap **_Space_** key to submit tag.
13. Notice tag from Step 4 is added to note


## Renaming

#### Alphanumeric

1. Tap any note in list.
2. Enter `tag1` in **_Add tag..._** field.
3. Tap **_Space_** or **_Enter_**/**_Return_** key.
4. Notice `tag1` is added to note.
5. Enter `tag2` in **_Add tag..._** field.
6. Tap **_Space_** or **_Enter_**/**_Return_** key.
7. Notice `tag2` is added to note.
8. Open Tag List screen.
9. Enter Editing mode.
10. Notice `tag1` and `tag2` are in tag list.
11. Tap `tag2` in tag list.
12. Notice `tag2` is now editable.
13. Replace `tag2` with `tag1`.
14. Tap **_Done_**/**_Save_** button to confirm changes.
15. Notice `tag1` is in tag list.
16. Notice `tag2` is not in tag list.
17. Open note from Step 1.
18. Notice `tag1` is added to note.
19. Notice `tag2` is not added to note.


#### Special

1. Tap any note in list.
2. Enter `tag!` in **_Add tag..._** field.
3. Tap **_Space_** or **_Enter_**/**_Return_** key.
4. Notice `tag!` is added to note.
5. Enter `tag?` in **_Add tag..._** field.
6. Tap **_Space_** or **_Enter_**/**_Return_** key.
7. Notice `tag?` is added to note.
8. Open Tag List screen.
9. Enter Editing mode.
10. Notice `tag!` and `tag?` are in tag list.
11. Tap `tag?` in tag list.
12. Notice `tag?` is now editable.
13. Replace `tag?` with `tag!`.
14. Tap **_Done_**/**_Save_** button to confirm changes.
15. Notice `tag!` is in tag list.
16. Notice `tag?` is not in tag list.
17. Open note from Step 1.
18. Notice `tag!` is added to note.
19. Notice `tag?` is not added to note.
