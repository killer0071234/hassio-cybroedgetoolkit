# Home Assistant Community App: Cybro Edge Toolkit

[![Release][release-shield]][release] ![Project Stage][project-stage-shield] ![Project Maintenance][maintenance-shield]

Cybro Edge Toolkit from [Cybrotech][cybrotech].

## About

The Cybro Edge Toolkit connects PLCs from Cybrotech / Robotina to other
systems. This app runs the Cybro Edge Toolkit from [Cybrotech][cybrotech]
(scgi_server v3.3.1, mqtt_client v1.0.4, data_logger v3.2.4):

- **SCGI server** – communicates with the controllers
- **MQTT client** – publishes controller variables to an MQTT broker
- **Data logger** – stores controller variables in a MySQL / MariaDB database

See [repository readme][addon-repo-install] on how to install the cybro app in Home Assistant.

See the [app documentation](cybroedgetoolkit/DOCS.md) for configuration.

**If you have questions or feedback please**

- via Issues and pull requests in the Github repository

## Updating the toolkit

The toolkit is downloaded from Cybrotech when the image is built. See the
[contributing guide](.github/CONTRIBUTING.md#update-the-cybro-edge-toolkit) on
how to update it.

To start and debug the app during development, see the
[contributing guide](.github/CONTRIBUTING.md#development).

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
[maintenance-shield]: https://img.shields.io/maintenance/yes/2026.svg
[project-stage-shield]: https://img.shields.io/badge/project%20stage-experimental-yellow.svg
[release-shield]: https://img.shields.io/badge/version-1.0.0-blue.svg
[release]: https://github.com/killer0071234/hassio-cybroedgetoolkit/releases/tag/v1.0.0
[addon-repo-install]: https://github.com/killer0071234/ha-addon-repository#installation
[cybrotech]: https://cybrotech.com/
