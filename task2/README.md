
# Explanation of Instance Setup, Nginx Installation & Hardening Steps

## 1. EC2 Instance Setup

-   Instance type is **t2.micro**, eligible for the AWS Free Tier.
-   The instance is launched inside the **public subnet**, ensuring it
    receives a public IP and is reachable through the internet.
-   A security group is attached that:
    -   Allows **HTTP (port 80)** from anywhere.
    -   Allows **SSH (port 22)** only if the user provides a CIDR in
        `allowed_ssh_cidr`.
-   The root volume is configured with:
    -   **8 GB gp3 EBS volume**
    -   **Encryption enabled** for security.

## 2. Nginx Installation

-   Executes `yum update -y` to ensure all system packages are updated.

-   Enables the **nginx1** package using `amazon-linux-extras`.

-   Installs Nginx using `yum -y install nginx`.

-   Replaces the default Nginx landing page by writing your **resume
    HTML** (from the `resume_html` variable) into:

        /usr/share/nginx/html/index.html

-   Sets appropriate file ownership and permissions:

    -   Nginx user (`nginx:nginx`) is assigned ownership.
    -   Directory permissions are tightened using `chmod`.

## 3. Server Hardening Steps

-   **File System Hardening**
    -   HTML directory protected using restrictive permissions
        (`chmod 750`).
    -   Ensures files are not publicly writable and are readable only by
        Nginx and privileged users.
-   **Nginx Hardening**
    -   Adds security headers in
        `/etc/nginx/conf.d/security_headers.conf`:
        -   `X-Content-Type-Options nosniff`
        -   `X-Frame-Options SAMEORIGIN`
        -   `server_tokens off`
-   **Service Hardening**
    -   Nginx is started and enabled at boot using:

            systemctl enable --now nginx
-   **Restricted SSH Access**
    -   SSH is opened **only if** the user explicitly provides a CIDR
        block.
    -   Prevents unnecessary exposure of port 22 to the internet.
