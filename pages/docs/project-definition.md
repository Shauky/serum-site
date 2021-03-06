---
title: Project Definition
group: docs
order: 3
template: doc
---

Every Serum project has a file named `serum.exs` right under the project's
root directory. This file indicates that the containing directory is a Serum
project directory, and it contains various metadata and options which are used
while Serum builds the project. This document describes how to configure your
own `serum.exs` file properly.

## Properties

Below is an example of `serum.exs`. Note that the code in this file must be
evaluated to a map.

```lang-elixir
%{
  site_name: "Serum",
  site_description: "A simple static website generator",
  author: "John Doe",
  author_email: "john.doe@example.com",
  base_url: "/~john/sample/",
  server_root: "https://example.com",

  posts_source: "posts",
  posts_path: "blog",
  tags_path: "blog/tags",
  date_format: "{D} {Mshort} {YYYY}",
  list_title_all: "All Posts",
  list_title_tag: "Posts Tagged \"~s\"",
  pagination: true,
  posts_per_page: 10,
  preview_length: {:chars, 200},

  plugins: [
    Serum.Plugins.TableOfContents,
    {Serum.Plugins.LiveReloader, only: :dev}
  ],

  theme: Serum.Themes.Essence
}
```

### Website Identification

* `site_name` (string, **required**)

    The name (title) of your website.

* `site_description` (string, **required**)

    A short description of your website.

* `author` (string, **required**)

    The name of the website author.

* `author_email` (string, **required**)

    The email address of the website author.

* `base_url` (string, **required**)

    The root path of the website on the web server. For example, if
    you want to make the front page of the website accessible from
    `https://example.com/~user/site1/`, you must set this property to
    `/~user/site1/`.

    Make sure you append a slash (`/`) at the end of the value, or build will
    fail with a validation error.

* `server_root` (string, optional)

    This is useful when you need to build an absolute URL of a page. It must
    start with `http://` or `https://`.

### Blog Configuration

* `posts_source` (string, optional)

    Path to a directory which holds source files for your blog posts.
    Defaults to `"posts"`.

* `posts_path` (string, optional)

    Path in a output directory which your rendered blog posts will be written
    to. Defaults to the value of `:posts_source`. (i.e. the default value will
    be `"posts"` if the value of `:posts_source` is not explicitly given.)

* `tags_path` (string, optional)

    Path in an output directory which the tag pages will be written to.
    Defaults to `"tags"`.

* `date_format` (string, optional)

    Determines how the date and time should be represented in blog posts and
    blog post lists. It uses the default formatting language of
    [Timex](https://github.com/bitwalker/timex). Please read
    [this documentation](https://hexdocs.pm/timex/Timex.Format.DateTime.Formatters.Default.html)
    for detailed explanation of the formatting syntax.

    If this property is not defined or invalid, Serum will fall back to
    `"{YYYY}-{0M}-{0D}"`.

* `list_title_all` (string, optional)

    Sets the title text of "all posts" list page. If not given, `"All Posts"`
    will be used as the default title string.

* `list_title_tag` (string, optional)

    Defines title text format of tag-filtered list page. The default format is
    `"Posts Tagged ~s"`. Note that you must put exactly one `~s` in the format
    string, as this is the placeholder for tag name. If you need to display
    `~` character in pages, insert `~~` (two consecutive tildes).

* `pagination` (boolean, optional)<br>
  `posts_per_page` (integer, optional)

    `pagination` Determines if pagination will be used while building post
    lists. If set to `true`, each post list will be split into pages, and each
    page will have at most `posts_per_page` post entries.

    Default values of `pagination` and `posts_per_page` are `false` and `5`,
    respectively.

* `preview_length` (2-tuple or integer, optional)

    > Note (soft-deprecated)
    > {: .title}
    >
    > It is recommended to use the new [Preview Text Generator
    > Plugin](%page:docs/plugin/preview), which was introduced in Serum v1.5.0
    > and is more powerful than using this built-in feature.
    {: .note}

    A value which sets the maximum number of characters in the preview text of
    each blog post. Defaults to `200`.

    You may set the value in the following formats. Any invalid values will
    result in empty preview texts.

    <pre class="lang-elixir"><code>number
&#35; Or
&#123;:chars, number}</code></pre>

    Maximum number of characters in a preview text.

    <pre class="lang-elixir"><code>&#123;:words, number}</code></pre>

    Maximum number of words in a preview text.

    <pre class="lang-elixir"><code>&#123;:paragraphs, number}</code></pre>

    Maximum number of paragraphs in a preview text. Serum counts the number of
    `<p>` tags in a generated HTML content.

    > Note
    > {: .title}
    >
    > It is recommended to explicitly set this property to zero if you are not
    > going to use preview texts, as by doing so some unnecessary HTML
    > processing can be skipped.
    {: .note}

### Miscellaneous

- `plugins` (list, optional)

    Plugins extend the functionality of Serum by altering source data,
    intermediate data, or final outputs during the build process.

    The documentation for [`Serum.Plugin`
    module](https://hexdocs.pm/serum/Serum.Plugin.html) describes what Serum
    plugins are, how to use them, and how to create your own Serum plugin.

- `theme` (atom, optional)

    A theme is a set of predefined templates and assets that helps you quickly
    setup the look and feel of your website. Read the documentation for
    [`Serum.Theme` module](https://hexdocs.pm/serum/Serum.Theme.html) to learn
    more about Serum themes.
