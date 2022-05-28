## CH1 Basics  
- Computer system composition
- OS architecture
- OS role
- System types
    - Multiprogramming system
    - Time-sharing system
    - Multiprocessors system
    - Distributed system
    - Real-time system
    - Batch system
    - Handheld system

## Computer system composition  
- 4 components
    - Hardware
        - CPU
        - Memory
        - I/O-device
        - etc.
    - Operating system
    - Application programs
        - Application software
            - Office
            - DBMS
            - etc.
        - System software
            - Text editor
            - Assembler
            - Compiler
            - etc.
    - Users
        - Outside of the computer
- Illustration
    - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/computer_system_composition.png?raw=true)

## OS architecture  
- [other version]
    - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/os_arch_other.png?raw=true)
    - User interface
        - Command interpreter
            - Receive user input, parse command format, run corresponding command file or system call, return result to users.
            - Interface
                - Menu
                - Text mode / command line
                - GUI
        - System calls
            - Communication interface between user program and OS kernel. OS kernel is a set which consists of OS vital service.
- [dinosaur]
    - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/os_arch_dinosaur.png?raw=true)

## OS role  
- OS is a communication interface between users and computer (hardware), make it easy for people to operate computer.
- OS offers an environment which user programs can run on it.
- OS is a resource allocator which can efficiently allocates resource efficiently.
- OS is a watcher which monitors user program execution to prevent critical error caused by intended/unexpected operation.

## System types  
- Multiprogramming system
    - Define
        - Allows multiple Jobs (processes) run in system simultaneously.
    - Main purpose
        - Raise CPU utilization to lower CPU idle time.
    - Tricks
        - Switch CPU between jobs by job scheduling.
        - e.g., If a process is waiting for I/O-complete, then the CPU can be switched to other process.
    - Multiprogramming degree
        - the number of process run on the system at the same time.
        - More degree ,more CPU utilization.
    - The way to run multiple process on the system at the same time.
        - Concurrent execution (1 CPU)
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/concurr_exec.png?raw=true)
        - Parallel execution (mutiple CPUs)
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/para_exec.png?raw=true)
- Time-sharing system  
    - Define
        - It's a logical extension of multiprogramming system, namely, time-sharing is one kind of multiprogramming system.
        - It is called Multi-tasking system in [Dinosaur].
        - Main different between multiprogramming system
            - The switch frequency of CPU is much higher then multiprogramming system.
        - Main features
            - User thinks that it has its own CPU because the CPU can be used due to the High switch frequency.
            - Short response time. (< 1s)
            - RR CPU scheduling, so that CPU can be used fairly and no starvation.
            - Use virtual memory to extend user logical memory space.
            - Suitable for user-interaction application/environment.
            - Use spooling to create multiple virtual I/O-devices.
                - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/spooling.png?raw=true)
- Multiprocessor system
    - Define
        - Also called multiprocessing system, tightly-coupled system and parallel system.
    - Main features
        - There are multiple CPUs resides in a machine (or motherboard), they shared memory, bus, I/O-devices, power supply, etc.
        - Usually these CPUs are controlled by the same CLK and OS.
        - Mostly, the communication between CPUs is shared memory.
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/shared_memory.png?raw=true)
    - Benefits
        - Increase throughput
            - Because it supports parallel execution, multiple jobs can run on different CPU at the same time.
        - Increase reliability
            - If a CPU is failed, another CPU can take over its job, the system is still operational.
        - Economy of scal
            - The CPUs shared hardware resources to each other.
    - It can be subdivided into two types based on CPU capability.
        - Symmetric MultiProcessors (SMP)
            - Define
                - The CPUs have the same capability and they have the same priority to acces resources.
            - Pros
                - Reliability is higher than ASMP.
                - Performance is higher than ASMP.
            - Cons
                - The development of OS is more complicated, because it needs to access shared resources by mutual exclusion mechanism.
        - ASynmmetric MultiProcessors (ASMP)
            - Define
                - The capability of each CPU is not the same, one for I/O, one for computation, etc. It usually implemented by master-slave or boss-employee architecture, which means there is a master CPU to manage or allocate jobs and resources to another slave CPUs.
            - Pros
                - The development of ASMP is more simple, it can be done by editing the single-CPU OS version.
            - Cons
                - Poor reliability.
                - Poor performance. (bottle-neck is master-CPU)
    - Hardware [dinosaur]
        - Multicores chip
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/multicores_chip.png?raw=true)
        - Multiprocessors
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/multiprocessors.png?raw=true)
        - The differences between above hardwares
            - [Multicore] better communication speed.
            - [Multicore] lower power consumption.
        - There's no difference from OS's view. e.g., 1 CPU with 2 cores = 2 CPUs. 4 CPUs with 2 cores = 8 CPUs.
