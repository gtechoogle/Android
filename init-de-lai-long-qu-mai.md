# Init 来龙去脉

Init 就是 initial 的简写，意味着初始，那么这个初始进程是从哪里开启的呢？

想要搞清楚这里，就又要回到 Linux 内核 \(Kernel\) 当中，谁让 init 进程是内核的第一个用户空间 \(User Space\) 的进程呢。

简单来讲，Linux 内核为了安全等因素，会将系统的进程分成两大类，内核进程 \(Kernel Space\) 和用户空间进程，这样做的目的就是为了不让用户空间进程去直接操作系统资源，而对于内核来说，用户空间的进程都是不可信的。所以当 Linux 内核把内核空间进程都初始话后，就要开始初始化第一个用户空间进程，而这个进程就是 Init 进程，那么是在哪里初始的呢? 

以 Kernel 4.14 代码为例，整个内核的入口点是在 kernel-4.14/init/main.c 中

{% code-tabs %}
{% code-tabs-item title="kernel-4.14/init/main.c" %}
```text
static int __ref kernel_init(void *unused)
{
	int ret;

	kernel_init_freeable();
	/* need to finish all async __init code before freeing the memory */
	async_synchronize_full();
	ftrace_free_init_mem();
	free_initmem();
	mark_readonly();
	system_state = SYSTEM_RUNNING;
	numa_default_policy();

	rcu_end_inkernel_boot();
#ifdef CONFIG_MTPROF
		log_boot("Kernel_init_done");
#endif
	if (ramdisk_execute_command) {
		ret = run_init_process(ramdisk_execute_command);
		if (!ret)
			return 0;
		pr_err("Failed to execute %s (error %d)\n",
		       ramdisk_execute_command, ret);
	}

	/*
	 * We try each of these until one succeeds.
	 *
	 * The Bourne shell can be used instead of init if we are
	 * trying to recover a really broken machine.
	 */
	if (execute_command) {
		ret = run_init_process(execute_command);
		if (!ret)
			return 0;
		panic("Requested init %s failed (error %d).",
		      execute_command, ret);
	}
	if (!try_to_run_init_process("/sbin/init") ||
	    !try_to_run_init_process("/etc/init") ||
	    !try_to_run_init_process("/bin/init") ||
	    !try_to_run_init_process("/bin/sh"))
		return 0;

	panic("No working init found.  Try passing init= option to kernel. "
	      "See Linux Documentation/admin-guide/init.rst for guidance.");
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

