# HackerNews Reader

Have you ever wanted to block certain articles or users on
[Hacker News](https://news.ycombinator.com/)? Look no further. HackerNews Reader
lets you configure "allow" and "deny" rules for: users, post titles, post URLs,
and comment content.

The reader is hosted at https://logicnow.io/hn/index.html and you can visit it
with or without a [View](#views) like this:

- **With a "No AI" filter**:
  https://logicnow.io/hn/index.html?view=https://raw.githubusercontent.com/dan-lovelace/hackernews-reader-filters/refs/heads/main/views/no-ai.json -
  AI-related stories and comments are automatically hidden.
- **With an "Only AI" filter**:
  https://logicnow.io/hn/index.html?view=https://raw.githubusercontent.com/dan-lovelace/hackernews-reader-filters/refs/heads/main/views/only-ai.json -
  The opposite of the "No AI" filter where only AI-related posts are displayed.
- **No view**: https://logicnow.io/hn/index.html - Just the list of stories
  without any filters.

You are welcome to write and host your own JSON filters anywhere (GitHub is
easy, [instructions below](#creating-a-view-with-github)) and pass its URL to
the reader's `view` query parameter. The client reads from your URL and updates
content dynamically based on your filters.

## Views

A sample view with all possible filters looks like this:

```json
{
  "schema_version": 1,
  "name": "Sample view",
  "description": "A kitchen-sink sample view with all possible filters",
  "filters": {
    "by": {
      "effect": "deny",
      "patterns": [{ "input": "^dang$", "flags": ["i"] }]
    },
    "text": {
      "effect": "deny",
      "patterns": [{ "input": "\\bcomputer\\b", "flags": ["i"] }]
    },
    "title": {
      "effect": "deny",
      "patterns": [{ "input": "\\bPC\\b", "flags": [] }]
    },
    "url": {
      "effect": "deny",
      "patterns": [{ "input": "wikipedia\\.com", "flags": ["i"] }]
    }
  }
}
```

This view will:

- Block all stories and comments created by user whose username is exactly
  "dang"
- Block any comments that mention the word "computer"
- Block any stories that mention the word "PC" in their title
- Block all [wikipedia.com](https://wikipedia.com) articles

### Creating a view with GitHub

1. Open your favorite text editor
1. Add the following contents:
   ```json
   {
     "schema_version": 1,
     "name": "New view",
     "description": "My new view",
     "filters": {}
   }
   ```
   • NOTE: Copy this template directly, most of these are required. You may skip
   providing a description if you wish.
1. Save the file with a name such as `my-view.json`
1. Sign in to GitHub and upload the JSON file to any public repository (private
   repositories will not work with this guide)
1. Find the file in the GitHub UI and click the "Raw" button - You should see a
   page with a URL that looks like this
   https://raw.githubusercontent.com/dan-lovelace/hackernews-reader-filters/refs/heads/main/views/only-ai.json
1. Copy the URL
1. Go to https://logicnow.io/hn/index.html
1. Edit the URL in the address bar and add `?view=` to the end of the URL and
   paste your own JSON filter URL
1. Verify your new URL is formatted like this
   https://logicnow.io/hn/index.html?view=https://raw.githubusercontent.com/dan-lovelace/hackernews-reader-filters/refs/heads/main/views/only-ai.json
   and hit Enter
1. The reader will load with your filters, any parsing errors are emitted to the
   console
