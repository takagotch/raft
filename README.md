### raft
---
https://github.com/hashicorp/raft

https://github.com/takagotch/etcd/blob/master/README.md

```
go version
```

```go
const (
  DefaultTimeoutScale = 256 * 1024
)

var (
  ErrLeader = errors.New("Node is the leader")
  
  ErrNotLeader = errors.New("node is not the leader")
  
  ErrleadershipLost = errors.New("leadership lost while committing log")
  
  ErrAbortByRestore = errors.New("snapshot restored while committing log")
  
  ErrRaftShutdown = errors.New("raft is already operation")
  
  ErrNothingNewToSnapshot = errors.New("nothing new to snapshot")
  
  ErrUnsupportedProtocol = errors.New("operation not supported with current protocol version")
  
  ErrCantBootstrap = errors.New("bootstrap only works on new clusters")
)

var (
  ErrTransportShutdown = errors.New("transport shutdown")
  
  ErrPipelineShutdown = errors.New("append pipeline closed")
)

var (
  ErrLogFound = errors.New("log not found")
  
  ErrPipelineReplicationNotSupported = errors.New("pipeline replication not supported")
)

func BootstrapCluster(conf *Config, logs LogStore, stable StableStore,
  snaps SnapshotStore, trans Transport, configuration Configuration) error
  
func HasExistingState(logs LogStore, stable StableStore, snaps SnapshotStore) (bool, error)

func NewInmemTransport(addr ServerAddress) (ServerAddress, *InmemTransport)

func NewInmemTransportWithTimeout(addr ServerAddress, timeout time.Duration) (ServerAddress, *InmemTransport)

func RecoverCluster(conf *Config, fsm FSM, logs LogStore, stable StableStore,
  snaps SnapshotStore, trans Transport, configuration Configuration) error

func ValidateConfig(config *Config, fsm FSM, logs LogStore, stable StableStore,
  snaps SnapshotStore, trans Transport, configuration Configuration) error

func ValidateConfig(config *Config) error

type AppendEntrieRequest struct {
  RPCHeader
  
  Term uint64
  Leader []byte
  
  PrevLogEntry uint64
  PrevLogTerm uint64
  
  Entries []*Log
  
  LeaderCommitIndex uint64
}


func (r *AppendEntriesRequest) GetRPCHeader() RPCHeader

type AppendEntriesResponse struct {
  RPCHeader
  
  Term uint64 
  
  LastLog uint64
  
  Success bool
  
  NoRetryBackoff bool
}

func (r *AppendEntriesResponse) GetRPCHeader() RPCHeader

type AppendFuture interface {
  Future
  
  Start() time.Time
  
  Request() *AppendEntriesRequest
  
  Response() *AppendEntriesResponse
}

type AppendPipeline interface {
  AppendEntries(args *AppendEntriesRequest, resp *AppendEntriesResponse) (AppendFuture, error)
  
  Consumer() <-chan AppendFuture
  
  Close() error
}

type ApplyFuture interface {
  IndexFuture
  
  Response() interface{}
}

type Config struct {
  ProtocolVersion ProtocolVersion
  
  HeartbeatTimeout time.Duration
  
  ElectionTimeout time.Duration
  
  CommitTimeout time.Duration
  
  MaxAppendEntries int
  
  ShutdownOnRemove bool
  
  TrailingLogs uint64
  
  SnapshotInterval time.Duration
  
  SnapshotThreshold uint64
  
  LeaderLeaseTimeout time.Duration
  
  StartAsLeader bool
  
  LocalID ServerID
  
  NotifyCh chan<- bool
  
  LogOutput io.Writer
  
  Logger *log.Logger
}

func DefaultConfig() *Config

type Configuration struct {
  Servers []Server
}

func ReadConfigJSON(path string) (Configuration, error)

func ReadPeersJSON(path string) (Configuration, error)

func (c *Configuration) Close() (copy Configuration)

type ConfigurationChangeCommand uint8

const (
  AddStaging ConfigurationChangeCommand = iota
  
  AddNonvoter
  
  DemoteVoter
  
  RemoveServer
  
  Promote
)

func (c ConfigurationChangeCommand) String() string

type ConfigurationFuture interface {
  IndexFuture
  
  Configuration() Configuration
}

type DiscardSnapshotSink struct{}

func(d *DiscardSnapshotSink) Cancel() error


type WithPeers interface {
  Connect(peer ServerAddress, t Transport)
  Disconnect(peer ServerAddress)
  DisconnectAll()
}

type WithRPCHeader interface {
  GetRPCHeader() RPCHeader
}
```

```
```


