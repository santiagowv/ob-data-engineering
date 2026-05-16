Secure file transfer protocol. A method companies use to <span style="color:rgb(216, 203, 251)">safely send and receive files</span>. 
- Alternatively known as <span style="color:rgb(216, 203, 251)">SSH file transfer protocol</span>.
- Utilizes port 22 by default.
- SFTP allows users to <span style="color:rgb(216, 203, 251)">resume file transfers that were previously paused</span> due to interrupted sessions.
![[Pasted image 20260516104514.png|398]]
# Client-server architecture
<span style="color:rgb(216, 203, 251)">This architecture model gives the server control</span> over connections, resourcing, and security, even though the client initiates a session.
# Set up SFTP job
## Authentication
Two common methods:
- **SSH Keys:** <span style="color:rgb(216, 203, 251)">SFTP typically relies on SSH keys for user authentication</span>. Instead of using a password, <span style="color:rgb(216, 203, 251)">we set up a public-private key pair</span>: the server holds the public key, while the client keeps the private key securely.
- **Password authentication:** username and password.
## Fyle encryption
Many <span style="color:rgb(216, 203, 251)">files transferred via SFTP are also encrypted using PGP (Pretty Good Privacy</span>. Especially when data has any form of PII, PHI.
### PGP key pair encryption
The <span style="color:rgb(216, 203, 251)">sender encrypts the file or message using the recipient's public key</span>, and the <span style="color:rgb(216, 203, 251)">recipient decrypts it with their private key</span>.
![[Pasted image 20260516110246.png|504]]
## PGP signatures
<span style="color:rgb(216, 203, 251)">Verify the authenticity and integrity</span> of a file or message through digital signatures. The file will be checked to <span style="color:rgb(216, 203, 251)">ensure we actually know who sent it</span>.
- The sender <span style="color:rgb(216, 203, 251)">signs a message or file using their private key, creating proof of origin</span>.
- The <span style="color:rgb(216, 203, 251)">recipient uses the public key to verify the signature, ensuring that the message of file hasn't been altered</span> and confirming the sender's identity.
## Schema, header, and aggregate file
- **Header:** sometimes a header will exist in the data file. The <span style="color:rgb(216, 203, 251)">header allows the receiving party to check the validity of the schema</span>.
- **Aggregate file checks:** <span style="color:rgb(216, 203, 251)">aggregate figure that describes the main file</span>, like the total number of rows, total dollars spent, or unique users.
# Common SFTP commands
## Initiating and closing an SFTP
```powershell
sftp user@hostname e.g. sftp dan@example.com
```
<span style="color:rgb(216, 203, 251)">Opens a new connection</span> on the example.com server.
- -P `[number]` to specify a port number
- -i `[file]` to include a private key file, and
- -r to switch on recursive directory transfer.
## Transferring files
- `get server_path_and_filename local_path` copies the given file from the server to the specified directory.
- `put local_path_and_filename server_path` transfers a local file to the given server directory.
## Remote file management
- `chown user path` changes the ownership of the file or folder at the given path on the server to the specified user.
- `chmod number path` changes the permissions of the file or folder at the given path on the server.
- `ls` shows the list of files and folders in the current server directory.
- `cd path` navigates to the given directory on the server.
- `mkdir dir_name` creates a new folder on the server.
- `rmdir dir_name` removes a given folder on the server.
- `rename old_file_name new_file_name` renames a given file on the server.
- `pwd` shows the current directory on the server.
- `lpwd` shows the current local directory.
# SFTP best practices
- Use <span style="color:rgb(216, 203, 251)">key rotation and secure storage</span> on the SFTP server.
- Ensure SFTP <span style="color:rgb(216, 203, 251)">server is always up to date with security updates and patches</span>.
- <span style="color:rgb(216, 203, 251)">Log succesful file transfers and failed access attempts</span> for anomaly detection and response.
- <span style="color:rgb(216, 203, 251)">SFTP is just one part of network security</span>. Firewalls, instrusion detection systems, and other security measures should be tailored to the specific network architecture.