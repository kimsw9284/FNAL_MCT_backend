```mermaid
graph TD
    %% Global Styles
    classDef default fill:#1e1e1e,stroke:#fff,stroke-width:1px,color:#fff;
    classDef gateway fill:#2d3748,stroke:#4a5568,stroke-width:2px,color:#fff;
    classDef server fill:#2b6cb0,stroke:#3182ce,stroke-width:2px,color:#fff;
    classDef crate fill:#1a202c,stroke:#a0aec0,stroke-width:2px,color:#fff;
    classDef dth fill:#d69e2e,stroke:#b7791f,stroke-width:2px,color:#000;
    classDef trig fill:#dd6b20,stroke:#c05621,stroke-width:2px,color:#fff;
    classDef daq fill:#319795,stroke:#2c7a7b,stroke-width:2px,color:#fff;
    classDef cosmic fill:#805ad5,stroke:#6b46c1,stroke-width:2px,color:#fff;

    %% Network & Server Infrastructure
    Gateway["<b>Gateway:</b> labc1"] ::: gateway
    DAQServer["<b>Central DAQ PC:</b> lab-daq-02<br/><code>192.168.0.100</code>"] ::: server

    Gateway --> DAQServer

    %% ATCA Crate Subgraph
    subgraph ATCA ["ATCA Crate (Lab 6 - Rack 2)"]
        direction TB

        Slot7["<b>Slot 7: DTH Hub</b><br/>CTRL: 192.168.0.107<br/>IPMC: 192.168.0.127"] ::: dth

        subgraph TriggerGroup ["Trigger Section"]
            Slot3["<b>Slot 3: Serenity (Trigger)</b><br/>CTRL: 192.168.0.103<br/>IPMC: 192.168.0.123"] ::: trig
            Slot4["<b>Slot 4: Serenity (Trigger)</b><br/>CTRL: 192.168.0.104<br/>IPMC: 192.168.0.124"] ::: trig
        end

        subgraph DAQGroup ["DAQ Section"]
            Slot10["<b>Slot 10: Serenity (DAQ)</b><br/>CTRL: 192.168.0.110<br/>IPMC: 192.168.0.130"] ::: daq
            Slot11["<b>Slot 11: Serenity (DAQ)</b><br/>CTRL: 192.168.0.111<br/>IPMC: 192.168.0.131"] ::: daq
        end

        subgraph CosmicGroup ["Cosmic Ray Trigger Section"]
            Slot13["<b>Slot 13: Serenity (Cosmic)</b><br/>CTRL: 192.168.0.113<br/>IPMC: 192.168.0.133"] ::: cosmic
            Slot14["<b>Slot 14: Serenity (Cosmic)</b><br/>CTRL: 192.168.0.114<br/>IPMC: 192.168.0.134"] ::: cosmic
        end
    end

    %% Network Connections
    DAQServer ---|Private Subnet| ATCA

    %% S-Link / FED Optical Connections
    Slot3 ==>|Fed 0| Slot7
    Slot4 ==>|Fed 0| Slot7
    Slot10 ==>|Fed 1| Slot7
    Slot11 ==>|Fed 1| Slot7

    %% Disconnected / Planned Links
    Slot7 -.-|Future 100G Ethernet| DAQServer
