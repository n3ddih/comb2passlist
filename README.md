# Comb Wordlist Generator

Generate a password wordlist from a set of usernames using ProxyNova's Comb API.

# Idea

Given a list of usernames, the tool will create a list of passwords that has been leaked by those passwords.

> API: `curl https://api.proxynova.com/comb?query=`
> 
- 2 output option:
    - A list of all passwords.
    - A list of dictionaries with the username as key and list of passwords as value.
- Usernames need to have the syntax `abcd_efgh-123` without using any other special chars.
    
    > Should username input that has [space] converted into `_` or `-`?

## Limit

- The comb API only limit 20 records per query ⇒ Limited password.
- The query is limited to special character, some might cause comb to perform AND search.

# Todo

- Make a function for username:
    - Fill it with `@` when length < 4.
    - Wonder what to do with usernames with space character.

# Usage

```
$ python comb2passlist.py -l usernames.lst
```