- Distributed system
    - Define
        - it is also called loosely-coupled system.
    - Main features
        - Consists of multiple machines which communicates to each other through network, each machine has its own CPU, memory, bus, power supply, etc.
        - Usually these CPUs are not control by the same CLK.
        - The OS on these machines may not be the same.
        - Usually use message passing to communicate between these CPUs.
        - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/distributed_system.png?raw=true)
    - Pros
        - Increase throughput
        - Increase reliability
        - Cost down (resource sharing)
            - It can support client-server model which means, one machine (server) offers some resources/services, other machine which without the same resources/services can send request to the server to get the operation result. e.g., file server, mail server, printer server, computation server, DNS, etc. (Remote Procedure Call)
        - Fullfill remote communication, e.g., e-mail, FTP, etc.
- Real-time system
    - Two types
        - Hard real-time system
        - Soft real-time system
    - Hard real-time system
        - Define
            - This system must ensure the critical tasks completed on time.
        - examples
            - nuclear secure control, military protection, auto manufacture in factory, robot control, etc.
        - [others]
            - ![](https://github.com/nshawn4675/OS_note/blob/master/_asset/CH1/hard_real_time_system.png?raw=true)
        - system design consideration
            - Take any time delay factor into consideration.
            - Don't use the device/mechanism which operation time is too long/unpredictable, e.g., disk (use ROM, RAM instead), virtual memory (page fault is the problem).
            - Can't be with time-sharing system.
            - The modern enterprise OS which is not support hard real-time system feature
                - UNIX
                - Linux
                - Windows
                - Mac-OS
        - The service time which OS handles needs to be short. (lower kernel dispatch latency)
    - Soft real-time system
        - Define
            - The system must ensure real-time process have the highest priority than the others and retain this level of priority until it finishes.
        - example
            - multimedia system, virtual reality, scientific simulation, etc.
        - system design consideration
            - about CPU scheduling
                - support preemptive priority rule.
                - not support aging mechanism.
            - lower kernel dispatch latency.
            - it can support virtual memory, but the pages of real-time process need to stay in memory, it can't be swapped out until completion.
            - Can be with time-sharing, e.g., Solaris.
        - Modern OS that support soft real-time system feature
            - UNIX
            - Windows
            - Linux
            - etc.
- Batch system
    - Define
        - The non-critical/periodical jobs can be accumulated and batch executed by the system, it doesn't need to interact with users during the execution.
    - Main purpose
        - Raise resource utilization when off-peak. e.g., file tax, scan virus, financial report, inventory check, etc.
    - Don't use user-interactive and real-time app.
- Handheld system
    - example
        - PDA, smart phone, PAD
    - smart phone OS
        - Google Android, Apple IOS, Nokia Symbian
    - Software design on the smart phone is restricted by hardware.
    - | HW limitation | SW requirement |
      |-|-|
      |slower processor<br/>(i) power supply<br/>(ii) heat dissipation<br/>issues|No sophisticated calculation.|
      |small memory size|concise code.<br/>keep unused memory space free.|
      |small display monitor size|clip display content, e.g., web clipping|