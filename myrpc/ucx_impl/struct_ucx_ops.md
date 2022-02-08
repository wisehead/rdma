#1.struct ucx_ops

```cpp
struct ucx_ops {
    ucs_status_t (*ucp_config_read)(const char *env_prefix, const char *filename,
                                    ucp_config_t **config_p);
    void (*ucp_config_print)(const ucp_config_t *config, FILE *stream,
                             const char *title, ucs_config_print_flags_t print_flags);
    void (*ucp_config_release)(ucp_config_t *config);
    ucs_status_t (*ucp_init_version)(unsigned api_major_version, unsigned api_minor_version,
                                     const ucp_params_t *params, const ucp_config_t *config,
                                     ucp_context_h *context_p);
    ucs_status_t (*ucp_worker_create)(ucp_context_h context,
                                      const ucp_worker_params_t *params,
                                      ucp_worker_h *worker_p);
    unsigned (*ucp_worker_progress)(ucp_worker_h worker);
    ucs_status_t (*ucp_listener_create)(ucp_worker_h worker,
                                        const ucp_listener_params_t *params,
                                        ucp_listener_h *listener_p);
    void (*ucp_listener_destroy)(ucp_listener_h listener);
    ucs_status_t (*ucp_ep_create)(ucp_worker_h worker, const ucp_ep_params_t *params,
                                  ucp_ep_h *ep_p);
    ucs_status_ptr_t (*ucp_ep_close_nb)(ucp_ep_h ep, unsigned mode);
    void (*ucp_ep_print_info)(ucp_ep_h ep, FILE *stream);
    ucs_status_ptr_t (*ucp_tag_recv_nb)(ucp_worker_h worker, void *buffer, size_t count,
                                        ucp_datatype_t datatype, ucp_tag_t tag,
                                        ucp_tag_t tag_mask, ucp_tag_recv_callback_t cb);
    ucs_status_ptr_t (*ucp_tag_send_nb)(ucp_ep_h ep, const void *buffer, size_t count,
                                        ucp_datatype_t datatype, ucp_tag_t tag,
                                        ucp_send_callback_t cb);
    ucs_status_ptr_t (*ucp_stream_send_nb)(ucp_ep_h ep, const void *buffer, size_t count,
                                           ucp_datatype_t datatype, ucp_send_callback_t cb,
                                           unsigned flags);
    ucs_status_ptr_t (*ucp_stream_recv_nb)(ucp_ep_h ep, void *buffer, size_t count,
                                           ucp_datatype_t datatype,
                                           ucp_stream_recv_callback_t cb,
                                           size_t *length, unsigned flags);
    void (*ucp_request_cancel)(ucp_worker_h worker, void *request);
    void (*ucp_request_free)(void *request);
    ucs_status_t (*ucp_request_check_status)(void *request);
    const char *(*ucs_status_string)(ucs_status_t status);
    ucs_status_t (*ucp_conn_request_query)(ucp_conn_request_h conn_request,
                                           ucp_conn_request_attr_t *attr);

};
```