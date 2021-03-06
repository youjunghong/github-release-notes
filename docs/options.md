---
layout: default
title: Options
---

{:.page-heading}
# Options

Below all the options for github-release-notes.
You can find [Global options](#global-options), [Options for the release action](#release-options) and [Options for the changelog action](#changelog-options).

To use an option in your terminal, prefix it with `--` _(e.g. `gren --data-source=commits`)_
To pass it to the `GithubReleaseNotes` class, in the [configuration file](#configuration-file) or in the [grunt task](https://github.com/github-tools/grunt-github-release-notes), they need to be `camelCase`.

### Global options

| Command | Options | Description | Default |
| ------- | ------- | ----------- | ------- |
| `username` | **Required** | The username of the repo _e.g. `github-tools`_ | `null` |
| `repo` | **Required** | The repository name _e.g. `github-release-notes`_ | `null` |
| `api-url` | **Optional** | Override the GitHub API URL, allows **gren** to connect to a private [GHE](https://enterprise.github.com/) installation. _e.g. `https://my-enterprise-domain.com/api/v3`_ | `null` |
| `action`| `release` `changelog` | The **gren** action to run. _(see details below for changelog generator)_ | `release` |
| `tags`    |   `0.1.0` `0.2.0,0.1.0` `all` |   A specific tag or the range of tags to build the release notes from. You can also specify `all` to write all releases. _(To override  existing releases use the --override flag)_ | `false` |
| `ignore-labels` | `wont_fix` `wont_fix,duplicate` | Ignore the specified labels. | `false` |
| `ignore-issues-with` | `wont_fix` `wont_fix,duplicate` | Ignore issues that contains one of the specified labels. | `false` |
| `data-source` | `issues` `commits` `milestones` | The informations you want to use to build release notes. For more informations about the `milestones` option, [have a look here]({{ "example#milestones" | relative_url }}) | `issues` |
| `milestone-match` | **String** | The title that the script needs to match to link the release to the milestone. [See further details]({{ "example#milestones" | relative_url }}) | `null` |
| `prefix` | **String** `e.g. v` | Add a prefix to the tag version. | `null` |
| `override` | **Flag** | Override the release notes if existing. | `false` |
| `include-messages` | `merge` `commits` `all` | Filter the messages added to the release notes. _Only used when `data-source` used is `commits` | `commits` |
| `group-by` | `label` `{...}` | Group the issues using the labels as group headings. You can set custom headings for groups of labels. [See example]({{ "example#group-by" | relative_url }}) | `false` |
| `only-milestones` | **Flag** | Add to the release bodies only the issues that have a milestone | `false` |

### Release options

| Command | Options | Description | Default |
| ------- | ------- | ----------- | ------- |
| `draft` | **Flag** | Set the release as a draft. | `false` |
| `prerelease` | **Flag** | To set the release as a prerelease. | `false` |

### Changelog options

| Command | Options | Description | Default |
| ------- | ------- | ----------- | ------- |
| `generate` | **Flag** | Generate the changelog with GithubReleaseNotes rather then using the repo releases | `false` |
| `changelog-filename` | **String**, like `changelog.md` | The name of the changelog file. | `CHANGELOG.md` |

---

## Configuration file

You can create a configuration file where the task will be ran, where to specify your options.
The options in the file would be camelCase *e.g*:

```json
{
    "action": "release",
    "timeWrap": "history",
    "dataSource": "commits",
    "ignoreIssuesWith": [
        "wontfix",
        "duplicate"
    ]
}
```

The accepted file extensions are the following:

- `.grenrc`
- `.grenrc.json`
- `.grenrc.yml`
- `.grenrc.yaml`
- `.grenrc.js`

#### Templates

You can configure the output of **gren** using templates. Set your own configuration inside the config file, which will be merged with the defaults, shown below:

{% raw %}
```json
{
    "template": {
        "commit": "- {{message}}",
        "issue": "- {{labels}} {{name}} [{{text}}]({{url}})",
        "label": "[**{{label}}**]",
        "noLabel": "closed",
        "group": "\n#### {{heading}}\n",
        "changelogTitle": "# Changelog\n\n",
        "release": "## {{release}} {{date}}\n{{body}}",
        "releaseSeparator": "\n---\n\n"
    }

}
```
{% endraw %}

If you're using a `.grenrc.js` config file, you can use JavaScript to manipulate the templates using functions as values.
The function will have an object as first parameter, containing all the values to display. _i.e._

```javascript
/* .grenrc.js */

module.exports = {
    template: {
        issue: function (placeholders) {
            return '- ' + placeholders.labels + ' | ' + placeholders.name.toLowerCase();
        }
    }
}
```
