---
title: Building a Website with Jekyll and Chirpy Theme
date: 2023-09-21 12:00:00 -500
categories: [self-hosted, website]
tags: [jekyll,website,github,gitlab]
---

If you're looking to create a beautiful and customizable website quickly, Jekyll and the Chirpy theme are an excellent choice. In this tutorial, I'll guide you through the process of setting up your website (hosted for free on GitHub), from installing dependencies to creating your first post.

## Prerequisites: Install Dependencies

Before we get started, make sure you have the following prerequisites installed on your system:  

```bash
sudo apt-get install ruby-full build-essential zlib1g-dev
```

Avoid installing RubyGems packages (called gems) as the root user. Instead, set up a gem installation directory for your user account. The following commands will add environment variables to your `~/.bashrc` file to configure the gem installation path:

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Install Jekyll and Bundler:

```bash
gem install jekyll bundler
```

Ensure `git` is installed:

```bash
sudo apt install git
```

## Step 1: Set Up Your GitHub Repository  

Let's start by heading to: [Chirpy Starter](https://github.com/cotes2020/chirpy-starter/generate)

Generate a new repository, and name it `USERNAME.github.io`, where `USERNAME` represents your GitHub username.

This sets up your Chirpy themed site without having to manually configure vanilla Jekyll.

## Step 2: Clone Your Repository and install dependencies

To clone your repository (copy the `ssh` link from your repository you just created):

```bash
git clone git@<YOUR-USER-NAME>/<YOUR-REPO-NAME>.git
```

To install dependencies:

```bash
cd REPO-NAME
bundle
```

## Step 3: Create Your First Post

To create a new post, navigate to the root of your project and run:

```bash
code .
```

This will open up Visual Studio Code.

Create a new file and name it using this format:

`YYYY-MM-DD-title.md`

At the top of the new file copy and format this `front matter` to your needs:

```yaml
---
title: TITLE
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [TOP_CATEGORY, SUB_CATEGORY]
tags: [TAG,TAG,TAG]     # TAG names should always be lowercase
---
```

## Step 4: Add Content Using Markdown

**Headers:** Create headers by using hash symbols (`#`). The number of hash symbols indicates the header level, with one `#` for the largest heading and six `######` for the smallest.

```markdown
# This is a Header 1 
## This is a Header 2 
### This is a Header 3
```

**Emphasis:** You can make text _italic_ by enclosing it in asterisks or underscores, and you can make it **bold** by enclosing it in double asterisks or underscores.

```markdown
*This is italic text*
_This is also italic text_

**This is bold text**
__This is also bold text__

```

**Lists:** Create ordered and unordered lists using numbers or asterisks/plus/minus signs.

  Unordered list:

   ```markdown
   - Item 1 
   - Item 2 
   - Item 3
```

Ordered list:

```markdown
1. First item 
2. Second item 
3. Third item
```

**Code:** Inline code can be enclosed in backticks, and code blocks can be created by using triple backticks with a specified language.

Code block:

```markdown

    ```python
    def greet(name):
        print(f"Hello, {name}!")
    ```
```

There's more you can do using markdown, but this isn't the place for that. But [this](https://markdownguide.org/cheat-cheat) is.

## Step 5: Edit `_config.yml`

The `_config.yml` file does a good job with the `#commented` out lines to help you configure your site.

## Step 6: Locally Host Your Site

To preview your website locally, run (from your site's directory):

```bash
bundle exec jekyll s
```

You can access your site at `http://localhost:4000`. Any changes you make to your posts or site configuration will be automatically reflected on the local server. If your server isn't updating the changes you've made stop and start the server. Also ensure that you haven't made any formatting errors with your `_posts` file or `_config.yml`.

Also, Jekyll will not generate any posts that occur in the future by default. To do so, you'll need to:

```bash
bundle exec jekyll s --future
```

## Step 7: Push Updated Site to GitHub

Using the terminal:

Check the status of your repository to see changes (in red)

```bash
git status
```

After confirming your changes, commit and push them up to `git`

```bash
git add .
git commit -m "made some changes"
git push
```

Alternatively, you can use the `source control` tab in Visual Studio Code to commit, make a comment, and push out the changes.

## Conclusion

🎉🎉🎉 Congratulations! 🎉🎉🎉

You've successfully set up a Jekyll website using the Chirpy theme. Now you can start creating content and customizing your site to make it truly yours.

Explore [Jekyll](https://jekyllrb.com/) and [Chirpy's](https://chirpy.cotes.page/) documentation for more advanced features and customization options.

Happy blogging!
