#1.ucp_stream_send_nbx

```cpp
UCS_PROFILE_FUNC(ucs_status_ptr_t, ucp_stream_send_nbx,
                 (ep, buffer, count, param),
                 ucp_ep_h ep, const void *buffer, size_t count,
                 const ucp_request_param_t *param)
                 
ucp_stream_send_nbx
--ucp_request_param_flags
--ucp_ep_resolve_remote_id                 
----ucp_wireup_connect_remote
------if (ucp_proxy_ep_test(ep->uct_eps[lane]))
--------uct_ep = ucp_proxy_ep_extract(ep->uct_eps[lane]);
--------uct_ep_destroy(ep->uct_eps[lane]);
------else
--------uct_ep = ep->uct_eps[lane];
------ucp_wireup_ep_create
--------UCS_CLASS_DEFINE_NAMED_NEW_FUNC(ucp_wireup_ep_create, ucp_wireup_ep_t, uct_ep_t,ucp_ep_h);
------ucs_queue_head_init(&tmp_q)
------uct_ep_pending_purge
--------ep->iface->ops.ep_pending_purge
------ucp_wireup_ep_set_next_ep(ep->uct_eps[lane], uct_ep)
------if (!(ep->flags & UCP_EP_FLAG_CONNECT_REQ_QUEUED))
--------ucp_wireup_send_request
----------ucp_wireup_msg_send
------------ucp_wireup_msg_prepare
------------ucp_request_send
```