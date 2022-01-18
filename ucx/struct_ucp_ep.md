#1.struct ucp_ep

```cpp
/**
 * Protocol layer endpoint, represents a connection to a remote worker
 */
typedef struct ucp_ep {
    ucp_worker_h                  worker;        /* Worker this endpoint belongs to */

    uint8_t                       refcount;      /* Reference counter: 0 - it is
                                                    allowed to destroy EP */
    ucp_worker_cfg_index_t        cfg_index;     /* Configuration index */
    ucp_ep_match_conn_sn_t        conn_sn;       /* Sequence number for remote connection */
    ucp_lane_index_t              am_lane;       /* Cached value */
    ucp_ep_flags_t                flags;         /* Endpoint flags */

    /* TODO allocate ep dynamically according to number of lanes */
    uct_ep_h                      uct_eps[UCP_MAX_LANES]; /* Transports for every lane */

#if ENABLE_DEBUG_DATA
    char                          peer_name[UCP_WORKER_ADDRESS_NAME_MAX];
    /* Endpoint name for tracing and analysis */
    char                          name[UCP_ENTITY_NAME_MAX];
#endif

#if UCS_ENABLE_ASSERT
    struct {
        /* How many times the EP create was done */
        unsigned                      create;
        /* How many Worker flush operations are in-progress where the EP is the
         * next EP for flushing */
        unsigned                      flush;
        /* How many UCT EP discarding operations are in-progress scheduled for
         * the EP */
        unsigned                      discard;
    } refcounts;
#endif

    UCS_STATS_NODE_DECLARE(stats)

} ucp_ep_t;
```

#2. uct_ep

```cpp
/**
 * Remote endpoint
 */
typedef struct uct_ep {
    uct_iface_h              iface;
} uct_ep_t;
```

#3. uct_iface_h

```cpp
/**
 * Communication interface context
 */
typedef struct uct_iface {
    uct_iface_ops_t          ops;
} uct_iface_t;
```
