---
title: C++ concurrency
tag: cpp
date: 2019-03-26 09:36:55
---

C++ lambda [&]

thread join

std::ref in thread with argument.

## promise and future

this is very similar to channel in go. a code snippet

```
void fun(promise<string> &&pr)
{
	cout << "Worker sending\n";
	pr.set_value("Message from thread.");
}

int main()
{
	promise<string> pr;
	future<string> fut = pr.get_future();

	thread th(fun, move(pr));
	cout << "Main receiving\n";

	string str = fut.get();
	cout << str << endl;
	th.join();
	return 0;
}
```
### some import function

`wait` `wait_for` `wait_until` `future_status`

## exception e

`e.waht()`

## std::async

`future<string> fut = async(fun, parameter);` can be used to replace `promise` to get callback from a `func`.

## Consider using tasks instead of threads

## move and unique_ptr

## 自旋锁
自旋锁是计算机科学用于多线程同步的一种锁，线程反复检查锁变量是否可用。由于线程在这一过程中保持执行，因此是一种忙等待。一旦获取了自旋锁，线程会一直保持该锁，直至显式释放自旋锁。

自旋锁避免了进程上下文的调度开销，因此对于线程只会阻塞很短时间的场合是有效的。因此操作系统的实现在很多地方往往用自旋锁。Windows操作系统提供的轻型读写锁（SRW Lock）内部就用了自旋锁。显然，单核CPU不适于使用自旋锁，这里的单核CPU指的是单核单线程的CPU，因为，在同一时间只有一个线程是处在运行状态，假设运行线程A发现无法获取锁，只能等待解锁，但因为A自身不挂起，所以那个持有锁的线程B没有办法进入运行状态，只能等到操作系统分给A的时间片用完，才能有机会被调度。这种情况下使用自旋锁的代价很高。