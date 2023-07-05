# nix-os-config

This repository contains my personal NixOS configuration, which defines the desired state of my NixOS-based system including package installations, system services, networking, users, and more. With NixOS, I enjoy the benefits of a purely functional package manager and a declarative system configuration approach. This readme provides an overview of the repository and its contents.

## Repository Structure

The repository is structured as follows:

-   **`configuration.nix`**: The main configuration file that describes the system state. It contains settings for system parameters, package management, filesystems, services, networking, users, and hardware configuration. This file serves as the entry point for customizing the NixOS installation.
    
-   **`hardware`**: This directory contains hardware-specific configuration files. It includes settings for kernel modules, graphics drivers, and input devices. Each file represents a specific hardware setup and can be included in the main configuration as needed.
    
-   **`services`**: In this directory, you'll find service-specific configurations. Each file corresponds to a particular service or daemon, enabling fine-grained control over system services. Examples may include web servers, databases, and monitoring tools.
    
-   **`users`**: The `users` directory contains user-specific configuration files. Here, you can define properties for individual users, such as home directories, login shells, and SSH keys. It also allows for user-specific settings and permissions.
    
-   **`helpers`**: This directory houses helper scripts or custom Nix expressions that can assist in the configuration process. These scripts provide reusable functions or modules to simplify certain aspects of the configuration.
    
-   **`README.md`**: This file provides an overview of the repository and its contents, guiding users through the configuration process.
    

## Getting Started

To use this configuration repository, follow these steps:

1.  Clone the repository to your local machine: `git clone <repository-url>`
    
2.  Open the `configuration.nix` file in a text editor. Customize the system parameters, package installations, services, networking, and other sections based on your requirements. Feel free to refer to the existing configuration and documentation for guidance.
    
3.  Check the `hardware` directory and select any relevant hardware-specific configurations for your system. Include them in the main configuration file using the `imports` directive.
    
4.  Explore the `services` directory to see if any additional services or daemons are available that you would like to include in your system. Include the necessary files in the main configuration file as needed.
    
5.  Navigate to the `users` directory and adjust the user-specific configurations according to your preferences. Add or modify user settings, including home directories, login shells, and SSH keys.
    
6.  Optionally, utilize the scripts or Nix expressions available in the `helpers` directory to simplify certain aspects of your configuration. These can be imported and used within the main configuration file or other relevant sections.
    
7.  Once you've made the desired changes, save the `configuration.nix` file.
    
8.  Build and activate the new system configuration by running `sudo nixos-rebuild switch`. This command will ensure that the dependencies are satisfied and activate the changes on the next system reboot.
    

## Contributing

Contributions to this repository are welcome! If you have improvements, bug fixes, or new features to propose, please create a pull request. Ensure that your changes align with the existing style and conventions used in the repository.

## License

This repository is licensed under the [GNU General Public License v2.0](https://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html#SEC1). Feel free to modify and distribute the configuration to suit your needs. However, please note that the repository may include third-party software or configuration files subject to different licenses. Ensure compliance with those licenses when using the corresponding parts of the configuration.

## Acknowledgments

I would like to acknowledge the NixOS community and the creators of Nix for their invaluable contributions. Their work enables us to enjoy the benefits of a functional and reproducible operating system.