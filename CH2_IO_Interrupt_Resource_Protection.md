## CH2 I/O operation, Interrupt and Resource Protection  
- I/O operation
    - Polling I/O
    - Interrupted I/O
    - DMA
- Interrupt mechanism and types
- Hardware resource protection
    - Basics
        - Dual mode operation
        - Privileged instruction
    - I/O protection
    - Memory protection
    - CPU protection


## I/O operation  
- Polling I/O
    - Define
        - It is also called busy-waiting I/O or programmed I/O.
    - Steps
        - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/polling_io.png?raw=true)
        1. Running process send "I/O-request" to OS.
        2. OS may pause current process.
        3. I/O-subsystem in the kernel receives I/O-request, if it can't find the request data in the cache, it will send request to I/O-Device driver
        4. I/O-Device driver send I/O-commands to I/O-Device controller.
        5. OS may switch CPU to another process.
        6. CPU keeps polling I/O-Device controller register to check whether the I/O is done or error occured.
    - Cons
        - CPU utilization is low, process throughput is low.
- Interrupted I/O
    - steps
        - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/int_io.png?raw=true)
        - step.1 ~ step.5 is same as polling I/O
        6. When I/O-completed, the I/O-Device controller sends "I/O-completed" interrupt to OS.
        7. OS receives interrupt, may pause current process, stores its status and context.
        8. OS searches interrupt service routine (ISR) address in the interrupt vector based on interrupt ID.
        9. Jump to ISR address, run ISR.
        10. kernel get control after ISR finished, do some final routines, e.g., nofity the process that its I/O operation is done.
        11. resume paused process.
    - Pros
        - CPU don't need to spend much time on polling.
        - CPU utilization and thoughput is higher.
    - Cons
        - The process of interrupt still need some CPU time. User process can't run during CPU is searching/running ISR.
        - If the frequency of interrupt is very high, then CPU utility is nearly zero, system performance will be very poor, because it spends too much time on interrupt handling.
        - CPU still need to spend some time on data transfer between I/O-Device and memory.
- DMA
    - Define
        - DMA controller is responsible for data transfer between I/O-Device and memory. CPU don't need to take part of and monitor it.
    - Pros
        - CPU utilization is higher.
        - CPU throughput is higher.
        - DMA is suite for block-transfer oriented I/O-Device, e.g., disk. Because interrupt frequency is relatively lower.
    - Cons
        - It will complicate hardware design because DMA will fight for memory, bus with CPU.
    - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/int_io_rqst.png?raw=true)
    - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/dma_transfer.png?raw=true)
        1. Device driver is told to transfer disk data to buffer at address x.
        2. Device driver tells disk controller to transfer to transfer C bytes from disk to buffer at address x.
        3. Disk controller initiates DMA transfer.
        4. Disk controller sends each byte to DMA controller.
        5. DMA controller transfers bytes to buffer x, increase memory address ad decrease C until C=0.
        6. When C=0, DMA interrupts CPU to signal transfer completion.
- Interrupt mechanism and types
    - there are interrupt vector and all kinds of ISR code in the OS kernel memory area.
        - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/int_vec_isr_code_in_kernel.png?raw=true)
    - When interrupt occurs
        1. OS may pause current process, and store process context.
        2. OS check interrupt vector by interrupt ID to confirm interrupt type and ISR address.
        3. Jumpt to ISR address, run ISR.
        4. when ISR　is finished, kernel get CPU control back.
        5. Kernel resume paused process.
    - Interrupt types
        - Type 1
            - External interrupt
                - Issue by CPU peripheral device/component.
                - e.g., I/O-completed, I/O-error, machine-check, etc.
            - Internal interrupt
                - Fatal error occurs when CPU is running process.
                - e.g., divide-by-zero, illegal memory access, illegal instruction execution, etc.
            - Software interrupt
                - Issued by User process when it needs OS service.
        - Type 2
            - |Interrupt|Trap|
              |-|-|
              |Hardware generated change of control flow.<br/>e.g., I/O-completed, I/O-error, machine-check, time-out, etc.|Software generated interrupt.<br/>(1) Fatal error caused by process, e.g., divided-by-zero, illegal memory access, illegal instruction execution, etc.<br/>(2) Issued by process when it needs OS service, e.g., I/O-request|
        - Type 3
            - Maskable interrupt
                - When this type of interrupt occurs, it can be postponed/ignored.
                - e.g., low priority interrupts like soft interrupt.
            - Non-maskable interrupt
                - When this type of interrupt occurs, it needs to be handle immediately.
                - e.g., fatal errors like divide-by-zero, time-out in RR.
- Hardware resource protection
    - fundamental
        - Dual modes operation
            - Define
                - OS operation needs to be divided into 2 modes: kernel mode, user mode.
            - Kernel mode
                - also called system mode, privileged mode, supervisor mode, monitor mode. This is the mode which kernel get CPU control to run system process. Privileged instruction can be run in this mode.
                - e.g., ISR, system call.
            - User mode
                - This is the mode which user process get CPU control, privileged instruction can't be run in this mode.
            - It needs hardware support to implement dual mode mechanism, there's a mode bit in CPU.
        - Privileged instruction
            - Define
                - Privileged instruction can be regarded as the instructions that may cause system fatal error, and it can be executed only in kernel mode.
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/privilege_instruction.png?raw=true)
    - I/O protection
        - Purpose
            - Prevent user process doing direct I/O-Device operation.
            - Prevent unexpected I/O-Device operation.
            - Lower complexity for user process to do I/O operation.
        - Method
            - Based on dual mode and privileged instruction, set every I/O instruction to privileged instruction, so that user process can only do I/O operation by sending request to OS.
    - Memory protection
        - Purpose
            - Prevent user process to modify other user process memory area and kernel memory space.
        - Method
            - OS can make use of "base" and "limit" register to record process starting/ending point and size.
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/mem_protect.png?raw=true)
                - Beside, the set/change function for base/limit register needs to be set as privileged instruction.
    - CPU protection
        - Purpose
            - Prevent process to occupy CPU for a long/unlimited time.
        - Method
            - Take use of the maximum time quantum/slice in the CPU, when process get CPU, the timer is default to maximum time quantum, as process run time iuncrease, the timer value decrease, the timer will send a time-out interrupt to OS when the timer value = 0, then OS can force the process to release CPU.
            - Beside, the set/change function for timer value needs to be set as privileged instruction.
- Others
    - Blocking I/O (Synchronous I/O)
        - Process suspend (block) until I/O-completed when process send I/O-request.
        - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/blk_io.png?raw=true)
        - Easy to use and understand.
        - Insufficient to some needs, e.g., video display (movie).
    - Non-Blocking I/O
        - After process send I/O-request, it can continue running concurrently with I/O operation.
        - e.g., user interface, data copy (buffered I/O).
        - Implemented with multi-threading.
        - Returns quickly with counts of bytes read or writen.
        - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH2/non_blk_io.png?raw=true)
        - I/O calls return as much as available.
    - Asynchronous I/O
        - Similar with non-blocking I/O.
        - Differ from non-blocking I/O
            - I/O-subsystem runs when I/O-completed.