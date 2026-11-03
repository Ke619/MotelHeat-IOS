<div # Universal Ren’Py iOS Builder </div>

A GitHub Actions workflow that automatically builds Ren’Py games into unsigned iOS IPA files using the official Ren’Py SDK and iOS support package.

No local Mac, Xcode installation, or manual project configuration required. Everything is built in GitHub Actions.


## Features

* Supports multiple Ren’Py games.
* Choose your Ren’Py SDK version.
* Automatically downloads the game from a GitHub Release.
* Automatically generates the Xcode project.
* Builds without code signing.
* Packages the result as an unsigned IPA.
* Uploads the IPA as a downloadable GitHub Actions artifact.

## Requirements

Before running a build, make sure you have:

* A GitHub repository containing this workflow.
* A Ren’Py game compatible with the selected SDK version.
* A game.zip archive containing the game’s game folder.
* A GitHub Release containing that archive.

## Setup

1. Add the workflow

Create the following file in your repository:

.github/
└── workflows/
    └── build-ios.yml

Paste the universal builder workflow into build-ios.yml.

2. Prepare your game

Locate your Ren’Py project’s game folder.

Compress it into a ZIP archive named:

game.zip

The archive should contain your game’s scripts, images, audio, archives, and other required assets.

3. Create a GitHub Release

1. Open your repository’s Releases page.
2. Create a new release.
3. Set a release tag (for example, 1.0).
4. Attach game.zip.
5. Publish the release.

The release tag must match the one entered when running the workflow.

## Building your game

1. Navigate to the Actions tab.
2. Select Universal Ren’Py iOS Builder.
3. Click Run workflow.
4. Enter the following information:

Input	Description	Default
Game name	Name used for the IPA and artifact	Required
Ren’Py version	SDK version used to build the game	8.3.6
Release tag	Release containing game.zip	1.0

5. Click Run workflow and wait for the build to finish.

## Downloading the IPA

Once the workflow completes successfully:

1. Open the completed workflow run.
2. Scroll down to Artifacts.
3. Download the generated IPA.

The output follows this naming convention:

<GameName>-unsigned.ipa

For example:

Edgefield-unsigned.ipa
MotelHeat-unsigned.ipa

## Installing on iOS

The generated IPA is unsigned and cannot normally be installed directly on an iPhone.

You must sign it using an appropriate sideloading or Apple code-signing solution, such as:

* AltStore
* Sideloadly

A valid signing method and provisioning profile may be required.

## Important notes

* The game must be compatible with the selected Ren’Py SDK version.
* The workflow uses the official Ren’Py SDK and matching iOS support package.
* Game-specific dependencies or native extensions may require additional configuration.
* The game name controls the output filename; it does not automatically change the app’s display name or bundle identifier.
* Existing saves are excluded from the packaged game.
* GitHub Actions artifacts are retained for 14 days by default.

## License

This repository provides an automated build workflow. Game assets, scripts, and other third-party materials remain subject to their respective licenses.
