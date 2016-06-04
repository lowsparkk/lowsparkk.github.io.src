+++
date = "2016-05-29T17:20:26-07:00"
title = "Setting Up a hugo static site on GitHub pages"
tags = ["hugo"]
topics = ["blogging"]
+++
Instructions for setting up a static site on github pages using hugo.

1. Set up a hugo site in a new directory

    ```
     md ~/blog
     cd ~/blog
     hugo new site .
    ```
1. Clone the theme directory inside the `~/blog` directory

    ```
    git clone https://github.com/enten/hyde-y.git themes/hyde-y themes/hyde-y
    ```

1. Create the `~/blog/data` folder and create a file called `Menu.toml` with the following

    ``` toml
    [posts]
      Name = "posts"
      Title = "Show list of posts"
      URL = "/post"

    [topics]
      Name = "topics"
      Title = "Show list of topics"
      URL = "/topics"

    [tags]
      Name = "tags"
      Title = "Show list of tags"
      URL = "/tags"
    ```


1. Create a new post, still in the `~/blog` directory

    ```
    hugo new post/setting-up-hugo-github-pages.md
    ```

1. Edit the `config.toml` file in the `~/blog` directory

    ``` toml
    baseurl = "/"
    languageCode = "en-us"
    title = "Lowsparkk Labs"
    theme = "hyde-y"

    [permalinks]
      post = "/:year/:month/:day/:slug/"
      code = "/:slug/"

    [taxonomies]
      tag = "tags"
      topic = "topics"

    [author]
      name = "Low Sparkk"
      email = "low.sparkk@yahoo.com"

    [params]
      # You can use markdown here.
      brand = "Your Brand Name"
      topline = "your tag line"

      # Sidebar position
      # false, true, "left", "right"
      sidebar = "left"

      home = "home"

      # Select a syntax highight.
      # Check the static/css/highlight directory for options.
      highlight = "solarized_light"

      # Google Analytics.
      googleAnalytics = "" #Your Google Analytics tracking code

      # Sidebar social links.
      github = "" # Your Github profile ID
      bitbucket = "" # Your Bitbucket profile ID
      linkedin = "" # Your LinkedIn profile ID (from public URL)
      googleplus = "" # Your Google+ profile ID
      facebook = "" # Your Facebook profile ID
      twitter = "" # Your Twitter profile ID
      youtube = ""  # Your Youtube channel ID
      flattr = ""  # populate with your flattr uid

    [blackfriday]
      angledQuotes = true
      fractions = true
      hrefTargetBlank = false
      latexDashes = true
      plainIdAnchors = true
      extensions = []
      extensionmask = []
    ```

1. Create **two** empty repositories on github
    1. `blog`
    1. `<username>.github.io`

Create the `<username>.github.io` with a `README.md` file

1. Kill your hugo server

1. Initialize a git repository and add the remote
    ```
    cd ~/blog
    git init
    git remote add origin git@github.com:<username>/blog.git
    ```

1. Clone the `<username>.github.io` repo into a submodule in `public`

    ```
    git submodule add git@github.com:<username>/<username>.github.io.git public
    ```

1. Add everything in the `blog` repository and push it to GitHub

    ```
    git add .
    git commit -m "your commit message here"
    git push -u origin master
    ```

1. Regenerate the static site HTML and push the `public` submodule to GitHub

    ```
    hugo
    cd public
    git add .
    git commit -m "your commit message here"
    git push origin master
    ```

1. In a short while, your static site will be available at `<username>.github.io`
