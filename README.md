# Distributed Directory Management System

## Socket Programming in java

You will write a distributed directory management system consisting of a server and three client processes. Each client process will connect to the server over a socket connection and register a username at the server. The server should be able to handle all three clients simultaneously and display the names of the connected clients in real time.
Two or more clients may not use the same username simultaneously. Should the server detect a concurrent conflict in username, the client’s connection should be rejected, and the client’s user should be prompted to input a different name.
Upon connection to the server, the server will check its local disk for a directory matching the client’s username. If a directory matching the client’s username does not exist, the server will create one. If a directory matching the client’s username already exists, the server will utilize the existing directory. This directory will be designated the client’s ‘home directory’.
Inside the home directory, the client will have the ability to:
• Create directories;
• Delete directories;
• Move directories;
• Rename directories; and,
• List the contents of directories.
Each client will be explicitly confined to its own home directory – no client should be able to navigate to, list the contents of, or modify the contents of any parent directory. The user may input instructions to the client as conventional text commands, but these commands must be accepted via the GUI, not the command line.
All three clients should have the ability to make changes and navigate through their respective home directories in parallel. All operations performed by the server should be displayed on the server’s GUI. Any directory operation errors generated by the server’s host operating system must be conveyed to the client and displayed to the user. Directory operation errors should not result in the client disconnecting from the server

## Some Addition

Each client process will be given an identifier of A, B, or C. This client identifier is separate from the username the client provides to the server. Each client will be assigned a local directory (LD) corresponding to its identifier: e.g., client A will always use LD ‘A’, client B will always use LD ‘B’, client C will always use LD ‘C’. How the process is mapped to the client identifier is left to the developer’s discretion (i.e., the mapping could be assigned statically in the source code, the user could be presented with the option of selecting A, B, or C on startup, et cetera), but only one process may use an identifier at a time.
Users will be given the option of locally synchronizing any home directory available at the server. Upon connection to the server, users will be presented with a list of available home directories located on the server. A user may select one or more home directories of which to create a local copy. The local copy will be maintained in the client’s LD and the user’s selection of directories to synchronize, as well as the contents of the LD itself, will be persistent between sessions.

Once a copy of the remote directory has been created locally, any changes to the server-based directory should be reflected in the local copy. Any update to the local copy will only be initiated at the server: a client will not make modifications to its local copy directly.
Any synchronized directory may also be desynchronized. Upon desynchronization, the copy of the directory should be removed from the local system. It is the developer’s discretion as to how a user should specify a local copy to be desynchronized, but this option must be accessible via the client’s GUI at least once per process instance.

## Few more additions

The server will maintain a log of all directory operations. This log will be persistent and presented on the server’s GUI. The user will be provided with the option to ‘Undo’ any logged operation at the server. When a user chooses to ‘Undo’ an operation, that operation should be rolled back across every applicable directory in the distributed system (e.g., across the server and all clients).
Operations which are selected for ‘Undo’ should be removed from the log once all applicable systems reach a consistent state. Any sequence-dependent operations must also be rolled back and removed from the log. For example, assume the following operations:
• Operation 1: Create parent directory;
• Operation 2: Create child directory.
A user choosing to ‘Undo’ Operation 1 would necessarily ‘Undo’ Operation 2. Both the parent directory and child directory should be removed from all applicable systems and both Operation 1 and Operation 2 should be removed from the log.
The developer should not assume sequence-dependent operations will be contiguous. Only one operation may be selected for ‘Undo’ at a time.


The functionality for the clients and the server is summarized as follows.<br>
<b>Client
Startup</b>
1. Prompt the user to input a username.
2. Screen the username to ensure it does not contain illegal characters. How to address illegal characters is left to the developer’s discretion.
3. Connect to the server over a socket and register the username.
a. When the client is connected, the user should be notified of the active connection.
b. If the provided username is already in use, the client should disconnect and prompt the user to input another username.
4. Display a list of the available server directories and allow the user to select one or more server-based home directories to synchronize to the local system.<br>
<b>Synchronization</b>
1. The client will automatically monitor the server for changes to the server-based home directory.
2. Upon detecting changes to the server-based directory, the client will update the copy of the home directory in its LD to match the server-based home directory.
3. Return to Step 1 of ‘Synchronization’ until manually terminated by the user.<br>
<b>Directory Commands</b>
1. Proceed to accept directory commands from the user via the GUI.
2. Display the output of those commands to the user via the GUI.
3. Return to Step 1 of ‘Directory Commands’ until manually terminated by the user.<br>
<b>Server</b>
The server should support three concurrently connected clients. The server should indicate which of those clients are presently connected on its GUI.
<b>Startup</b>


1. Listen for incoming connections.
2. Print that a client has connected, and:
a. If the client’s username is available (e.g., not currently being used by another client), fork a thread to handle that client; or,
b. If the username is in use, reject the connection from that client.
3. Provide a home directory to the client (e.g., create a directory if necessary or use an extant directory).
4. Proceed to accept and execute directory commands received from the client.
5. Return to Step 1 of ‘Startup’ until manually killed by the user.<br>
<b>Logging</b>
1. The server will log all directory commands executed on behalf of clients.
2. Logged commands will be presented to users. Users will have the ability to select any logged command and choose ‘Undo’.
3. The effects of the command chosen for ‘Undo’, as well as the effects of any sequence-dependent operations, will be rolled back across all applicable systems.
4. The command chosen for ‘Undo’, as well as any sequence-dependent commands, will be removed from the log.
5. Return to Step 1 of ‘Logging’ until manually killed by the user.<br>
<b>Notes:</b>
• All three clients and the server may run on the same machine.
• The server must correctly handle an unexpected client disconnection without crashing.
• When a client disconnects from the server, the server GUI must indicate this to the user in real time.
• The program must operate independently of a browser.
