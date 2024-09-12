# Troubleshooting

This document covers some common node-sass issues and how to resolve them. You
should always follow these steps before opening a new issue.

## TOC

- [Installation problems](#installation-problems)
  - [404 downloading binding.node file](#404s)
- [Glossary](#glossary)
  - [Which node runtime am I using?](#which-node-runtime-am-i-using)
  - [Which version of node am I using?](#which-version-of-node-am-i-using)
  - [Debugging installation issues.](#debugging-installation-issues)
    - [Windows](#windows)
    - [Linux/OSX](#linuxosx)
- [Using node-sass with Visual Studio 2015 Task Runner.](#using-node-sass-with-visual-studio-2015-task-runner)
- [Installing node-sass 4.x with Node < 4](#installing-node-sass-4x-with-node--4)

## Installation

### 404s

If you see a 404 when trying to install node-sass, this indicates that you're trying
to install a version of node-sass that doesn't support your version of NodeJS, or
uses an alternate V8 environment (Meteor, Electron, etc...) that isn't supported
by node-sass.

```console
> node-sass@4.6.1 install /src/node_modules/node-sass
> node scripts/install.js

Downloading binary from https://github.com/sass/node-sass/releas…
Cannot download "https://github.com/sass/node-sass/releas…":

HTTP error 404 Not Found
```

If you encounter this, please check what version of NodeJs you're running (`node -v`)
and check for a supported version of node-sass for your NodeJs by checking our
[release page](https://github.com/sass/node-sass/releases).

### Proxy issues

If you work in behind a corporate proxy try setting the proxy variables. The
following is a [guide for setting this up](https://jjasonclark.com/how-to-setup-node-behind-web-proxy/).

### Running with sudo or as root

This can happen if you are install node-sass as `root`, or globally with `sudo`.
This is a security feature of `npm`. You should always avoid running `npm` as
`sudo` because install scripts can be unintentionally malicious.
Please check [npm documentation on fixing permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions).

If you must however, you can work around this error by using the `--unsafe-perm`
flag with npm install i.e.

```sh
sudo npm install --unsafe-perm -g node-sass
```

If this didn't solve your problem please open an issue with the output from
[our debugging script](#debugging-installation-issues).

### npm

Some users upgrading from previous versions of npm before 5 have found conflicts with
old lock file formats. This may be show up as a URL instead of the actual version
number when downloading the binary. EX:

```console
Downloading binary from https://github.com/sass/node-sass/releases/download/vhttps://registry.npmjs.org/node-sass/-/node-sass-4.5.3.tgz/win32-x64-57_binding.node
Cannot download "https://github.com/sass/node-sass/releases/download/vhttps://registry.npmjs.org/node-sass/-/node-sass-4.5.3.tgz/win32-x64-57_binding.node":

HTTP error 404 Not Found
```

The easiest way to get around this is just to cleanup the npm files and reinstall.

```console
rm -rf node_modules
rm package-lock.json
npm cache clean
npm install
```

## Helping us, help you

### Find what version of Node you're running

To determine which version of Node.js or io.js you are currently using run the
following command in a terminal.

```console
node -v
```

The resulting value the version you are running.

### Debugging installation issues

Node sass runs some install scripts to make it as easy to use as possible, but
some times there can be issues. Before opening a new issue please follow the
instructions for [Windows](#windows) or [Linux/OSX](#linuxosx) and provide
their output in you [GitHub issue](https://github.com/sass/node-sass/issues).

**Remember to always search before opening a new issue**.

#### Windows

Firstly create a clean work space.

```console
mkdir \temp1
cd \temp1
```

Check your `COMSPEC` environment variable.

```console
node -p process.env.comspec
```

Please make sure the variable points to `C:\WINDOWS\System32\cmd.exe`

Gather some basic diagnostic information.

```console
npm -v
node -v
node -p process.versions
node -p process.platform
node -p process.arch
```

Clean npm cache

```console
npm cache clean
```

Install the latest node-sass

```console
npm install node-sass@latest
```

Note which version was installed by running

```console
npm ls node-sass
```

```console
y@1.0.0 /tmp
└── node-sass@3.8.0
```

If node-sass couldn't be installed successfully, please publish your `npm.log`
and `npm.err` files for analysis.

You can [download reference known-good logfiles](https://gist.github.com/saper/62b6e5ea41695c1883e3)
to compare your log against.

If node-sass install successfully lets gather some basic installation information.

```console
node -p "require('node-sass').info"
```

```console
node-sass       3.8.0   (Wrapper)       [JavaScript]
libsass         3.3.6   (Sass Compiler) [C/C++]
```

If the node-sass installation process produced an error, open the vendor folder.

```console
cd node_modules\node-sass\vendor
```

Then, using the version number we gather at the beginning, go to `https://github.com/sass/node-sass/releases/tag/v<your-version>`.

There you should see a folder with same name as the one in the `vendor` folder.
Download the `binding.node` file from that folder and replace your own with it.

Test if that worked by gathering some basic installation information.

```console
node -p "require('node-sass').info"
```

```console
node-sass       3.8.0   (Wrapper)       [JavaScript]
libsass         3.3.6   (Sass Compiler) [C/C++]
```

If this still produces an error please open an issue with the output from these
steps.

#### Linux/OSX

Firstly create a clean work space.

```sh
mkdir ~/temp1
cd ~/temp1
```

Gather some basic diagnostic information.

```sh
npm -v
node -v
node -p process.versions
node -p process.platform
node -p process.arch
```

Install the latest node-sass

```sh
npm install node-sass@latest
```

Note which version was installed by running

```sh
npm ls node-sass
```

```sh
y@1.0.0 /tmp
└── node-sass@3.8.0
```

If node-sass install successfully lets gather some basic installation information.

```sh
node -p "require('node-sass').info"
```

```sh
node-sass       3.8.0   (Wrapper)       [JavaScript]
libsass         3.3.6   (Sass Compiler) [C/C++]
```

If the node-sass installation process produced an error, open the vendor folder.

```sh
cd node_modules/node-sass/vendor
```

Then, using the version number we gather at the beginning, go to `https://github.com/sass/node-sass/releases/tag/v<your-version>`.

There you should see a folder with same name as the one in the `vendor` folder.
Download the `binding.node` file from that folder and replace your own with it.

Test if that worked by gathering some basic installation information.

```sh
node -p "require('node-sass').info"
```

```sh
node-sass       3.8.0   (Wrapper)       [JavaScript]
libsass         3.3.6   (Sass Compiler) [C/C++]
```

If this still produces an error please open an issue with the output from these
steps.

### Using node-sass with Visual Studio 2015 Task Runner

If you are using node-sass with VS2015 Task Runner Explorer, you need to make
sure that the version of node.js is same as the one you installed node-sass
with. This is because for each node.js runtime modules version (`node -p process.versions.modules`)
, we have a separate build of native binary. See [#532](https://github.com/sass/node-sass/issues/532).

Alternatively, if you prefer using system-installed node.js (supposedly higher
version than one bundles with VS2015), you may want to point Visual Studio 2015
to use it for task runner jobs by [following the guidelines](http://blogs.msdn.com/b/webdev/archive/2015/03/19/customize-external-web-tools-in-visual-studio-2015.aspx).

### Installing node-sass 4.x with Node < 4

See the discussion in [this comment](https://github.com/sass/node-sass/issues/2100#issuecomment-334651235)
for a workaround. As of node-sass@v5 only Node 6 and above will be officially supported.
