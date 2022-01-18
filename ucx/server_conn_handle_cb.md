#1.server_conn_handle_cb

```cpp
server_conn_handle_cb
--attr.field_mask = UCP_CONN_REQUEST_ATTR_FIELD_CLIENT_ADDR;
--ucp_conn_request_query
----ucs_sockaddr_copy((struct sockaddr *)&attr->client_address,(struct sockaddr *)&conn_request->client_address);
--context->conn_request = conn_request;

```