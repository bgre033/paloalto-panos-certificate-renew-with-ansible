# Automated Palo Alto PAN-OS Certificate Renewal with Ansible

* PAN-OS Version 10.1.9-h1

Rather than manually replacing certificates in PAN-OS I used this playbook to automate the process. This is useful when ustilising Let's Encrypt certificates which are only valid for 90 days.

The playbook performs the following.

1. Get the name of an existing certificate to delete.

   This isn't strictly required but I didn't want to leave a plethora of expired certificates on my firewall. To find the certificate to delete, I query an existing SSL TLS Service Profile ('gp-ssl-profile') which is using the certificate.

2. Extract certificate name.

   From the output in step 1, I use regex to extract just the certificate name.

4. Import the certificate.

   The private key and certificate have to be in the same file for this to work (you can use 'cat key.key cert.pem > combined.pem' to combine the certificate and the key in one file).

   **NOTE**: PanOS requires that you specify a password for the private key, even if the private key is not encrypted with a password.

5. Update decryption rule.

   Update the decryption rule with the new certificate.

6. Update SSL TLS Profile.

   Update SSL TLS Service Profile with the new certificate.

7. Delete old certificate.

8. Commit configuration.
