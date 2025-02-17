RAII
智能指针（std::shared_ptr和std::unique_ptr）即RAII最具代表的实现，使用智能指针，可以实现自动的内存管理，再也不需要担心忘记delete造成的内存泄漏。
在使用CRITICAL_SECTION时，EnterCriticalSection和LeaveCriticalSection必须成对使用，很多时候，经常会忘了调用LeaveCriticalSection，此时就会发生死锁的现象。
std::thread：借助 C++ 的 RAII 机制，线程对象生命周期结束时，相关资源会自动清理，例如调用join或者detach ，减少了因人为疏忽导致的资源管理问题。

RTTI
