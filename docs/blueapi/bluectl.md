# bluectl

[`bluectl`](https://github.com/alphauslabs/bluectl) is the official command line interface (CLI) for Alphaus services. It uses the same [API](https://alphauslabs.github.io/blueapidocs/) that powers our Ripple/Wave(Pro)/Aqua UI consoles.

## Installation
You can install `bluectl` using [Homebrew](https://brew.sh/) (MacOS, Linux, and Windows through [WSL/2](https://docs.microsoft.com/en-us/windows/wsl/install)). Run the command below in a terminal:
``` sh
$ brew install alphauslabs/tap/bluectl
```

## Authentication
`bluectl` uses API client credentials for authentication. You can generate your API credentials either from Ripple under "Tools > API Access Tokens", or Wave(Pro) under "Settings > API Access Tokens".

To validate your credentials with `bluectl`, run the command below (replace the `{client-*}` part with your actual client id and client secret values):
``` sh
$ bluectl whoami --client-id {client-id} --client-secret {client-secret}
```

If successful, it will output some information about the authenticated user.

## Environment variables
You can also store your credentials as environment variables instead of typing them everytime you run a command. Check out the "Environment setup" section [here](https://alphauslabs.github.io/docs/blueapi/authentication/#environment-setup).

With environment variables set, you should now be able to run any `bluectl` commands without the `--client-id` and `--client-secret` flags.
``` sh
$ bluectl whoami
```

## Configuration file
`bluectl` also supports authentication using a configuration file located in `$HOME/.config/alphaus/config.toml`.

``` toml
[default]
client-id = 'sample-id'
client-secret = 'sample-secret'

[beta]
client-id = 'sample-id'
client-secret = 'sample-secret'
auth-url = 'https://loginnext.alphaus.cloud/ripple/access_token'
```

You can select a profile using the `--profile` flag. For example:
``` sh
$ bluectl whoami --profile beta
```

If the configuration file exists and the `[default]` profile is set, `bluectl` will use that credentials. In this case, the `--profile default` flag can be omitted. If both environment variables and the `[default]` profile are present, `bluectl` will use the `[default]` profile.

``` sh
# This usage...
$ bluectl whoami --profile default

# is the same as...
$ bluectl whoami
```

## Usage
Finally, you can explore some of `bluectl`'s available supported commands by running:
``` sh
# Check out the main commands:
$ bluectl -h

# More information on a specific subcommand:
$ bluectl {subcommand} -h
```
