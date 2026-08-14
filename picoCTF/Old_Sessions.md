Info

Old Sessions
Web Exploitation
Easy
by David Gaviria
·
picoCTF 2026
Proper session timeout controls are critical for securing user accounts. If a user logs in on a public or shared computer but doesn’t explicitly log out (instead simply closing the browser tab), and session expiration dates are misconfigured, the session may remain active indefinitely.

This then allows an attacker using the same browser later to access the user’s account without needing credentials, exploiting the fact that sessions never expire and remain authenticated.

Your friend tells you to check out a new social media platform he built a few years ago. Although its still under development, he said the site is almost complete. He also mentioned that he hates constantly logging into sites, and so has made his page that 'once you login, you never have to log-out again'!

starting gives you a url to test <http://dolphin-cove.picoctf.net/>

# Thought Process

- right off the bat i knew this had something to do with cookies. most authentication system nowadays have stuff to do with cookies.
- i made an account and logged in and check my cookies.
- right away when logged in theres a comment on the site that menitons the suspicious /session page

/sessions page result is just raw text shown below example

```

1) session:fOvy_ZcKddxbLUSLKwt8rFhJvBJcBEqN5ihY4EvSxCs, {'_permanent': True, 'key': 'admin'}

2) session:S1D2qCSOnpLVrIGMUTM9YaIzIalehUYw-HBFkJlT8LA, {'_permanent': True, 'key': 'bob'}
```

- went in saw the session cookies. theres one cookie that related to the admin
- went back to the logged in page. went into developer tools and replaced the session cookie value with the admin
