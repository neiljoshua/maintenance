Local Apache Development Setup (macOS / Homebrew / Apple Silicon M3)
A quick guide for setting up local virtual host environments (.local) using Homebrew Apache on macOS.

1. Configure Apache Virtual Hosts (httpd-vhosts.conf)
Add a new VirtualHost block for both HTTP (Port 80) and HTTPS (Port 443) to your Homebrew configuration file:

File Path: /opt/homebrew/etc/httpd/extra/httpd-vhosts.conf

Apache
# HTTP Host
<VirtualHost *:80>
    DocumentRoot "/Users/YOUR_USERNAME/Projects/your-project-folder"
    ServerName your-project.local
    <Directory "/Users/YOUR_USERNAME/Projects/your-project-folder">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

# HTTPS Host (Using local SSL)
<VirtualHost *:443>
    DocumentRoot "/Users/YOUR_USERNAME/Projects/your-project-folder"
    ServerName your-project.local
    
    SSLEngine on
    SSLCertificateFile "/opt/homebrew/etc/httpd/localhost.crt"
    SSLCertificateKeyFile "/opt/homebrew/etc/httpd/localhost.key"

    <Directory "/Users/YOUR_USERNAME/Projects/your-project-folder">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
2. Update System Host Mappings (/etc/hosts)
Map your local domain to the loopback address so your browser resolves it correctly.

Open the hosts file in your terminal:

Bash
sudo nano /etc/hosts
Add your domain mapping:

Plaintext
127.0.0.1   your-project.local
Save and exit (Ctrl+O, Enter, Ctrl+X).

3. Test Syntax & Restart Apache
Always verify your configuration syntax before restarting the server to prevent downtime:

Bash
# Test for syntax errors
httpd -t

# Restart Homebrew Apache background service
brew services restart httpd
4. Troubleshooting Checklist
Wrong site opens / Overlapping domains: Ensure your active project's VirtualHost block is positioned correctly in httpd-vhosts.conf (the first block acts as the default fallback for port 80).

Browser Chrome DNS caching issues: If your browser stubbornly redirects or opens an old project layout, clear Chrome's internal DNS cache via chrome://net-internals/#dns or perform a hard reload (Cmd + Shift + R).