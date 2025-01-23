# Configure username and email

When managing multiple Git accounts, you need to configure the username and email for each one.
To set these only for your current repository:
## Local configuration
```bash
git config user.name "Your name"
git config user.email "Your email"
```
To be more specific, you can use the `--local` flag:
```bash
git config --local user.name "Your name"
git config --local user.email "Your email"
```

## Global configuration
To configure globally:
```bash
git config --global user.name "Your name"
git config --global user.email "Your email"
```

## List configuration
To list the configuration:
```bash
git config --list
```
to get the source of the configuration:
```bash
git config --list --show-origin
```
