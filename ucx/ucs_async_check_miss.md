#1.ucs_async_check_miss

```cpp
ucs_async_check_miss(&worker->async);
--if (!ucs_mpmc_queue_is_empty(&async->missed))
----__ucs_async_poll_missed
------while (!ucs_mpmc_queue_is_empty(&async->missed))
--------ucs_mpmc_queue_pull
--------ucs_async_method_call_all
----------ucs_async_signal_ops._fun
------------ucs_async_signal_block_all
--------------ucs_async_signal_allow(0)
----------------sigaddset(&sig_set, ucs_global_opts.async_signo);
----------------pthread_sigmask(allow ? SIG_UNBLOCK : SIG_BLOCK, &sig_set, NULL);
--else if (ucs_unlikely(async->mode == UCS_ASYNC_MODE_POLL))
----ucs_async_poll
```