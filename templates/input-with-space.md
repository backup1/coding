# input with space

ref [https://eecs.oregonstate.edu/ecampus-video/CS161/template/chapter\_3/strings.html](https://eecs.oregonstate.edu/ecampus-video/CS161/template/chapter\_3/strings.html)

```cpp
// Return char way
char exampleChar;
exampleChar = cin.get();

//char as function argument way
char exampleChar;
cin.get(exampleChar);

// skip a single character
cin.ignore()

// In this form, characters are skipped until the number skipped is equal to n
// or until the character c is encountered, whichever comes first.
// If the character c is encountered, it itself is not skipped.
// It just triggers the skipping to stop.
cin.ignore(n, c) // where n is an integer and c is a character.

// The getline function reads in an entire line including all leading and trailing
// whitespace up to the point where return is entered by the user.
// The result is returned as a string object.
getline(cin, stringVariable);
```
