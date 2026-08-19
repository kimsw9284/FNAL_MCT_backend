```mermaid
graph TD
    %% Network Infrastructure
    Gateway["Gateway: labc1"]
    DAQServer["Central DAQ PC: lab-daq-02<br/>192.168.0.100"]

    Gateway --> DAQServer

    %% ATCA Crate Subgraph
    subgraph ATCA ["ATCA Crate (Lab 6 - Rack 2)"]
        direction TB

        %% DTH Board & Interfaces
        Slot7["Slot 7: DTH<br/>CTRL: 192.168.0.107<br/>IPMC: 192.168.0.127"]
        Fed0["DTH Fed 0"]
        Fed1["DTH Fed 1"]

        %% Internal DTH Wiring
        Slot7 ~~~ Fed0
        Slot7 ~~~ Fed1

        subgraph TriggerGroup ["Trigger (Cassettes)"]
            Slot3["Slot 3: Serenity (Trigger)<br/>CTRL: 192.168.0.103<br/>IPMC: 192.168.0.123<br/>Connected to DTH Fed 1"]
            Slot4["Slot 4: Serenity (Trigger)<br/>CTRL: 192.168.0.104<br/>IPMC: 192.168.0.124<br/>Not connected to DTH"]
            Slot3 ~~~ Slot4
        end

        subgraph DAQGroup ["DAQ (Cassettes)"]
            Slot10["Slot 10: Serenity (DAQ)<br/>CTRL: 192.168.0.110<br/>IPMC: 192.168.0.130<br/>Not connected to DTH"]
            Slot11["Slot 11: Serenity (DAQ)<br/>CTRL: 192.168.0.111<br/>IPMC: 192.168.0.131<br/>Connected to DTH Fed 0"]
            Slot10 ~~~ Slot11
        end

        subgraph CosmicGroup ["Cosmic Ray Trigger"]
            Slot13["Slot 13: Serenity (Cosmic)<br/>CTRL: 192.168.0.113<br/>IPMC: 192.168.0.133<br/>Not connected to DTH"]
            Slot14["Slot 14: Serenity (Cosmic)<br/>CTRL: 192.168.0.114<br/>IPMC: 192.168.0.134<br/>Not connected to DTH"]
            Slot13 ~~~ Slot14
        end
    end

    %% Network & Physical Connections
    DAQServer -.-|"Private Subnet"| ATCA

    %% Optical S-Link / Fiber Mapping Connections directly to FED Nodes
    
    %% Slot 3 (Trigger) -> DTH Fed 1
    Slot3 ==>|"Fiber [1,12] -> [1,12]"| Fed1

    %% Slot 11 (DAQ) -> DTH Fed 0 (3 Fiber Pairs)
    Slot11 ==>|"Fiber [5,5] -> [15,22]"| Fed0
    Slot11 ==>|"Fiber [7,7] -> [14,23]"| Fed0
    Slot11 ==>|"Fiber [9,9] -> [13,24]"| Fed0

    %% Disconnected / Future Links
    Slot7 -.-|"Future 100G Ethernet (not installed yet)"| DAQServer

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
    class Slot7,Fed0,Fed1 dth;
    class Slot3,Slot4 trig;
    class Slot10,Slot11 daq;
    class Slot13,Slot14 cosmic;
