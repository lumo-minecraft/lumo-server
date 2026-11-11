# Plugin download compatibility

The image retains Minecraft 1.21.10 and Java 21. Modrinth downloads require a release advertising the requested Minecraft version, successful HTTP responses, and the published SHA-512 checksum. There is no automatic fallback to the latest release for another Minecraft version.

FancyNpcs is pinned to Modrinth version aBqGwA1m (2.9.2): its loader targets Java 21 (class version 65). Newer releases can target Java 25 and interrupt Paper plugin discovery, causing secondary missing-dependency errors for otherwise valid plugins.

EssentialsX is explicitly pinned to nY6VN1XH (2.22.0), whose metadata lists representative patch versions rather than 1.21.10. Explicit pins are compatibility exceptions and must pass the same server smoke tests. Keep them under review when upgrading Minecraft or Java.

CI queries registered plugin versions over RCON and rejects disable events; download and remapping log lines alone do not establish successful loading. Here-strings avoid early-closing grep pipelines reporting SIGPIPE under pipefail.

Delivery and acceptance: https://linear.app/seventwo/issue/ST-528 and https://github.com/lumo-minecraft/lumo-server/pull/29.

BlueMap 5.x requires vertical bounds inside `render-mask`; top-level `min-y` and `max-y` cause configuration loading to fail. CI requires the BlueMap HTTP endpoint to respond, since an enable log line and plugin registration can survive an asynchronous load failure.

During initial world generation, RCON can return an empty plugin-version reply while the plugin remains active. CI retries empty replies at most five times; nonempty missing-plugin replies and disable events still fail immediately.
