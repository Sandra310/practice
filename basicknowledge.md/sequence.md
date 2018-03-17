## JS执行顺序

### event-loop
* 首先判断JS是同步还是异步,同步就进入主进程,异步就进入event table
* 异步任务在event table中注册函数,当满足触发条件后,被推入event queue
* 同步任务进入主线程后一直执行,直到主线程空闲时,才会去event queue中查看是否有可执行的异步任务,如果有就推入主进程中


