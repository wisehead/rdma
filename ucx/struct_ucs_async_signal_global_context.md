#1. ucs_async_signal_global_context

```cpp
static struct {
    struct sigaction            prev_sighandler;       /* Previous signal handler */
    int                         event_count;           /* Number of events in use */
    pthread_mutex_t             event_lock;            /* Lock for adding/removing events */
    pthread_mutex_t             timers_lock;           /* Lock for timers array */
    ucs_async_signal_timer_t    timers[UCS_SIGNAL_MAX_TIMERQS];/* Array of all threads */
} ucs_async_signal_global_context = {
    .event_count = 0,
    .event_lock  = PTHREAD_MUTEX_INITIALIZER,
    .timers_lock = PTHREAD_MUTEX_INITIALIZER,
    .timers      = {{ .tid = 0 }}
};
```

#2. ucs_async_signal_timer

```cpp
/*
 * Per-thread system timer and software timer queue. We can dispatch timers only
 * on the same thread which added them.
 */
typedef struct ucs_async_signal_timer {
    pid_t                      tid;          /* Thread ID */
    timer_t                    sys_timer_id; /* System timer ID */
    ucs_timer_queue_t          timerq;       /* Queue of timers for the thread */
} ucs_async_signal_timer_t;
```

#3.ucs_async_signal_dispatch_timer

```cpp
ucs_async_signal_dispatch_timer
--timer = &ucs_async_signal_global_context.timers[uid];
--ucs_async_dispatch_timerq(&timer->timerq, ucs_get_time());

```