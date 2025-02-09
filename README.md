# Objective + Roadmap
This project is focused on designing a serializer/deserializer(SerDes) transceiver for high-speed, high-bandwidth data transfer within an Adpative 5G Beamformer module application.
The design is intended to send beam steering instructions(data) over a high speed channel, an improvement on the SPI-Master/SPI-Slave operations done for signal processing in my capstone [write up](https://assets.zyrosite.com/YyGzaM3Vx5Co27Mb/capstone-mePGD21OBlfrXGlL.pdf).

### Details and Specifications
- Accounting for the clock domain crossing issues(skew) associated with data transfer over an extremely high frequency clock, an asynchronous FIFO is implemented at the deserializer input ports to sync up system data flow. With the same thought process, a Double flop synchronizer is placed within `serializer.sv` to account for metastability, ensuring that the signal is sampled correctly at the destination domain.
- The design validation testbench for this project follows the Universal Verification Methodology(UVM) standard for digital design verification. UVM Driver, UVM Monitor, UVM Interface, UVM Scoreboard and UVM Environment were the components used:
     - `Serial_sequence`, `Deserial_follow_sequence`: This forms the base transaction object that will be used in the environment to initiate new transactions and capture transactions at DUT interface.
     - `Serial_driver`, `Deserial_driver`: This is responsible for driving transactions to the DUT - main function is to get a transaction from sequencer (if available_ and drive it to interface.
     - `Serial_interace`, `Deserial_interface`: This is allows verification components to access DUT signals using a virtual interface handle.
     - `Serial_monitor`, `Deserial_monitor`: This possesses a virtual interface handle with which it can monitor the events happening on the interface. It sees new transactions and then captures (cont'd) 
     - (Cont'd) information into a packet and sends it to the scoreboard.
     - `Serial_agent`, `Deserial_agent_A`,`Deserial_agent_B`: This is an intermediary container to hold driver, monitor and sequencer objects. `Serial_agent_A` receives stimulus output from
     - (Cont'd) `Serial_interace`.
     - `Serial_sequencer`, `Deserial_sequencer`: This generated data transactions as class objects and sends it to the Driver for execution. `Serial_agent_A` leverages `Deserial_follow_sequence` to parameterize request and response item types for
     - (Cont'd)`Deserial_sequencer`.

-  The Bit Error Rate (BER) defines the percentage of received bits that are corrupted due to factors such as loss or noise. For this project, it should be  approximately 10^12
- Project should be run on Vivado™ Edition - 2024.2

### System Architecture
<img  align="center" src="https://github.com/user-attachments/assets/42fa0303-b6f5-4f76-8e46-fbd153972029" width="550px" />

### UVM Testbench Framework
<img  align="center" src="https://github.com/user-attachments/assets/c261ac6f-940f-44f9-b657-7ce6bacc6354" width="550px" />
