#1.send_recv_stream

```cpp
send_recv_stream
--fill_request_param
----fill_buffer
------generate_test_string
----*msg        = (iov_cnt == 1) ? iov[0].buffer : iov;
----*msg_length = (iov_cnt == 1) ? iov[0].length : iov_cnt;
----param->datatype     = (iov_cnt == 1) ? ucp_dt_make_contig(1) :UCP_DATATYPE_IOV;
----param->user_data    = ctx;
--if (!is_server)
----param.cb.send = send_cb;
----ucp_stream_send_nbx
--else
----param.op_attr_mask  |= UCP_OP_ATTR_FIELD_FLAGS;
----param.flags          = UCP_STREAM_RECV_FLAG_WAITALL;
----param.cb.recv_stream = stream_recv_cb;
----request              = ucp_stream_recv_nbx(ep, msg, msg_length,&msg_length, &param);
--request_finalize
```