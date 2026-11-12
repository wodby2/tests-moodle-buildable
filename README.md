# Moodle boilerplate

Keep Moodle's version and your own Moodle plugins and themes in Git and deploy
them with Wodby.

Use it with the [buildable Moodle](https://github.com/wodby/service-moodle-buildable)
service (`moodle-buildable`). For Moodle without your own plugins, where Wodby
keeps Moodle up to date for you, use the
[Moodle](https://github.com/wodby/service-moodle) service instead.

## Moodle's version

`.moodle-version` names the Moodle release to run, for example `5.3.1`. Before
building the images, the pipeline runs `moodle-install-release`, which
downloads that release from Moodle's GitHub repository with its Composer
packages and adds your plugins. To upgrade Moodle, change the version and
deploy: the deployment upgrades Moodle's database.

Moodle 5.1 and later 5.x releases are supported. The file is required: the
build fails without it.

You decide when to upgrade, so watch
[Moodle's releases](https://moodledev.io/general/releases) for security fixes.

## Adding a plugin or theme

Put each plugin in its place in Moodle's `public/` directory, the way Moodle's
plugin documentation describes it:

```
public/
  local/myplugin/version.php
  mod/myactivity/version.php
  theme/mytheme/version.php
```

The build fails if a file in `public/` is already part of Moodle: plugins are
added next to Moodle's own code and never replace it.

On every deployment Wodby installs Moodle on the first deployment, or
upgrades it, and installs new plugins and upgrades the ones whose version went
up.

To remove a plugin, uninstall it in Moodle's administration first, then remove
it from the repository.

Plugins can't be installed from Moodle's web interface: the code is part of the
image.

## Files

| Path | Purpose |
| --- | --- |
| `.moodle-version` | The Moodle release to run |
| `public/` | Your Moodle plugins and themes, at their place in Moodle's tree |
| `.wodby/pipeline.yml` | The Wodby CI pipeline that puts Moodle together and builds and deploys the images |
| `.dockerignore` | Files left out of the image |
