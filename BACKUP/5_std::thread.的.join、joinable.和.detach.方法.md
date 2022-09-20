# [std::thread 的 join、joinable 和 detach 方法](https://github.com/zkliuym/log/issues/5)

在 C++ 中使用一个可调用对象构造一个 std::thread 对象，即可创建一个线程。

join()
当线程结束时 join 函数返回，就是阻塞调用线程，让调用线程等待子线程执行完毕，然后再往下执行。

detach()
分离，调用线程不再等待子线程结束，执行 detach 后，子线程和调用线程失去关联，驻留在后台，由 C++ 运行时库接管。

joinable()可联结性
一个 std::thread 对象只可能处于可联结或不可联结两种状态之一。即 std::thread 对象是否与某个有效的底层线程关联。
1．可联结：当线程可运行、已经运行或处于阻塞时是可联结的。但如果某个底层线程已经执行完任务，但是没有被 join 的话，该线程依然会被认为是一个活动的执行线程，仍然处于 joinable 状态。
2．不可联结：不带参构造的std::thread对象为不可联结，因为底层线程还没创建；已经移动的std::thread对象为不可联结；已经调用join或detach的对象为不可联结状态。
3．joinable()：判断是否可以成功使用 join() 或者 detach() ，返回true 表示可以，否则返回 false。

std::thread 对象析构
std::thread 对象析构时，会先判断 joinable() ，如果可联结，则程序会直接被终止（terminate）。
因此，在创建 thread 对象以后，要在随后的某个地方显式地调用 join 或 detach 以便让 std::thread 处于不可联结状态。

参考
https://blog.csdn.net/c_base_jin/category_9289004.html