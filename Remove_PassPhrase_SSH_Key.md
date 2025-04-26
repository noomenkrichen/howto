# Remove passphrase from your existing SSH key
Run this command:

```bash
ssh-keygen -p -f ~/.ssh/id_rsa
```
(replace id_rsa with your actual key filename if it's different, like id_ed25519.)

It will ask you:

```sql
Enter old passphrase:
```
👉 Type your current passphrase.

Then it will ask:

```php
Enter new passphrase (empty for no passphrase):
```
👉 Just press Enter (don't type anything).

Confirm by pressing Enter again when asked.

That’s it!
Now your SSH private key has no passphrase and Git will use it without asking anything.

⚡ Important note: Without a passphrase, your SSH key is less secure.
Anyone who gets access to your private key file can connect as you — so keep it safe (permissions 600 on the file).

You can make sure permissions are correct:

```bash
chmod 600 ~/.ssh/id_rsa
```
