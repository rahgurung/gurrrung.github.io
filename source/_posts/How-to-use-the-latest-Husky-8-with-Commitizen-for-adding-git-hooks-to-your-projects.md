---
title: >-
  How to use the latest Husky 8 with Commitizen for adding git hooks to your
  projects
readtime: 3
thumbnail: >-
  /How-to-use-the-latest-Husky-8-with-Commitizen-for-adding-git-hooks-to-your-projects/header.png
date: 2022-06-17 22:55:03
description: Guide to setup git hooks in your project using Husky and Commitizen
tags:
  - Git
  - Husky
  - Commitizen
  - Web Development
  - Git Hooks
---

I’ve been trying to setup [<u>Husky</u>](https://typicode.github.io/husky/#/) with [<u>Commitizen</u>](https://github.com/commitizen/cz-cli) to add [<u>git hooks</u>](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) to my project. I wanted if when anyone does git commit on the project, it should open the Commitizen menu for creating nice commit messages. But seems like recently Husky released its 8 version which has breaking changes, which means most of the online articles using package.json to achieve the above goal are outdated now and won’t work. I did some research and dived into the documentation to see what should be the changes to setup Commitizen for Husky 8. Below given are steps about how I achieved it.

---

 1. Start with installing Husky using the automatic setup command given below. This will install Husky and create a `pre-commit` hook in the `.husky/pre-commit` file. The `pre-commit` hook is run before we even write the commit message. It is used to inspect the code before we commit it, lint checks and tests commands can be placed in this file if required.

{% codeblock lang:js %}
    npx husky-init && npm install
{% endcodeblock %}

>  Don’t place your Commitizen command in `pre-commit` file as it will not be able to add the commit message creating empty messages even after input.

2. Let’s install Commitizen and a famous commit message convention to be followed by our project.

{% codeblock lang:js %}
    npm install -g commitizen commitizen init cz-conventional-changelog --save-dev --save-exact
{% endcodeblock %}

3. Now earlier we could just add the command we want to run with git hooks in  `package.json` itself but now this is deprecated. To add Commitizen to git hooks we have to create a file in `.husky` folder with the same name as the git hook we want. For Commitizen we need `prepare-commit-msg` hook. `prepare-commit-msg` hook is run before the commit message editor opens but after the default message is created. It lets us edit the default message of our commit giving us the ability to use Commitizen command here. Below is given the Husky command which will create the file and also add the command triggering Commitizen to the same file.

{% codeblock lang:js %}
    npx husky add .husky/prepare-commit-msg 'exec < /dev/tty && npx cz --hook || true'
{% endcodeblock %}

>  That’s all. Try running git commit now and you will see nice input field open up which helps create standard git commit messages.
