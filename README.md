# omv-pihole-rpi

# Pi-hole and OpenMediaVault Installation Guide on Raspberry Pi

This guide will help you install Pi-hole and OpenMediaVault (OMV) on a Raspberry Pi with 4GB RAM. Follow the steps carefully to set up your ad-blocking DNS service and NAS server.

## Prerequisites

- Raspberry Pi 4 (4GB RAM)
- MicroSD card (16GB or larger)
- External HDD/SSD for NAS storage (if required)
- Stable internet connection
- Keyboard, mouse, and monitor for setup

## Step 1: Install Raspberry Pi OS

1. Download the latest Raspberry Pi OS from the [official website](https://www.raspberrypi.org/software/operating-systems/).
2. Use Raspberry Pi Imager to write the OS image to the MicroSD card.
3. Insert the MicroSD card into your Raspberry Pi and boot it up.
4. Follow the on-screen instructions to complete the initial setup.

## Step 2: Install Pi-hole

1. Open a terminal on your Raspberry Pi.
2. Update the package lists:

    ```sh
    sudo apt update
    sudo apt upgrade -y
    ```

3. Install Pi-hole by running the installation script:

    ```sh
    curl -sSL https://install.pi-hole.net | sudo bash
    ```

4. Follow the on-screen instructions to complete the Pi-hole installation.

## Step 3: Change Pi-hole Default Port

1. Stop the Pi-hole lighttpd service:

    ```sh
    sudo systemctl stop lighttpd
    ```

2. Edit the lighttpd configuration file to change the default port (80 to 8080):

    ```sh
    sudo nano /etc/lighttpd/lighttpd.conf
    ```

3. Find the following line and change the port number:

    ```plaintext
    server.port = 80
    ```

    Change it to:

    ```plaintext
    server.port = 8080
    ```

4. Save the file and exit the editor.
5. Restart the lighttpd service:

    ```sh
    sudo systemctl start lighttpd
    ```

6. Verify that Pi-hole is accessible on the new port (`http://<your-pi-ip-address>:8080`).

## Step 4: Install OpenMediaVault

1. Open a terminal on your Raspberry Pi.
2. Install necessary packages:

    ```sh
    sudo apt install -y wget
    ```

3. Download and install the OMV installation script:

    ```sh
    wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
    ```

4. Once the installation is complete, reboot your Raspberry Pi:

    ```sh
    sudo reboot
    ```

5. Access the OMV web interface by navigating to `http://<your-pi-ip-address>` in a web browser.


## Troubleshooting

### Common Issues and Solutions

- **Network Service Issues**: If you encounter network service issues, ensure that `rpcbind` is enabled and running:

    ```sh
    sudo systemctl enable rpcbind
    sudo systemctl start rpcbind
    ```

- **CORS Errors**: Ensure the CORS headers are correctly added to the nginx configuration file as shown above.

- **nginx Errors**: Check the nginx error logs for specific issues:

    ```sh
    sudo tail -f /var/log/nginx/openmediavault-webgui_error.log
    ```

- **Service Status**: Verify the status of OMV services:

    ```sh
    sudo systemctl status openmediavault-engined
    sudo systemctl status nginx
    ```

### Additional Resources

- [OpenMediaVault Documentation](https://openmediavault.readthedocs.io/)
- [Pi-hole Documentation](https://docs.pi-hole.net/)

By following these steps, you should have Pi-hole and OpenMediaVault successfully installed and configured on your Raspberry Pi. If you encounter any issues, refer to the troubleshooting section or consult the official documentation for further assistance.
