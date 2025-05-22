
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

```shell
Enter a file in which to save the key (/home/YOU/.ssh/id_ALGORITHM):[Press enter]
```

leave the passphrase blank 
```shell
Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

```shell
 eval "$(ssh-agent -s)"
```

```shell
ssh-add ~/.ssh/id_ed25519
```

Add to github account

Copy the SSH public key to your clipboard.

```shell
 cat ~/.ssh/id_ed25519.pub
```

Then select and copy the contents of the id_ed25519.pub file


1. In the upper-right corner of any page on GitHub, click your profile photo, then click  **Settings**.
    
2. In the "Access" section of the sidebar, click  **SSH and GPG keys**.
    
3. Click **New SSH key** or **Add SSH key**.
    
4. In the "Title" field, add a descriptive label for the new key. For example, if you're using a personal laptop, you might call this key "Personal laptop".
    
5. Select the type of key, either authentication or signing. For more information about commit signing, see [About commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification).
    
6. In the "Key" field, paste your public key.
    
7. Click **Add SSH key**.