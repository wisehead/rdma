#1.struct UcxRequest

```cpp
struct UcxRequest {
    UcxConn      *_conn;
    UcxBufHandle  _bh;
    bool          _iov_dtype;
    bool          _recv_completed;
    int           _recv_len;
    Msg          *_msg;                  // only rsp need set
    list_head     _inflight_node;        // link in inflight req list in UcxConn
};
```