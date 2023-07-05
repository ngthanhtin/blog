# This project uses hugo v0.95.0, you can download it at: https://raw.githubusercontent.com/Homebrew/homebrew-core/c27b162abd2f40785678da93924b47660bf98f5a/Formula/hugo.rb

```
brew uninstall hugo
brew install hugo.rb
```

# Add new content
```
hugo new site my-site
cd my-site
hugo new privacy.md
```
# Testing the page
```
hugo server -w -D
```
# Publish a new site
```
hugo
```