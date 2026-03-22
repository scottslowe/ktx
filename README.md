# ktx

## Overview

Managing Kubeconfig files can become tedious when you have multiple clusters and contexts to switch between. `ktx` aims to reduce friction caused by switching between various configurations.

`ktx` assumes that you use separate Kubeconfig files for each cluster (or group of clusters), and takes the approach of modifying the `KUBECONFIG` environment variable to select the desired config.

`ktx` is pronounced as "k thanks".

## Getting Started

### Prerequisites

* Your shell is Bash or Zsh. (Zsh is supported, but `ktx` has primarily been tested with Bash.)
* `git` is installed.

### Installation

```sh
# Clone the ktx repo
git clone https://github.com/scottslowe/ktx.git
cd ktx

# Install the bash function
cp ktx "${HOME}"/.ktx

# Add this to your "${HOME}/".bash_profile (or similar)
source "${HOME}"/.ktx

# Install the auto-completion
cp ktx-completion.sh "${HOME}"/.ktx-completion.sh

# Add this to your "${HOME}/".bash_profile (or similar)
source "${HOME}"/.ktx-completion.sh

# Reload your shell
exec bash
```

The location of `ktx` and `ktx-completion.sh` are not important; you can put them in your home directory as hidden files (as in the example above), or you can copy them to your `~/.local/bin` directory.

### Usage

Once `ktx` is installed you can use it as auto-complete:

```sh
$ kubectl get po
The connection to the server localhost:8080 was refused - did you specify the right host or port?
# Useful to see what clusters you have in ${HOME}/.kube/
$ ktx <tab><tab>
alpha beta gamma delta epsilon
$ ktx gamma
$ kubectl get po
No resources found.
```

## Optional Bells & Whistles

### PS1

It is helpful to display the active cluster in the command prompt.

![shows the cluster name in the command prompt](/ss.png?raw=true "ktx in action")

Although this functionality is available in `ktx`, you may find using a prompt such as [Starship](https://starship.rs) or similar to be more effective.

### Steps

1. Find out what the current value of `PS1`: `echo "${PS1}"`
2. Put `"\$(basename \${KUBECONFIG:=\"\"})" ` in front of the existing value of `PS1`

Note: The backslashes are very important. This tells bash to re-evaluate every time instead of once on load.

### Example

```sh
salazar:ktx cha$ echo "${PS1}"
\h:\W \u$

# inside .bash_profile
export PS1="\$(basename \${KUBECONFIG:=\"\"}) \h:\W \u$ "

# Reload your shell
exec bash
```

## License

This repository is licensed with the Apache 2.0 license.
