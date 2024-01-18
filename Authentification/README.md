# Authentication with OpenLDAP, SSH, Apache, OpenVPN

## Section 1: OpenLDAP Configuration

### 1.1 Configure an OpenLDAP server with at least two users and two groups.

1. Installation of Dependencies:
    ![Installation of Dependencies](./screenshots/section1/Capture.PNG)
    ![Installation of Dependencies](./screenshots/section1/Capture1.PNG)

2. Adding an Admin Password:

    ![Adding an Admin Password](./screenshots/section1/Capture2.PNG)

3. Installation of LAM: 

    LDAP ACCOUNT MANAGER: Graphical Interface for Server Management;

    All configurations have been done using LAM.
    ![Installation of LAM](./screenshots/section1/Capture3.PNG)

    Enable PHP CGI PHP Extension: 
    ![Enable PHP CGI PHP Extension](./screenshots/section1/enable_php_cgi%20php%20extension.PNG)

4. Starting the Configuration:
   
    1. Access to `192.168.56.101.lam`; access to the LAM interface.
    2. Begin the configuration with LAM Configuration.
    3. Change the base DC to `dc=mon-projet, dc=tn`.
    4. Change the manager's name, considering the new configuration (`cn=admin,dc=mon-projet,dc=tn`).
    5. Change the default password (lam).
    6. Change the suffix used by Tree to visualize the DIT (Directory Information Tree).

    ![LAM Configuration](./screenshots/section1/Screenshot%20(449).png)
    ![LAM Configuration](./screenshots/section1/Screenshot%20(455).png)

    7. Log in as Admin and Add Users and Groups, Considering the New Configurations:
   
    ![Add Users and Groups](./screenshots/section1/Screenshot%20(457).png)
    ![Add Users and Groups](./screenshots/section1/Screenshot%20(460).png)

### 1.2 Add information of your choice, including the x509 certificate, for all users.

1. Create Certificates for Both Users and Link Them, Also Using LAM:
![Create Certificates](./screenshots/section1/Screenshot%20(465).png)
![Create Certificates](./screenshots/section1/Screenshot%20(467).png)
![Create Certificates](./screenshots/section1/Screenshot%20(469).png)
![Create Certificates](./screenshots/section1/Screenshot%20(472).png)

1. Viewing the DIT (Directory Information Tree):

![Viewing the DIT](./screenshots/section1/Screenshot%20(537).png)

### 1.3 Ensure that users can authenticate successfully on the OpenLDAP server.

#### Client Machine Configuration:

1. First, check the status of the server: 
   ![Check the status of the server](./screenshots/section1/1.3/Screenshot%20(473).png)
2. Retrieve its IP address: 
   ![Retrieve its IP address](./screenshots/section1/1.3/Screenshot%20(475).png)
