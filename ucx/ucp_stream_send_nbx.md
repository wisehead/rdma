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
```