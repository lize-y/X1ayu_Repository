- **互斥锁**加锁失败后，线程会**释放 CPU** ，给其他线程；
- **互斥锁**会记录下**访问锁的线程的id**，用于进行**线程切换**、**组织阻塞队列**等操作
# 原理
- 依赖于硬件提供的原子指令
- **[[CAS]]**
- 阻塞
# 流程
## 加锁
- **尝试加锁**
	- **未锁定**，直接通过原子操作将状态置为锁定，无需进入内核态
	- **锁定**
		- **自旋尝试**：在正常模式下，线程先自旋（循环执行PAUSE指令）尝试获取锁，减少上下文切换开销（自旋次数通常限制为4次）。
		- **排队等待**：自旋失败后，线程被加入等待队列，通过信号量（如Linux的futex）进入阻塞状态，由操作系统调度唤醒。
		- **饥饿模式处理**：若等待时间超过阈值（如1ms），锁切换为饥饿模式，新请求直接进入队列尾部，避免新线程“插队”
## 解锁
- **释放锁**：通过原子操作将锁状态复位，并检查是否有等待线程。
- **唤醒策略**：
	- **正常模式**：唤醒等待队列头部的线程，与新请求竞争锁（提升吞吐量）3。
	- **饥饿模式**：直接将锁交给队列头部线程，确保公平性