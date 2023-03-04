+++
title = "Securely storing API keys on a laptop"
date = 2023-03-04
description = "Simple solution to avoid unsecure/dirty hacks"
tags = ["dev"]
+++

If you are a hobbyist and playing with 3rd-party APIs, you probably have API tokens/keys stored somewhere (hopefully) secure. In my case since OpenAI announced their new policy and prices for the [ChatGPT API](https://openai.com/blog/introducing-chatgpt-and-whisper-apis) I wanted to give it a try so I created an account and fetched some credentials. 

The next step was to see how these credentials could be safely stored on my laptop. I do have a password manager but unfortunately it's not great for programmatic retrieval, so I had to find something else. After a quick search on the Interwebs, I stumbled upon [pass](https://www.passwordstore.org/). The tool seemed simple enough, is open-source, and was written by [Jason Donenfeld](https://www.zx2c4.com/), the creator of Wireguard: very promising!

In short, `pass` stores credentials in GPG-encrypted files on your disk and the CLI allows you to create, retrieve and edit them. To set it up, I started by installing it and creating a new GPG key:

```bash

# Fedora ftw
sudo dnf install pass

# Create GPG key (I kept all defaults)
gpg --full-generate-key 3
```

To retrieve the *GPG key ID*, grab the last 8 hexadecimal digits in the "pub" section of this command's output:

```bash
gpg --fingerprint your.email@example.com
```

You can now initialize your password store:

```bash
# Replace hex digits with your GPG key ID
pass init 0x1234ABCD
```

From there, you can add your API keys:

```bash
pass insert api.openai.com
```

When comes the time to retrieve it, just run the following command and type your passphrase:

```bash
pass -c api.openai.com
```

And that's it! To be honest, playing a bit with GPG tools made me curious about it so I may dig deeper on that subject later on. For now, time to see what funny things can be done with the ChatGPT API :)

