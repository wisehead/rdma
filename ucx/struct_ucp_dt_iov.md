#1.struct ucp_dt_iov

```cpp
/**
 * @ingroup UCP_DATATYPE
 * @brief Structure for scatter-gather I/O.
 *
 * This structure is used to specify a list of buffers which can be used
 * within a single data transfer function call. This list should remain valid
 * until the data transfer request is completed.
 *
 * @note If @a length is zero, the memory pointed to by @a buffer
 *       will not be accessed. Otherwise, @a buffer must point to valid memory.
 */
typedef struct ucp_dt_iov {
    void   *buffer;   /**< Pointer to a data buffer */
    size_t  length;   /**< Length of the @a buffer in bytes */
} ucp_dt_iov_t;

```