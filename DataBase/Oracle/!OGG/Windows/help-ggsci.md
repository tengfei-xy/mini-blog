```
GGSCI (WIN-QHLFMQGHCTV) 1> help

ADDhis file describes the commands that can be issued through the Oracle GoldenGate
Software Command Interface (GGSCI). This is the command interface between
users and Oracle GoldenGate functional components.


Summary of Manager Commands
Use the Manager commands to control the Manager process. Manager is the parent
process of Oracle GoldenGate and is responsible for the management of its processes
and files, resources, user interface, and the reporting of thresholds and errors.

Command                     Description
INFO MANAGER                Returns information about the Manager port and process id.
SEND MANAGER                Returns information about a running Manager process and optionally
                            child processes.
START MANAGER               Starts the Manager process.
STATUS MANAGER              Returns the state of the Manager port and process ID.
STOP MANAGER                Stops the Manager process.


Summary of Extract Commands
Use the Extract commands to create and manage Extract groups. The Extract process
captures either full data records or transactional data changes, depending on configuration
parameters, and then sends the data to a trail for further processing by a downstream
process, such as a data-pump Extract or the Replicat process.

Command                     Description
ADD EXTRACT                 Creates an Extract group.
ALTER EXTRACT               Changes attributes of an Extract group
CLEANUP EXTRACT             Deletes run history for an Extract group
DELETE EXTRACT              Deletes an Extract group.
INFO EXTRACT                Returns information about an Extract group.
KILL EXTRACT                Forcibly terminates the run of an Extract group.
LAG EXTRACT                 Returns information about Extract lag.
REGISTER EXTRACT            Registers an Extract group with an Oracle database.
SEND EXTRACT                Sends instructions to, or returns information about, a running
                            Extract group.
START EXTRACT               Starts an Extract group.
STATS EXTRACT               Returns processing statistics for an Extract group.
STATUS EXTRACT              Returns the state of an Extract group.
STOP EXTRACT                Stops an Extract group.
UNREGISTER EXTRACT          Unregisters an Extract group from an Oracle database.


Summary of Replicat Commands
Use the Replicat commands to create and manage Replicat groups. The Replicat
process reads data extracted by the Extract process and applies it to target tables
or prepares it for use by another application, such as a load application.

Command                     Description
ADD REPLICAT                Adds a Replicat group.
ALTER REPLICAT              Changes attributes of a Replicat group.
CLEANUP REPLICAT            Deletes run history of a Replicat group.
DELETE REPLICAT             Deletes a Replicat group.
INFO REPLICAT               Returns information about a Replicat group.
KILL REPLICAT               Forcibly terminates a Replicat group.
LAG REPLICAT                Returns information about Replicat lag.
REGISTER REPLICAT           Registers a Replicat group with an Oracle database.
SEND REPLICAT               Sends instructions to, or returns information about, a running
                            Replicat group.
START REPLICAT              Starts a Replicat group.
STATS REPLICAT              Returns processing statistics for a Replicat group.
STATUS REPLICAT             Returns the state of a Replicat group.
STOP REPLICAT               Stops a Replicat group.
SYNCHRONIZE REPLICAT        Returns all threads of a coordinated Replicat to a uniform
                            start point after an unclean shutdown of the Replicat process.
UNREGISTER REPLICAT         Unregisters a Replicat group from an Oracle database.


Summary of the ER Command
Use the ER command to issue standard Extract and Replicat commands to multiple
Extract and Replicat groups as a unit. See "ER" for how to use this command.

Command                     Description
INFO ER *                   Returns information about the specified wildcarded groups.
KILL ER *                   Forcibly terminates the specified wildcarded groups.
LAG ER *                    Returns lag information about the specified wildcarded groups
SEND ER *                   Sends instructions to, or returns information about, the
                            specified wildcarded groups.
START ER *                  Starts the specified wildcarded groups.
STATS ER *                  Returns processing statistics for the specified wildcarded
                            groups.
STATUS ER *                 Returns the state of the specified wildcarded groups.
STOP ER *                   Stops the specified wildcarded groups.


Summary of Wallet Commands
Use the wallet commands to manage the master-key wallet that stores Oracle GoldenGate
master encryptions keys, and to add master keys to this wallet.

Command                     Description
CREATE WALLET               Creates a wallet that stores master encryption keys.
OPEN WALLET                 Opens a master-key wallet.
PURGE WALLET                Permanently removes from a wallet the master keys that are
                            marked as deleted.
ADD MASTERKEY               Adds a master key to a master-key wallet.
INFO MASTERKEY              Returns information about master keys.
RENEW MASTERKEY             Adds a new version of a master key.
DELETE MASTERKEY            Marks a master key for deletion.
UNDELETE MASTERKEY          Changes the state of a master key from being marked as deleted
                            to marked as available.

Summary of Credential Store Commands
Use the credential store commands to manage an Oracle GoldenGate credential store and
to add credentials to the credential store.

Command                     Description
ADD CREDENTIALSTORE         Creates a credentials store (wallet) that stores encrypted
                            database user credentials.
ALTER CREDENTIALSTORE       Changes the contents of a credentials store.
INFO CREDENTIALSTORE        Returns information about a credentials store.
DELETE CREDENTIALSTORE      Deletes the wallet that serves as a credentials store.


Summary of Trail Commands
Use the trail commands to create and manage Oracle GoldenGate trails. A trail is a series
of files in which Oracle GoldenGate temporarily stores extracted data on disk until it has
been applied to the target location.

Command                     Description

ADD EXTTRAIL                Adds a local trail to the Oracle GoldenGate configuration.
ADD RMTTRAIL                Adds a remote trail to the Oracle GoldenGate configuration.
ALTER EXTTRAIL              Changes attributes of a local trail.
ALTER RMTTRAIL              Changes attributes of a remote trail.
DELETE EXTTRAIL             Removes a local trail from the Oracle GoldenGate configuration.
DELETE RMTTRAIL             Removes a remote trail from the Oracle GoldenGate configuration.
INFO EXTTRAIL               Returns information about a local trail.
INFO RMTTRAIL               Returns information about a remote trail.


Summary of Parameter Commands
Use the parameter commands to view and manage Oracle GoldenGate parameter files.
See Administering Oracle GoldenGate for more information about how to work with parameter
files.

Command                     Description
EDIT PARAMS                 Opens a parameter file for editing in the default text editor.
SET EDITOR                  Sets the default text editor program for editing parameter files.
VIEW PARAMS                 Displays the contents of a parameter file in read-only mode on-screen.
INFO PARAM                  Returns parameter definition information.


Summary of Database Commands
Use the database commands to interact with the database from GGSCI.

Command                     Description
DBLOGIN                     Logs the GGSCI session into a database so that other commands that
                            affect the database can be issued.
DUMPDDL                     Shows the data in the Oracle GoldenGate DDL history table.
ENCRYPT PASSWORD            Encrypts a database login password.
FLUSH SEQUENCE              Updates an Oracle sequence so that initial redo records are
                            available at the time that Extract starts capturing transaction
                            data after the instantiation of the replication environment.
LIST TABLES                 Lists the tables in the database with names that match the input specification.
MININGDBLOGIN               Specifies the credentials of the user that an Oracle GoldenGate process
                            uses to log into an Oracle mining database.
SET NAMECCSID               Sets the CCSID of the GGSCI session in a DB2 for i environment.


Summary of Trandata Commands
Use trandata commands to configure the appropriate database components to provide the
transaction information that Oracle GoldenGate needs to replicate source data operations.

Command                     Description
ADD SCHEMATRANDATA          Enables schema-level supplemental logging.
ADD TRANDATA                Enables table-level supplemental logging.
DELETE SCHEMATRANDATA       Disables schema-level supplemental logging.
DELETE TRANDATA             Disables table-level supplemental logging.
INFO SCHEMATRANDATA         Returns information about the state of schema-level
                            supplemental logging.
INFO TRANDATA               Returns information about the state of table-level supplemental
                            logging.
SET_INSTANTIATION_CSN       Sets whether and how table instantiation CSN filtering is used.
CLEAR_INSTANTIATION_CSN     Clears table instantiation CSN filtering.


Summary of Checkpoint Table Commands
Use the checkpoint table commands to manage the checkpoint table that is used by Oracle
GoldenGate to track the current position of Replicat in the trail.
For more information about checkpoints and using a checkpoint table,
see Administering Oracle GoldenGate.

Command                     Description
ADD CHECKPOINTTABLE         Creates a checkpoint table in a database.
CLEANUP CHECKPOINTTABLE     Removes checkpoint records that are no longer needed.
DELETE CHECKPOINTTABLE      Removes a checkpoint table from a database.
INFO CHECKPOINTTABLE        Returns information about a checkpoint table.
UPGRADE CHECKPOINTTABLE     Adds a supplemental checkpoint table when upgrading Oracle
                            GoldenGate from version 11.2.1.0.0 or earlier.


Summary of Oracle Trace Table Commands
Use the trace table commands to manage the Oracle GoldenGate trace table that is used
with bidirectional synchronization of Oracle databases. Replicat generates an operation
in the trace table at the start of each transaction. Extract ignores all transactions
that begin with an operation to the trace table. Ignoring Replicat's operations
prevents data from looping back and forth between the source and target tables.
For more information about bidirectional synchronization, see Administering
Oracle GoldenGate for Windows and UNIX

Command                     Description
ADD TRACETABLE              Creates a trace table.
DELETE TRACETABLE           Removes a trace table.
INFO TRACETABLE             Returns information about a trace table.


Summary of Oracle GoldenGate Monitor JAgent Commands
Use the JAgent commands to control the Oracle GoldenGate Monitor JAgent.

Command                     Description
INFO JAGENT                 Returns information about the JAgent.
START JAGENT                Starts the JAgent.
STATUS JAGENT               Returns the state of the JAgent.
STOP JAGENT                 Stops the JAgent.


Summary of  PMSRVR COMMANDS

Use the PMSRVR commands to control the Performance Metrics Server process. The
Performance Metrics Server uses the metrics service to collect and store
instance deployment performance results.

Command                     Description
INFO PMSRVR                 Returns information about the PMSRVR.
START PMSRVR                Starts the PMSRVR.
STATUS PMSRVR               Returns the state of the PMSRVR.
STOP PMSRVR                 Stops the PMSRVR.


Summary of Oracle GoldenGate Automatic Heartbeat Commands
Use the heartbeat table commands to control the Oracle GoldenGate automatic
heartbeat functionality.

Command                     Description
ADD HEARTBEATTABLE          Creates the objects required for automatic heartbeat functionality.
ALTER HEARTBEATTABLE        Alters existing heartbeat objects.
DELETE HEARTBEATTABLE       Deletes existing heartbeat objects.
DELETE HEARTBEATENTRY       Deletes entries in the heartbeat table.
INFO HEARTBEATTABLE         Displays heartbeat table information.
UPGRADE HEARTBEATTABLE      Upgrade the heartbeat table.


Summary of Procedural Replication Commands

Use the following commands to enable, delete or retrieve information about procedures
that have supplemental logging turned on.

Command                     Description
ADD PROCEDURETRANDATA       Adding supplemental logging for Procedural Replication.
DELETE PROCEDURETRANDATA    Remove supplemental logging for Procedural Replication.
INFO PROCEDURETRANDATA      Display display supplemental logging information about Procedural Replication.


Summary of Miscellaneous Oracle GoldenGate Commands
Use the following commands to control various other aspects of Oracle GoldenGate.
Command                     Description

!                           Executes a previous GGSCI command without modifications.
ALLOWNESTED | NOALLOWNESTED Enables or disables the use of nested OBEY files.
CREATE SUBDIRS              Creates the default directories within the Oracle GoldenGate home directory.
DEFAULTJOURNAL              Sets a default journal for multiple tables or files for the ADD
                            TRANDATA command when used for a DB2 for i database.
FC                          Allows the modification and re-execution of a previously issued
                            Provides assistance with syntax and usage of GGSCI commands.
HISTORY                     Shows a list of the most recently issued commands since the
                            startup of the GGSCI session.
INFO ALL                    Displays status and lag for all Oracle GoldenGate processes on
                            a system.
OBEY                        Processes a file that contains a list of Oracle GoldenGate commands.
SHELL                       Executes shell commands from within the GGSCI interface.
SHOW                        Displays the attributes of the Oracle GoldenGate environment.
VERSIONS                    Displays information about the operating system and database.
VIEW GGSEVT                 Displays the Oracle GoldenGate error log (ggserr.logfile).
VIEW REPORT                 Displays the process report or the discard file that is generated
                            by Extract or Replicat.


For help on a specific command, type HELP <command> <object>.

Example: HELP ADD REPLICAT
```

