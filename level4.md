# level4

## Goal1
* <font color="red">***InterfaceA1***</font>と<font color="blue">***InterfaceB1***</font>と<font color="green">***InterfaceR1***</font>のサブネットマスクがセットされていないため、適当なサブネットマスクを設定する。（省略）
* 既にIPアドレスが設定されている<font color="red">***InterfaceA1***</font>とネットワークアドレスは一致し、ホストアドレスが異なるIPアドレスを<font color="blue">***InterfaceB1***</font>に設定する。

## Goal2
* 既にIPアドレスが設定されている<font color="red">***InterfaceA1***</font>とネットワークアドレスは一致し、ホストアドレスが異なるIPアドレスを<font color="green">***InterfaceR1***</font>に設定する。

## Goal3
* <font color="yellow">***InterfaceR2***</font>と<font color="skayblue">***InterfaceR3***</font>と（<font color="red">***InterfaceA1***</font>,<font color="blue">***InterfaceB1***</font>,<font color="green">***InterfaceR1***</font>）のネットワークアドレスが異なり、且つ、<font color="red">***InterfaceA1***</font>と<font color="blue">***InterfaceB1***</font>と<font color="green">***InterfaceR1***</font>のネットワークアドレスが同一、且つ、ホストアドレスが異なるIPアドレスに調節する。

## chart
```mermaid
%%{init:{'theme': 'dark'}}%%
%%{init:{'flowchart': {'curve': 'natural'}}}%%
flowchart
subgraph Goal2
    direction BT
    CA[[ClientA]]
    CB[[ClientB]]
    RR(((RouterR)))
    IA[InterfaceA1]
    IB[InterfaceB1]
    IR1[InterfaceR1]
    IR2[InterfaceR2]
    IR3[InterfaceR3]
    S{SwichS}
    CA-->IA-->CA
    IA-->S-->IA
    RR-.->IR2
    RR-.->IR3
    RR-->IR1-->RR
    IR1-->S-->IR1
    CB-->IB-->CB
    IB-->S-->IB
end
```
## example
```mermaid
%%{init:{'theme': 'dark'}}%%
%%{init:{'flowchart': {'curve': 'natural'}}}%%
flowchart
Goal3_before-->Goal3_after
subgraph Goal3_before
    direction TB
    G3_BE_CL_B[[ClientB]]
    G3_BE_RR(((RouterR)))
    G3_BE_IF_B[0.0.0.0/0.0.0.0]
    G3_BE_IF_R1[0.0.0.0/0.0.0.0]
    G3_BE_S{SwichS}
    G3_BE_CL_B-->G3_BE_IF_B-->G3_BE_S-->G3_BE_IF_R1-->G3_BE_RR
end

subgraph Goal3_after
    direction TB
    G3_AF_CL_B[[ClientB]]
    G3_AF_RR(((RouterR)))
    G3_AF_IF_B[117.254.115.134/255.255.0.0]
    G3_AF_IF_R1[117.254.115.133/255.255.0.0]
    G3_AF_S{SwichS}
    G3_AF_CL_B-->G3_AF_IF_B-->G3_AF_S-->G3_AF_IF_R1-->G3_AF_RR
end

Goal2_before-->Goal2_after
subgraph Goal2_before
    direction TB
    G2_BE_CL_A[[ClientA]]
    G2_BE_RR(((RouterR)))
    G2_BE_IF_A[117.254.115.132/0.0.0.0]
    G2_BE_IF_R1[0.0.0.0/0.0.0.0]
    G2_BE_S{SwichS}
    G2_BE_CL_A-->G2_BE_IF_A-->G2_BE_S-->G2_BE_IF_R1-->G2_BE_RR
end

subgraph Goal2_after
    direction TB
    G2_AF_CL_A[[ClientA]]
    G2_AF_RR(((RouterR)))
    G2_AF_IF_A[117.254.115.132/255.255.0.0]
    G2_AF_IF_R1[117.254.115.133/255.255.0.0]
    G2_AF_S{SwichS}
    G2_AF_CL_A-->G2_AF_IF_A-->G2_AF_S-->G2_AF_IF_R1-->G2_AF_RR
end


Goal1_before-->Goal1_after
subgraph Goal1_before
    direction TB
    G1_BE_CL_A[[ClientA]]
    G1_BE_CL_B[[ClientB]]
    G1_BE_IF_A[117.254.115.132/0.0.0.0]
    G1_BE_IF_B[0.0.0.0/0.0.0.0]
    G1_BE_S{SwichS}
    G1_BE_CL_A-->G1_BE_IF_A-->G1_BE_S-->G1_BE_IF_B-->G1_BE_CL_B
end

subgraph Goal1_after
    direction TB
    G1_AF_CL_A[[ClientA]]
    G1_AF_CL_B[[ClientB]]
    G1_AF_IF_A[117.254.115.132/255.255.0.0]
    G1_AF_IF_B[117.254.115.134/255.255.0.0]
    G1_AF_S{SwichS}
    G1_AF_CL_A-->G1_AF_IF_A-->G1_AF_S-->G1_AF_IF_B-->G1_AF_CL_B
end
```
