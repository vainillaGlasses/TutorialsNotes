# Create SSH

### Generating a new SSH key

1. Open Terminal.

2. Paste the text below, substituting in your email address.

   ```
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

   This creates a new ssh key, using the provided email as a label.

   ```
   Generating public/private rsa key pair.
   ```

3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

   ```
   Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
   ```

4. At the prompt, type a secure passphrase. For more information, see

    

   "Working with SSH key passphrases"

   ```
   Enter passphrase (empty for no passphrase): [Type a passphrase]
   Enter same passphrase again: [Type passphrase again]
   ```

Get from: https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/