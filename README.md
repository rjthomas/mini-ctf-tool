# Mini CTF tool

Mini CTF tool is a quick and easy tool to manage the challenges for a CTF in
a controlled, automated fashion.

It cleanly handles challenge creation and deployment scripts as well as
integrating with the [CTFd](https://github.com/CTFd/CTFd) platform as a
scoreboard.

## Installation

Simply copy the `ctftool.py` script into the root directory of your CTF
challenge directory.

The script depends on:

- colorama
- requests
- pyyaml

To install all of the dependencies:

    $ pip3 install colorama requests pyyaml
    
### Upgrading

To upgrade your existing installation (and overwrite the existing script):

    $ ./ctftool.py upgrade

## Usage

Ctftool automatically detects `challenge.json` and `challenge.yaml` files in
the `challenges/` directory.

    $ mkdir -p challenges/demo
    $ cat << EOF > challenges/demo/challenge.yaml
    name: demo
    category: web
    description: >
        Just a demonstration challenge!
    flags:
    - "FLAG{demo}"
    files:
    - flag.txt
    points: 50
    EOF

List all challenges:

    $ ./ctftool.py list
    [web] demo - challenges/demo/challenge.yaml

Validate all challenge configs:

    $ ./ctftool.py validate

Upload the challenges to CTFd:

    $ ./ctftool upload https://demo.ctf.io -u USERNAME -p PASSWORD

## Documentation

Fields:

- `name`

  Name of the challenge. Must be unique.

- `category`

  The "type" of challenge. These correspond to CTFd categories, and
  challenges will be shown grouped into these categories.

- `description`

  Plain text description of the challenge, may include some arbitrary HTML.

- `points`
  How many points the challenge is worth

- `flags`
  A list of strings that are valid flag submissions.

  If the flag starts and ends with a `/`, e.g. `/FLAG{demo}/`, the internal
  parts will be interpreted as a regex.

- `files`

  A list of files that should be uploaded to CTFd as part of the challenge.

  The path to the file should be relative to the directory of the
  corresponding challenge file.

Note that while ctftool interprets all of the above fields, it will not give
warnings/errors on unknown fields. This means that you can use any additional
keys for your own purposes.
