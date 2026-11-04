# Universal Ren'Py iOS Builder

Build Ren'Py games into unsigned iOS IPA files directly through GitHub Actions.

No local Mac or Xcode installation required. Simply provide your game name, Ren'Py SDK version, and release tag.

![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Automated-blue?logo=githubactions)
![Platform](https://img.shields.io/badge/Platform-iOS-lightgrey?logo=apple)
![Ren'Py](https://img.shields.io/badge/Engine-Ren'Py-red)

## Features

- **Universal:** Build different Ren'Py games using the same workflow.
- **Configurable:** Choose the Ren'Py SDK version.
- **Live2D support:** Optional, using a Cubism SDK zip you provide (see [Live2D games](#live2d-games)).
- **Automated:** Downloads game assets and prepares the Xcode project.
- **Unsigned builds:** No Apple Developer certificate required during compilation.
- **Cloud-based:** Builds run on GitHub-hosted macOS runners.
- **Ready to download:** IPA files are uploaded as workflow artifacts.

## Requirements

- A GitHub repository with Actions enabled.
- A Ren'Py game compatible with the selected SDK.
- A `game.zip` archive containing your game's `game` folder.

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

## Live2D games

If your game uses Live2D models, Ren'Py needs the Live2D Cubism Core library to build. Live2D does not allow redistribution of this library, so the workflow does not include it. You supply your own copy:

1. **Use a private repository.** Release files in a public repository can be downloaded by anyone, which would redistribute the Cubism files and break Live2D's license. Games that don't use Live2D can stay in a public repository.
2. **Download the Cubism SDK for Native** from [Live2D's website](https://www.live2d.com/en/sdk/download/native/) and accept their license.
3. **Use version 5-r.4.1 or older.** Versions from 5-r.5 onward (Core 06) are not compatible with Ren'Py yet, and the build will stop with an error.
4. **Attach the zip, unchanged, to the same release as `game.zip`.** Its name must start with `CubismSdkForNative` (for example `CubismSdkForNative-5-r.4.1.zip`).
5. Run the workflow as usual. If a Cubism zip is found in the release, the workflow installs it automatically. If there isn't one, it assumes the game has no Live2D.

Notes:

- The Cubism zip is large, so attach it to the release rather than committing it to the repository.
- Private repositories use GitHub Actions minutes, and macOS minutes are billed at a higher rate than Linux ones. Check your plan's allowance.

## Building

Navigate to **Actions → Universal Ren'Py iOS Builder → Run workflow**.

| Input | Description | Default |
|:---|:---|:---|
| `game_name` | Name of the game | Required |
| `renpy_version` | Ren'Py SDK version | `8.3.6` |
| `release_tag` | Release containing `game.zip` (and the Cubism zip for Live2D games) | `1.0` |

Once submitted, GitHub Actions will automatically:

1. Download the game assets (and the Cubism zip, if present).
2. Extract and prepare the Ren'Py project.
3. Download the specified Ren'Py SDK and iOS support package.
4. Install Live2D Cubism Core, if you provided it.
5. Generate the Xcode project.
6. Compile the application without code signing.
7. Package the application into an IPA.
8. Upload the finished IPA as an artifact.

## Downloading the IPA

After a successful build:

1. Open the completed workflow run.
2. Find the **Artifacts** section.
3. Download the generated IPA.

Output naming:

`<GameName>-unsigned.ipa`

## Installing on iOS

The generated IPA is unsigned and cannot normally be installed directly on an iPhone.

You will need to sign it using a suitable sideloading solution:

- [AltStore](https://altstore.io/)
- [Sideloadly](https://sideloadly.io/)

A valid signing method and provisioning profile may be required.

## Troubleshooting

| Error | Cause and fix |
|:---|:---|
| `Could not load Live2D ... libLive2DCubismCore.dylib was not found` | Your game uses Live2D but no Cubism zip was attached to the release. Attach one (see [Live2D games](#live2d-games)). |
| `This Cubism SDK is too new for Ren'Py` or `csmGetDrawableRenderOrders not found` | The Cubism SDK is version 5-r.5 or newer. Use 5-r.4.1 or older. |
| `Launching the project failed` | Ren'Py could not start your game. Look for the first error in the log above this message. |

## Notes

- The selected SDK should match the version used to develop the game.
- Native extensions and additional dependencies may require modifications to the workflow.
- The game name determines the output filename, not necessarily the installed app's display name.
- Existing save files are excluded from the build.
- Workflow artifacts are retained for 14 days by default.

## Tested Games

- Summertime Saga (tested with SDK 8.5.3)
- Beast Control (tested with SDK 8.3.6 even though the game's original SDK is 8.3.0 which didn't work in the workflow.)
- Motel Heat (tested with SDK 8.5.0)
- Edgefield (tested with SDK 8.3.6)

## License

This repository contains an automated build workflow. Game assets, scripts, and third-party materials remain subject to their respective licenses. The Live2D Cubism SDK is proprietary software owned by Live2D Inc.; you are responsible for complying with its license terms.
