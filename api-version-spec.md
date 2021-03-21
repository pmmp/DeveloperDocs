# API versioning
PocketMine-MP plugins are required to declare which API versions they are compatible. This is used to decide whether or not to load a plugin, and to gracefully degrade when the plugin is not compatible with the given server version.

## Description of API version system
As of PocketMine-MP 3.0.0, the API version is the same as the server version. This version is a [semantic major.minor.patch version number](https://semver.org/).

### Definitions
Semver is roughly defined as the following:
- Major version bump: Breaking changes - the public API has changed in such a way that it breaks thing depending on it.
- Minor version bump: Feature additions or big changes which do not break API. This can include API methods becoming deprecated, new API features being added, but should not break plugins designed for previous minor versions.
- Patch version bump: Usually bug fixes. These shouldn't break the API nor cause any significant alteration to the description of a version.

PocketMine-MP uses a condition of `==.>=.>=` (or `eq.ge.ge` if you prefer `bash` notation) for comparing API versions. This means that:
- The major version **MUST be the same** to be compatible
- The server's minor version **MUST be AT LEAST the same** as the plugin's, although it can be greater.
- The server's patch version **MUST be AT LEAST the same** as the plugin's, but can also be greater.

The PocketMine-MP developers strive to ensure that any non-major version does not break API compatibility with plugins. This means that a plugin written to target `3.1.1` should also work on any future `3.x.y` version, but **not** `4.0.0` or any future major versions.

### Examples

| Server version | Plugin version | Compatible | Reason |
|:---------------|:---------------|:----------:|:-------|
| 4.0.0 | 3.0.0 | NO | Major versions are different. |
| 3.1.0 | 3.0.0 | YES | Server has a greater minor version than the plugin, guaranteeing compatibility. |
| 3.0.0 | 3.1.0 | NO | Plugin requires new features not found in the given server version |
| 3.0.1 | 3.0.0 | YES | Server has a greater patch version than the plugin, guaranteeing compatibility. |
| 3.0.0 | 3.0.1 | NO | Plugin requires newer bug fixes not found in the given server version |

### How to choose your supported API version
You should choose the **minimum version that supports the things that you need**.

For example, if the feature you want was first added in `3.1.0`, you can require `3.1.0` as a minimum even if the currently supported version is `3.2.0`. This is to ensure that users get a smoother experience no matter what version of PocketMine-MP they are using. 

Of course, you should test your plugin on your minimum version to make sure that everything works correctly. However, it may of course not make sense to require a lower version if the earlier versions are end-of-life.

### Common pitfalls and misconceptions
- The API version **is not a magic number**. Changing it blindly **will not make a plugin magically work** on a version it wasn't designed for.
- It is **not required to write out every single compatible version** in your plugin's manifest. Only the **minimum required version of each major version** is required.
  - Good: `[3.1.0]` (single minimum version), `[3.2.0, 4.0.0]` (minimum version from two different major branches)
  - Bad: `[3.1.0, 3.1.1, 3.1.2]` - this is **not** desired and the superfluous versions will be ignored.
- Ensure that your plugin **actually runs on the versions you specified**. Versions which it a) crashes on, or b) doesn't work as expected, should be removed from your versions list.
