# Reactions tests file access detection recipes

Each reaction receives a `data` object describing the detection event.
The `data` object fields depend on the event type.

Event types (`data.event_type`):

- file_access
- execution
- network_peer
- ... (more may be added)

Top-level fields in `data`:

- `uuid`: string, unique id of the detection event
- `timestamp`: string, when the detection event occurred
- `note`: string, optional note or annotation
- `metadata`: object, detection event metadata
  - `kind`: string, class name of the detection event
  - `name`: string, name of the detection recipe
  - `format`: string, format of the detection event
  - `version`: string, version of the detection event format
  - `description`: string, description of the detection event
  - `importance`: string, importance level
  - `documentation`: string, reference documentation
  - `tactic`: string, mitre tactic
  - `technique`: string, mitre technique
  - `subtechnique`: string, mitre subtechnique
- `base`: object, base event fields
  - `uuid`: string, unique id (same as above)
  - `timestamp`: string, timestamp (same as above)
  - `note`: string, optional note (same as above)
  - `metadata`: object, metadata (see above)
  - `attenuator`: object, optional, event-specific attenuation info
  - `background`: object, contextual background info
  - `habitat`: object, system/environment info
  - `scenario`: list, scenarios relevant to the event

Additional fields by event type:

For `file_access` events:

- `file`: object (`data.file`)
  - `file`: string, absolute path to the file
  - `dir`: string, directory containing the file
  - `basename`: string, base name of the file
  - `actions`: list, actions performed (e.g. "read", "write", "unlink")
  - `fasync`: bool, async file operations.
  - `flock`: bool, file locking.
  - `fsync`: bool, file synchronization.
  - `llseek`: bool, seeking within the file.
  - `mmap`: bool, memory-mapped file access.
  - `open`: bool, file opening.
  - `read`: bool, file reading.
  - `write`: bool, file writing.
  - `rename`: bool, file renaming.
  - `truncate`: bool, file truncation.
  - `unlink`: bool, file deletion.
  - `create`: bool, file creation.
  - `close`: bool, file closing.
  - `link`: bool, file linking.
  - `execve`: bool, execution via execve.

For execution events:

- `process`: object (`data.process`)
  - `start`: string, start time of the process
  - `exit`: string, exit time of the process
  - `retcode`: int, return code
  - `uid`: int, user id
  - `pid`: int, process id
  - `ppid`: int, parent process id
  - `comm`: string, command name
  - `cmd`: string, full command line
  - `exe`: string, executable name
  - `args`: list, arguments passed
  - `loader`: string, loader used
  - `prev_exe`: string, previous executable name
  - `prev_args`: list, previous arguments
  - `prev_loader`: string, previous loader name

For network_peer events:

- `flow`: object (`data.flow`)
  - `ip_version`: int, ip version (4 or 6)
  - `proto`: string, protocol (e.g. tcp, udp)
  - `icmp`: object, icmp info (type, code)
  - `local`: object
    - `address`: string, local address
    - `name`: string, local node name
    - `names`: list, local node names
    - `port`: int, local port
  - `remote`: object
    - `address`: string, remote address
    - `name`: string, remote node name
    - `names`: list, remote node names
  - `service_port`: int, service port number
  - `flags`: object
    - `ingress`: bool, flow is incoming
    - `egress`: bool, flow is outgoing
    - `incoming`: bool, flow is incoming
    - `outgoing`: bool, flow is outgoing
    - `started`: bool, flow has started
    - `ongoing`: bool, flow is ongoing
    - `ended`: bool, flow has ended
    - `terminator`: bool, flow is a terminator
    - `terminated`: bool, flow has been terminated
  - `phase`: object
    - `direction`: string, direction of the flow
    - `initiated_by`: string, who initiated
    - `status`: string, current status
    - `ended_by`: string, who ended the flow

Additional context fields:

- `base.background.files`: object, aggregate info about the file tree
- `base.background.flows`: object, aggregate info about network flows
- `base.background.ancestry`: list, process ancestry (parent/ancestors)
