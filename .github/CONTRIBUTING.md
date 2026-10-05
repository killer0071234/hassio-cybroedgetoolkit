# Contributing

When contributing to this repository, please first discuss the change you wish
to make via issue, email, or any other method with the owners of this repository
before making a change.

Please note we have a code of conduct, please follow it in all your interactions
with the project.

## Issues and feature requests

You've found a bug in the source code, a mistake in the documentation or maybe
you'd like a new feature? You can help us by submitting an issue to our
[GitHub Repository][github]. Before you create an issue, make sure you search
the archive, maybe your question was already answered.

Even better: You could submit a pull request with a fix / new feature!

## Pull request process

1. Search our repository for open or closed [pull requests][prs] that relates
   to your submission. You don't want to duplicate effort.

1. You may merge the pull request in once you have the sign-off of two other
   developers, or if you do not have permission to do that, you may request
   the second reviewer to merge it for you.

## Development

### Debug the toolkit without Home Assistant

This builds the image and starts the SCGI server directly, without the Home
Assistant startup scripts:

```bash
cd cybroedgetoolkit
docker build -t cybroedgetoolkit-dev .
docker run --rm -it --network host --entrypoint sh cybroedgetoolkit-dev -c '
  mkdir -p /config /data/log /data/alc &&
  cp "$CET_HOME"/defaults/* /config/ &&
  sed -i "s/^enabled = false/enabled = true/; s/^verbose_level = ERROR/verbose_level = DEBUG/" /config/config.ini &&
  cd "$CET_HOME/app" &&
  exec python3 scgi_server/start.py'
```

In a second terminal, check that the server answers:

```bash
curl "http://localhost:4000/?sys.server_version"
```

Start `mqtt_client/start.py` or `data_logger/start.py` the same way to test the
other services.

### Run the linters

CI runs these checks on every pull request. To run them locally:

```bash
# shell scripts
docker run --rm -v "$PWD:/w" -w /w koalaman/shellcheck -s bash \
  cybroedgetoolkit/rootfs/etc/s6-overlay/s6-rc.d/*/run \
  cybroedgetoolkit/rootfs/etc/s6-overlay/s6-rc.d/*/finish

# Dockerfile
docker run --rm -i hadolint/hadolint < cybroedgetoolkit/Dockerfile

# yaml
docker run --rm -v "$PWD:/w" -w /w cytopia/yamllint -c .yamllint .

# formatting
docker run --rm -v "$PWD:/w" -w /w node:22-alpine \
  npx -y prettier --check "**/*.{json,js,md,yaml}"
```

### Update the Cybro Edge Toolkit

The toolkit is downloaded from Cybrotech when the image is built.

1. Download the new `CybroEdgeToolkit.zip` and calculate its checksum with
   `sha256sum CybroEdgeToolkit.zip`.
1. Update `CET_SHA256` (and `CET_URL` if it changed) in
   `cybroedgetoolkit/Dockerfile`.
1. Compare the new `app/config.ini` and `app/requirements.txt` with the old
   ones. The `[MQTT]` section must still contain the `ip`, `port`, `username`
   and `password` keys used by `cybro-mqtt-config`.
1. Update the component versions in `README.md`,
   `cybroedgetoolkit/.README.j2` and `cybroedgetoolkit/CHANGELOG.md`.

Afterwards, start the server as described above to make sure it still runs.

[github]: https://github.com/killer0071234/hassio-cybroedgetoolkit/issues
[prs]: https://github.com/killer0071234/hassio-cybroedgetoolkit/pulls
