## Key's Aren't Enough
While it's great that an attacker would need to steal your AWS credentials to be able to maliciously change the contents of your bucket, relying _only_ on the secrecy of keys is often not enough.

_Keys and passwords are compromised all the time_.

One way to add an additional layer of security is to ensure that your keys can only be used from certain (virtual) locations. Then an attacker would need your keys _and_ to be on your network to gain access.

## Scoping Permission

A _critical_ rule of thumb in cyber security is the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege): You should allow the fewest permissions possible that can still get the job done.

For example, _your_ user is in the "manager" group which we gave "full admin access" to. Especially at smaller companies, it's common for folks to have more permissions than they truly need, usually for the sake of speed and convenience.

But that's _not_ the most secure way to do things.

# Encryption

Although our S3 bucket is private (which means outsiders can't gain access to the files directly without credentials), it's still good for stuff to be encrypted. After all, what if a hacker physically walked into the data center to read our customers' secrets directly?

## At Rest

Files in S3 are encrypted at rest ("at rest" just means "while they're sitting in storage on disk") by default. This was not always the case, but it is now! You don't need to do anything, the S3 service takes care of all of that for you. When you access S3 with your credentials, the service decrypts the files for you before handing them over.

## In Transit

When you're uploading or downloading files from S3, how do you know that someone can't intercept the data as it travels through the internet? Well, when you access S3 via the web, you're using [httpS](https://en.wikipedia.org/wiki/HTTPS). The `S` means that the data is encrypted as it travels between your computer and the S3 service.

When you access S3 via the SDK (in your Go code), it also uses HTTPS by default. So as long as you don't go out of your way to disable encryption, you're good to go.

Boots

Spellbook

![Boots](https://www.boot.dev/_nuxt/new_boots_profile.DriFHGho.webp)

**Struggling?** I, Boots the Gormless Glutton, am happy to help! Ask me anything about the lesson. If you find my explanations suspect, ask a human for help in [the Discord](https://boot.dev/community)!

What does 'encryption at rest' in S3 mean?

Files are encrypted while stored on disk in S3

Files are encrypted only when accessed via the AWS SDK

Files are encrypted manually by the user before upload

Files are encrypted as they travel across the internet

🪄 Explain