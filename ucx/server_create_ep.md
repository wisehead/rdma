#1.server_create_ep

```cpp
server_create_ep
--ucp_ep_create
----UCS_ASYNC_BLOCK(&worker->async);
----if (flags & UCP_EP_PARAMS_FLAGS_CLIENT_SERVER)
------ucp_ep_create_to_sock_addr
----else if (params->field_mask & UCP_EP_PARAM_FIELD_CONN_REQUEST)
------ucp_ep_create_api_conn_request
----else if (params->field_mask & UCP_EP_PARAM_FIELD_REMOTE_ADDRESS)
------ucp_ep_create_api_to_worker_addr
----ucp_ep_params_check_err_handling
----ucp_ep_update_flags
----UCS_ASYNC_UNBLOCK
```