#1.ucp_proxy_ep

```cpp
/**
 * Generic proxy endpoint, used to change behavior of a specific transport lane
 * without adding data-path checks when not needed.
 * By default, all transport endpoint operations are redirected to the underlying
 * UCT endpoint, and interface operations would result in a fatal error.
 * When this endpoint is destroyed, the lane in UCP endpoint is replaced with
 * the real transport endpoint.
 *
 * TODO make sure it works with err handling and print_ucp_info
 */
typedef struct ucp_proxy_ep {
    uct_ep_t                  super;    /**< Derived from uct_ep */
    uct_iface_t               iface;    /**< Embedded stub interface */
    ucp_ep_h                  ucp_ep;   /**< Pointer to UCP endpoint */
    uct_ep_h                  uct_ep;   /**< Underlying transport endpoint */
    int                       is_owner; /**< Is uct_ep owned by this proxy ep */
} ucp_proxy_ep_t;

```