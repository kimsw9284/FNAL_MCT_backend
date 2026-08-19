```mermaid
graph TD
    %% Network Infrastructure
    Gateway["Gateway: labc1"]
    DAQServer["Central DAQ PC: lab-daq-02<br/>192.168.0.100"]

    Gateway --> DAQServer

    %% ATCA Crate Subgraph
    subgraph ATCA ["ATCA Crate (Lab 6 - Rack 2)"]
        direction TB

        Slot7["Slot 7: DTH Hub<br/>CTRL: 192.168.0.107<br/>IPMC: 192.168.0.127"]

        subgraph TriggerGroup ["Trigger Section"]
            Slot3["Slot 3: Serenity (Trigger)<br/>CTRL: 192.168.0.103<br/>IPMC: 192.168.0.123"]
            Slot4["Slot 4: Serenity (Trigger)<br/>CTRL: 192.168.0.104<br/>IPMC: 192.168.0.124"]
        end

        subgraph DAQGroup ["DAQ Section"]
            Slot10["Slot 10: Serenity (DAQ)<br/>CTRL: 192.168.0.110<br/>IPMC: 192.168.0.130"]
            Slot11["Slot 11: Serenity (DAQ)<br/>CTRL: 192.168.0.111<br/>IPMC: 192.168.0.131"]
        end

        subgraph CosmicGroup ["Cosmic Ray Trigger Section"]
            Slot13["Slot 13: Serenity (Cosmic)<br/>CTRL: 192.168.0.113<br/>IPMC: 192.168.0.133"]
            Slot14["Slot 14: Serenity (Cosmic)<br/>CTRL: 192.168.0.114<br/>IPMC: 192.168.0.134"]
        end
    end

    %% Network & Physical Connections
    DAQServer -.-|Private Subnet| ATCA

    %% Optical S-Link / Fiber Mapping Connections
    
    %% Slot 3 (Trigger) -> DTH Fed 1
    Slot3 ==>|DTH Fed 1<br/>Fiber [1,12] ➔ [1,12]| Slot7

    %% Slot 11 (DAQ) -> DTH Fed 0 (3 Fiber Pairs)
    Slot11 ==>|DTH Fed 0<br/>Fiber [5,5] ➔ [15,22]| Slot7
    Slot11 ==>|DTH Fed 0<br/>Fiber [7,7] ➔ [14,23]| Slot7
    Slot11 ==>|DTH Fed 0<br/>Fiber [9,9] ➔ [13,24]| Slot7

    %% Disconnected / Future Links
    Slot7 -.-|Future 100G Ethernet| DAQServer

    %% Styling Definitions
    classDef gateway fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#fff;
    classDef server fill:#2b6cb0,stroke:#3182ce,stroke-width:2px,color:#fff;
    classDef dth fill:#d69e2e,stroke:#b7791f,stroke-width:2px,color:#000;
    classDef trig fill:#dd6b20,stroke:#c05621,stroke-width:2px,color:#fff;
    classDef daq fill:#319795,stroke:#2c7a7b,stroke-width:2px,color:#fff;
    classDef cosmic fill:#805ad5,stroke:#6b46c1,stroke-width:2px,color:#fff;

    %% Apply Classes
    class Gateway gateway;
    class DAQServer server;
    class Slot7 dth;
    class Slot3,Slot4 trig;
    class Slot10,Slot11 daq;
    class Slot13,Slot14 cosmic;
