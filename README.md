# Bengfort CS Web Page

**My personal academic website at the University of Maryland: [http://cs.umd.edu/~bengfort/](http://cs.umd.edu/~bengfort/)**

![Header Image](images/babbage-1.jpg)

## Writing

I don't update this site very often, so it's useful for me to have some notes to both write and publish the site to the UMD servers. The site is a [jekyll](https://jekyllrb.com/) statically generated site. Make sure you have the `jekyll` command installed (OS upgrades tend to get rid of it):

    $ gem install jekyll

To start writing, edit the the `_config.yml` file and swap the comments for the url as follows:

    # url:              http://cs.umd.edu/~bengfort
    url:              http://localhost:4000

This will allow you to serve the local website and be able to click around correctly. Speaking of, serve the website as follows:

    $ jekyll serve

To create a new post, all you need to do is create a new file in the `_posts` directory. How you name files in this folder is important. Jekyll requires blog post files to be named according to the following format:

    YEAR-MONTH-DAY-title.md

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file (by default I simply use Markdown).

Jekyll also has built-in support for syntax highlighting of code snippets using either Pygments, and including a code snippet in any post is easy. Just use the dedicated Liquid tag as follows:

    {% highlight python %}
    def fib(n):
        if n < 2:
            return n
        return fib(n-2) + fib(n-1)

    print map(fib, range(0, 10))
    #=> prints '[0, 1, 1, 2, 3, 5, 8, 13, 21, 34]' to STDOUT.
    {% endhighlight %}

### Papers

On this site I also host academic papers that I've written in PDF format. Add papers to the `papers` directory and then include them in posts at the end by adding the `paper` tag in the YAML front matter of the post. A link to the PDF paper will automatically be included in the post.

## Publishing

Edit the `_config.yml` to put back the UMD url as the domain:

    url:              http://cs.umd.edu/~bengfort
    # url:              http://localhost:4000

Then _before you commit to Github_ create a build of the site:

    $ jekyll build

This is important since you can't install Jekyll on Junkfood (the Unix cluster the site is hosted on). So the static files must be in the repository and cloned directly to Junkfood.

Next, SSH into junkfood - which is possible if you're not actually on campus as long as you're connected to the CS VPN. Then change to the www repo in your home directory. Pull the changes from Github (and hope that everything is configured correctly).

    $ ssh junkfood
    $ cd repos/www
    $ git pull

Now, this directory is not where the site is hosted from. I use `rsync` to sync the `_site` directory in particular to the correct location. There is a script in the `~/bin` directory to make this happen. Luckily, this is also on the `$PATH` so simply:

    $ websync

That's it, you should be able to reload the page and have your new static site up and running!