3. Add the server's IP address to the client machine to avoid errors in /etc/hosts: 
   ![Add the server's IP address to the client machine](./screenshots/section1/1.3/Screenshot%20(476).png)
4. Verify the client's connectivity to the server through a ping: 
   ![Verify the client's connectivity to the server through a ping](./screenshots/section1/1.3/Screenshot%20(477).png)
5. Install client dependencies: libnss-ldap, libpam-ldap, ldap-utils, nscd
6. During the installation, a configuration window appears: 
   ![During the installation, a configuration window appears](./screenshots/section1/1.3/Screenshot%20(479).png)
7. Add the server configurations, including the address, admin password, distinguished name (DN), version, etc.
8. Configure the file /etc/nsswitch.conf as follows: 
   ![Configure the file /etc/nsswitch.conf as follows](./screenshots/section1/1.3/Screenshot%20(480).png)
9.  Configure the file /etc/pam.d/common-password as follows:
    ![Configure the file /etc/pam.d/common-password as follows](./screenshots/section1/1.3/Screenshot%20(482).png)
10. Configure the file /etc/pam.d/common-session as follows: 
    ![Configure the file /etc/pam.d/common-session as follows](./screenshots/section1/1.3/Screenshot%20(485).png)
11. Restart nscd: 
    ![Restart nscd](./screenshots/section1/1.3/Screenshot%20(486).png)
12. Testing:
    
    - Through the ldapsearch command to retrieve everything:
  ![ldapsearch command](./screenshots/section1/1.3/Screenshot%20(487).png)
    - Verify the addition of new users and groups, and try to log in: 
  ![Verify the addition of new users and groups, and try to log in](./screenshots/section1/1.3/Screenshot%20(490).png)
  ![Verify the addition of new users and groups, and try to log in](./screenshots/section1/1.3/Screenshot%20(492).png)
    ![Verify the addition of new users and groups, and try to log in](./screenshots/section1/1.3/Screenshot%20(493).png)

### 1.4 Test the secure part of LDAP with LDAPS, and describe the various advantages.

1. Create self-signed certificates for the server: 
![Create self-signed certificates for the server](./screenshots/section1/1.4/Screenshot%20(494).png)

1. Assign ownership to OpenLDAP using the chown command: 
![Assign ownership to OpenLDAP using the chown command](./screenshots/section1/1.4/Screenshot%20(495).png)

1. Create an LDIF configuration file: 
![Create an LDIF configuration file](./screenshots/section1/1.4/Screenshot%20(497).png)

1. Execute changes from the LDIF file: 
![Execute changes from the LDIF file](./screenshots/section1/1.4/Screenshot%20(498).png)

1. Add ldaps:// to the file /etc/default/slapd: 
![Add ldaps:// to the file /etc/default/slapd](./screenshots/section1/1.4/Screenshot%20(450).png)

![Add ldaps:// to the file /etc/default/slapd](./screenshots/section1/1.4/Screenshot%20(501).png)

1. Configure the file: /etc/ldap/ldap.conf: 
![Configure the file: /etc/ldap/ldap.conf](./screenshots/section1/1.4/Screenshot%20(506).png)
1. Restart slapd: 
![Restart slapd](./screenshots/section1/1.4/Screenshot%20(507).png)
1. Test an LDAPS query: 
![Test an LDAPS query](./screenshots/section1/1.4/Screenshot%20(509).png)

## Section 2: SSH Authentication

- The configurations made for the client machine in 1.3 will enable SSH connection: 
![SSH connection](./screenshots/section2/Screenshot%20(514).png)
![SSH connection](./screenshots/section2/Screenshot%20(515).png)
- Limit access only to the first group:
- Verify that members belong to the associated groups: 
![Verify that members belong to the associated groups](./screenshots/section2/Screenshot%20(521).png)

- Configure /etc/ssh/sshd_config: 
![Configure /etc/ssh/sshd_config](./screenshots/section2/Screenshot%20(522).png)

- Test:
    - SSH possible for nbennejma: 
  ![SSH possible for nbennejma](./screenshots/section2/Screenshot%20(523).png)

    - SSH impossible for wsboui: 
  ![SSH impossible for wsboui](./screenshots/section2/Screenshot%20(524).png)

## Section 3: Integration of Apache

- Install dependencies: 
  ![Install dependencies](./screenshots/section3/Screenshot%20(528).png)
  ![Install dependencies](./screenshots/section3/Screenshot%20(529).png)

- Check the Apache2 service: 
  ![Check the Apache2 service](./screenshots/section3/Screenshot%20(530).png)

- Configure the service to allow access only to users from group 3: 
  ![Configure the service to allow access only to users from group 3](./screenshots/section3/Screenshot%20(531).png)
  ![Configure the service to allow access only to users from group 3](./screenshots/section3/Screenshot%20(531.5).png)

- Restart Apache2: 
  ![Restart Apache2](./screenshots/section3/Screenshot%20(532).png)

- Test access: 
  ![Test access](./screenshots/section3/Screenshot%20(533).png)
  ![Test access](./screenshots/section3/Screenshot%20(534).png)