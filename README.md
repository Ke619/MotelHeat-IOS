# Universal Ren'Py iOS Builder

Build Ren'Py games into unsigned iOS IPA files directly through GitHub Actions.

No local Mac or Xcode installation required. Simply provide your game name, Ren'Py SDK version, and release tag.

![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Automated-blue?logo=githubactions)
![Platform](https://img.shields.io/badge/Platform-iOS-lightgrey?logo=apple)
![Ren'Py](https://img.shields.io/badge/Engine-Ren'Py-red)

## Features

- **Universal:** Build different Ren'Py games using the same workflow.
- **Configurable:** Choose the Ren'Py SDK version.
- **Automated:** Downloads game assets and prepares the Xcode project.
- **Unsigned builds:** No Apple Developer certificate required during compilation.
- **Cloud-based:** Builds run on GitHub-hosted macOS runners.
- **Ready to download:** IPA files are uploaded as workflow artifacts.

## Requirements

- A GitHub repository with Actions enabled.
- A Ren'Py game compatible with the selected SDK.
- A `game.zip` archive containing your game's `game` folder.
- A GitHub Release containing `game.zip`.

## Setup

### 1. Add the workflow

Create the following file in your repository:

`.github/workflows/build-ios.yml`

Paste the universal workflow into it and commit your changes.

### 2. Prepare your game

Locate your Ren'Py project's `game` folder and compress it into:

`game.zip`

Make sure the archive contains the game's scripts, images, audio, archives, and other required assets.

### 3. Create a release

Create a GitHub Release and attach `game.zip`.

The default release tag is `1.0`, but you can use any tag when running the workflow.

## Building

Navigate to **Actions → Universal Ren'Py iOS Builder → Run workflow**.

| Input | Description | Default |
|:---|:---|:---|
| `game_name` | Name of the game | Required |
| `renpy_version` | Ren'Py SDK version | `8.3.6` |
| `release_tag` | Release containing `game.zip` | `1.0` |

Once submitted, GitHub Actions will automatically:

1. Download the game assets.
2. Extract and prepare the Ren'Py project.
3. Download the specified Ren'Py SDK and iOS support package.
4. Generate the Xcode project.
5. Compile the application without code signing.
6. Package the application into an IPA.
7. Upload the finished IPA as an artifact.

## Downloading the IPA

After a successful build:

1. Open the completed workflow run.
2. Find the **Artifacts** section.
3. Download the generated IPA.

Output naming:

`<GameName>-unsigned.ipa`

Examples:

- `Edgefield-unsigned.ipa`
- `MotelHeat-unsigned.ipa`

## Installing on iOS

The generated IPA is unsigned and cannot normally be installed directly on an iPhone.

You will need to sign it using a suitable sideloading solution:

- [AltStore](https://altstore.io/)
- [Sideloadly](https://sideloadly.io/)

A valid signing method and provisioning profile may be required.

## Notes

- The selected SDK should match the version used to develop the game.
- Native extensions and additional dependencies may require modifications to the workflow.
- The game name determines the output filename, not necessarily the installed app's display name.
- Existing save files are excluded from the build.
- Workflow artifacts are retained for 14 days by default.

## License

This repository contains an automated build workflow. Game assets, scripts, and third-party materials remain subject to their respective licenses.
