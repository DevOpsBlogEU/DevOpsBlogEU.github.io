---
layout: post
title:  "Texting from GitHub Pull Requests"
date:   2020-04-18 19:56:39 +0200
categories: github 46elks
---
GitHub Actions is a new feature from GitHub, the largest Git provider. Actions allow a simple way to automate stuff at GitHub, like IFTTT but with YAML. One can run a build and maybe a deploy, welcome new users, close stale issues and pull requests, etc, and also send SMS messages.

Sending an SMS when something happens is useful in many ways, but in this example I'll just be sending an SMS on changes in a pull request.

# Prerequisites

You'll need a GitHub Account and a [46elks][46elks-homepage] account with credits.

The 46elks API credentials must've been added as secrets to your repo:

1. Open the Repository **Settings** tab and select **Secrets**.
1. Add the secrets `ELKS_API_USERNAME`, `ELKS_API_PASSWORD`, values can be found at [46elks.se/account][46elks-account].

# Setting up the Action

1. Head over to the **Actions** tab in the repo.
1. Choose **Setup a workflow yourself**.
1. Name the Action file **pr-sms.yml** (you can choose any name you want).
  1. Set `name` to `Pull Request SMS`.
  1. Set `on:` to just `pull_request`.
1. Search the Marketplace for "46elks" and copy-paste the example code to your pipeline under **Steps**.
1. **Optional:** *Remove the _other_ steps defined if you don't want to keep them.*
1. Specify:
  1. The `apiUsername` as `$\{{ secrets.ELKS_API_USERNAME }}`.
  1. The `apiPassword` as `$\{{ secrets.ELKS_API_PASSWORD }}`.
  1. The `to` as your phone number(s) in the format `+46701234567,+46702345678` (comma-separated).
  1. The `from` as `DevOpsBlog`.
  1. The `message` as `Hello from GitHub Actions!`.
  
While the phone number(s) could be in plain text I'd prefer to keep them as secrets to not get spammed.

The `${{ }}` stuff allows you to use variables. The `secrets` are pre-defined by *you* and you need to set them up.

Here's what the pipeline should look like (feel free to copy-paste):

{% highlight yaml %}
name: Pull Request SMS
on: pull_request
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: 46elks SMS
      uses: 46elks/gh-actions-sms@v1.0.2
      with:
        apiUsername: $\{{ secrets.ELKS_API_USERNAME }}
        apiPassword: $\{{ secrets.ELKS_API_PASSWORD }}
        to: +46701234567
        from: DevOpsBlog
        message: Hello from GitHub Actions!
{% endhighlight %}

Press **Start Commit** and commit it to the branch you want, then check if you've received your SMS 😁.

[46elks-homepage]: https://46elks.se
[46elks-account]: https://46elks.se/account