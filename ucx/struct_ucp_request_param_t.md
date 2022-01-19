#1.struct ucp_request_param_t

```cpp
/**
 * @ingroup UCP_CONTEXT
 * @brief Operation parameters passed to @ref ucp_tag_send_nbx,
 *        @ref ucp_tag_send_sync_nbx, @ref ucp_tag_recv_nbx, @ref ucp_put_nbx,
 *        @ref ucp_get_nbx, @ref ucp_am_send_nbx and @ref ucp_am_recv_data_nbx.
 *
 * The structure @ref ucp_request_param_t is used to specify datatype of
 * operation, provide user request in case the external request is used,
 * set completion callback and custom user data passed to this callback.
 *
 * Example: implementation of function to send contiguous buffer to ep and
 *          invoke callback function at operation completion. If the
 *          operation completed immediately (status == UCS_OK) then
 *          callback is not called.
 *
 * @code{.c}
 * ucs_status_ptr_t send_data(ucp_ep_h ep, void *buffer, size_t length,
 *                            ucp_tag_t tag, void *request)
 * {
 *     ucp_request_param_t param = {
 *         .op_attr_mask               = UCP_OP_ATTR_FIELD_CALLBACK |
 *                                       UCP_OP_ATTR_FIELD_REQUEST,
 *         .request                    = request,
 *         .cb.send                    = custom_send_callback_f,
 *         .user_data                  = pointer_to_user_context_passed_to_cb
 *     };
 *
 *     ucs_status_ptr_t status;
 *
 *     status = ucp_tag_send_nbx(ep, buffer, length, tag, &param);
 *     if (UCS_PTR_IS_ERR(status)) {
 *         handle_error(status);
 *     } else if (status == UCS_OK) {
 *         // operation is completed
 *     }
 *
 *     return status;
 * }
 * @endcode
 */
typedef struct {
    /**
     * Mask of valid fields in this structure and operation flags, using
     * bits from @ref ucp_op_attr_t. Fields not specified in this mask will be
     * ignored. Provides ABI compatibility with respect to adding new fields.
     */
    uint32_t       op_attr_mask;

    /* Operation specific flags. */
    uint32_t       flags;

    /**
     * Request handle allocated by the user. There should
     * be at least UCP request size bytes of available
     * space before the @a request. The size of the UCP request
     * can be obtained by @ref ucp_context_query function.
     */
    void          *request;

    /**
     * Callback function that is invoked whenever the
     * send or receive operation is completed.
     */
    union {
        ucp_send_nbx_callback_t         send;
        ucp_tag_recv_nbx_callback_t     recv;
        ucp_stream_recv_nbx_callback_t  recv_stream;
        ucp_am_recv_data_nbx_callback_t recv_am;
    }              cb;

    /**
     * Datatype descriptor for the elements in the buffer. In case the
     * op_attr_mask & UCP_OP_ATTR_FIELD_DATATYPE bit is not set, then use
     * default datatype ucp_dt_make_contig(1)
     */
    ucp_datatype_t datatype;

    /**
     * Pointer to user data passed to callback function.
     */
    void          *user_data;

    /**
     * Reply buffer. Can be used for storing operation result, for example by
     * @ref ucp_atomic_op_nbx.
     */
    void          *reply_buffer;

    /**
     * Memory type of the buffer. see @ref ucs_memory_type_t for possible memory types.
     * An optimization hint to avoid memory type detection for request buffer.
     * If this value is not set (along with its corresponding bit in the op_attr_mask -
     * @ref UCP_OP_ATTR_FIELD_MEMORY_TYPE), then use default @ref UCS_MEMORY_TYPE_UNKNOWN
     * which means the memory type will be detected internally.
     */
    ucs_memory_type_t memory_type;

    /**
     * Pointer to the information where received data details are stored
     * in case of an immediate completion of receive operation. The user has to
     * provide a pointer to valid memory/variable which will be updated on function
     * return.
     */
    union {
        size_t              *length;   /* Length of received message in bytes.
                                          Relevant for non-tagged receive
                                          operations. */
        ucp_tag_recv_info_t *tag_info; /* Information about received message.
                                          Relevant for @a ucp_tag_recv_nbx
                                          function. */
    } recv_info;

    /**
     * Memory handle for pre-registered buffer.
     * If the handle is provided, protocols that require registered memory can
     * skip the registration step. As a result, the communication request
     * overhead can be reduced and the request can be completed faster.
     * The memory handle should be obtained by calling @ref ucp_mem_map.
     */
    ucp_mem_h memh;

} ucp_request_param_t;

    
```