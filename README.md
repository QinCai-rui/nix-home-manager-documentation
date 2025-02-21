# Nix & Home Manager (Unofficial) Documentation

This guide will help you understand what Nix and Home Manager are, and how to use them to manage your environment.

## Table of Contents
- [What is Nix?](#what-is-nix)
  - [Key Features of Nix](#key-features-of-nix)
  - [How Nix Works](#how-nix-works)
- [What is Home Manager?](#what-is-home-manager)
  - [Key Features of Home Manager](#key-features-of-home-manager)
  - [How Home Manager Works](#how-home-manager-works)
- [Useful Nix Commands](#useful-nix-commands)
- [Getting Started](#getting-started)
- [Examples](#examples)
- [Troubleshooting](#troubleshooting)
- [Applying the Configuration](#applying-the-configuration)
- [Useful Aliases](#useful-aliases)
- [Further Reading](#further-reading)
- [In-built Manuals](#in-built-manuals)
- [References](#references)
- [Contribution Guidelines](#contribution-guidelines)
- [License](#license)

## What is Nix?

Nix is a powerful package manager for Linux and other Unix-like systems that makes package management reliable and reproducible. 

### Key Features of Nix:
- **Declarative Configuration**: Describe your entire system configuration in a single file.
- **Reproducibility**: Ensures that builds are reproducible, meaning you can recreate the same environment on different machines.
- **Isolation**: Builds packages in isolation from each other, preventing dependency conflicts.
- **Rollbacks**: Easily roll back to previous configurations if something goes wrong.
- **Binary Caching**: Pre-built binaries can be cached and reused, speeding up the build process.

### How Nix Works:
Nix uses a purely functional approach to package management. This means that instead of executing commands that change the state of your system, Nix builds packages in isolation and stores them in unique directories. This ensures that different versions of a package do not interfere with each other.

## What is Home Manager?

Home Manager is a tool that allows you to manage your user configuration using the Nix package manager. 

### Key Features of Home Manager:
- **Dotfiles Management**: Manage your dotfiles (e.g., `.bashrc`, `.vimrc`) using Nix.
- **User-specific Packages**: Install and manage user-specific packages separately from the system packages.
- **Consistent Environment**: Ensure that your user environment is consistent across different machines.
- **Declarative Configuration**: Define your user environment in a single configuration file.
- **Integration with NixOS**: Seamlessly integrates with NixOS, allowing for unified configuration management.

### How Home Manager Works:
Home Manager uses Nix to manage user-specific configurations and packages. You define your entire user environment in a single configuration file (`home.nix`, usually located at `$HOME/.config/home-manager/home.nix`). This file describes everything from installed packages to configuration files for various tools and applications. Home Manager then ensures that your environment matches this configuration.

## Useful Nix Commands

Here are some useful Nix commands to help you manage your environment:

> [!IMPORTANT]  
> The command `nix-collect-garbage` is mostly an alias of `nix-store --gc`. That is, it deletes all unreachable store objects in the Nix store to clean up your system.
> 
> However, it provides two additional options, `--delete-old` and `--delete-older-than`, which *also* delete old profiles, allowing potentially more store objects to be deleted because profiles are also garbage collection roots.
> 
> These options are the equivalent of running `nix-env --delete-generations` with various augments on multiple profiles, prior to running `nix-collect-garbage` (or `just nix-store --gc`) without any flags.

- **Collect Garbage**: Remove unused packages and data.
  
  ```sh
  nix-collect-garbage
  ```

  To run garbage collection more aggressively, use:

  ```sh
  nix-collect-garbage -d  # Delete old generations of the system and user profiles
  ```
> [!WARNING]
> Deleting previous configurations makes rollbacks to them impossible.

- **Update Channels**: Update your Nix channels to the latest versions.
  
  ```sh
  nix-channel --update
  ```

- **Optimise Store**: Optimise the Nix store to save disk space.
  
  ```sh
  nix-store --optimise
  ```

- **Clean the Store**: Remove unreachable paths from the Nix store.
  
  ```sh
  nix-store --gc
  ```

## Getting Started

To get started with Nix and Home Manager, you will need to install them and set up your configuration files. Once set up, you can manage your packages and configurations declaratively, ensuring a consistent and reproducible environment across different machines.

## Examples

> [!TIP]
> See [My Raspberry Pi's Home Manager config](https://github.com/QinCai-rui/nix-home-manager-raspberrypi) for more examples!
Here are a few example configurations to help you get started:

### Example 1: Basic Configuration

```nix
{ config, pkgs, ... }:

{
  home.username = "your-username";
  home.homeDirectory = "/home/your-username";

  programs.bash.enable = true;
  programs.vim.enable = true;

  # Add more configurations as needed
}
```

### Example 2: Advanced Configuration

```nix
{ config, pkgs, ... }:

{
  home.username = "your-username";
  home.homeDirectory = "/home/your-username";

  programs.bash.enable = true;
  programs.vim.enable = true;
  programs.git.enable = true;

  home.packages = with pkgs; [
    btop
    tree
    fastfetch
  ];

  home.sessionVariables = {
    EDITOR = "nano";
  };

  # Add more configurations as needed
}
```

## Troubleshooting

Here are some common issues and their solutions:

- **Issue**: Nix channel update fails.
  - **Solution**: Ensure you have a stable internet connection and try running `nix-channel --update` again.
  
- **Issue**: Home Manager switch fails with an error.
  - **Solution**: Check your `home.nix` configuration for syntax errors or missing dependencies.

- **Issue**: Packages not found after configuration.
  - **Solution**: Make sure the packages are correctly specified in your `home.nix` file.

## Applying the Configuration

Once you have created your configuration file, you can apply it using Home Manager to set up your environment according to the defined configurations.

```sh
home-manager switch
```

## Useful Aliases

These are some **very** useful aliases that I personally use:
- `hm-rebuild` aliased to `home-manager switch |& nom`
- `nix-update` aliased to `nix-channel --update -vvv`
- `hm-edit` aliased to `export EDITOR="nano" && home-manager edit`

## Further Reading

- [Nix Manual](https://nixos.org/manual/nix/stable/)
- [Using Home Manager](https://nix-community.github.io/home-manager/index.xhtml#ch-usage)

## In-built Manuals

- Home Manager: `man home-manager`
- Nix: `nix --help`

## References

- [How Nix Works](https://nixos.org/guides/how-nix-works/)
- [Home Manager Manual](https://nix-community.github.io/home-manager/)

## Contribution Guidelines

If you would like to contribute to this documentation, please follow these guidelines:

1. Fork the repository.
2. Create a new branch for your changes.
3. Submit a pull request with a detailed description of your changes.

## License

This documentation is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.
