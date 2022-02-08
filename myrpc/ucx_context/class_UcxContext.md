#1.class UcxContext

```cpp
class UcxContext {
public:
    UcxContext() : _inited(false), _cid(-1), _context(), _worker() {}
    virtual ~UcxContext() {}
          ut
    int init(container_id_t cid, bool loop);
       }
    void loop();
       /
    static void request_init(void *request);

    ucp_worker_h &get_worker() {
        return _worker;
    }
private:
    bool            _inited;
    container_id_t  _cid;
    ucp_context_h   _context;
    ucp_worker_h    _worker;
};
```