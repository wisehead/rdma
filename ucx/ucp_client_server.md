#1.main

```cpp
main
--parse_cmd
--init_context
----ucp_params.field_mask = UCP_PARAM_FIELD_FEATURES;
----ucp_init
------ucp_init_version
--------ucp_get_version
--------if (config == NULL)
----------ucp_config_read
------------ucs_config_parser_fill_opts
------------ucs_list_head_init(&config->cached_key_list);
--------context = ucs_calloc
--------ucs_list_head_init(&context->cached_key_list);
--------ucp_fill_config//todo
--------ucp_fill_resources//todo
--------ucp_context_create_vfs
----init_worker
--if (server_addr == NULL)
----run_server
--else
----run_client
```

#2. run_server

```cpp
run_server
--init_worker
--if (send_recv_type == CLIENT_SERVER_SEND_RECV_AM)
----ucp_worker_set_am_recv_handler
------ucp_worker_set_am_handler_common
--------ucs_array_reserve(ucp_am_cbs, &worker->am.cbs, id + 1)
--------for (i = ucs_array_length(&worker->am.cbs); i < capacity; ++i)
----------ucp_worker_am_init_handler
------ucp_worker_am_init_handler
--start_server
----set_sock_addr(address_str, &listen_addr);
----params.sockaddr.addr      = (const struct sockaddr*)&listen_addr;
----params.conn_handler.cb    = server_conn_handle_cb;
----params.conn_handler.arg   = context;
----ucp_listener_create(ucp_worker, &params, listener_p);
----ucp_listener_query
--while (1)
----while (context.conn_request == NULL)
------ucp_worker_progress(ucp_worker);
--------uct_worker_progress(worker->uct);
----------ucs_callbackq_dispatch(&worker->progress_q)
------------for (elem = cbq->fast_elems; (cb = elem->cb) != NULL; ++elem)
--------------count += cb(elem->arg);
--------ucs_async_check_miss(&worker->async);
----server_create_ep
----client_server_do_work
------for (i = 0; i < num_iterations; i++)
--------client_server_communication
----------case CLIENT_SERVER_SEND_RECV_STREAM:
------------send_recv_stream
----------case CLIENT_SERVER_SEND_RECV_TAG:
------------send_recv_tag
----------case CLIENT_SERVER_SEND_RECV_AM:
------------send_recv_am
----ep_close
----context.conn_request = NULL;
--//end of while(1)
```

#3.run_client

```cpp
run_client
--start_client
----set_sock_addr(address_str, &connect_addr);
----ep_params.sockaddr.addr    = (struct sockaddr*)&connect_addr;
----ucp_ep_create
--client_server_do_work
----client_server_communication
--ep_close

```
