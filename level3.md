# level3

## Goal1
* <font color="red">***InterfaceA1***</font>と<font color="blue">***InterfaceB1***</font>のサブネットマスクがセットされていないため、値がセットされている<font color="green">***InterfaceC1***</font>のサブネットマスク に合わせる。（省略）
* 既にIPアドレスが設定されている<font color="red">***InterfaceA1***</font>とネットワークアドレスは一致し、ホストアドレスが異なるIPアドレスを<font color="blue">***InterfaceB1***</font>に設定する。

## Goal2
* 既にIPアドレスが設定されている<font color="red">***InterfaceA1***</font>とネットワークアドレスは一致し、ホストアドレスが異なるIPアドレスを<font color="green">***InterfaceC1***</font>に設定する。

## Goal3
* Goal1とGoal2の設定が競合しないようなIPアドレスを調整する。

## chart
```mermaid
flowchart
subgraph Goal2
    direction BT
    CA[[ClientA]]
    CB[[ClientB]]
    CC[[ClientC]]
    IA[InterfaceA1]
    IB[InterfaceB1]
    IC[InterfaceC1]
    S{SwichS}
    CA-->IA-->CA
    IA-->S-->IA
    CC-->IC-->CC
    IC-->S-->IC
    CB-->IB-->CB
    IB-->S-->IB
end
```
## example
```mermaid
flowchart
Goal3_before-->Goal3_after
subgraph Goal3_before
    direction TB
    G3_BE_CL_C[[ClientC]]
    G3_BE_CL_B[[ClientB]]
    G3_BE_IF_C[0.0.0.0/255.255.255.128]
    G3_BE_IF_B[0.0.0.0/0.0.0.0]
    G3_BE_S{SwichS}
    G3_BE_CL_C-->G3_BE_IF_C-->G3_BE_S-->G3_BE_IF_B-->G3_BE_CL_B
end

subgraph Goal3_after
    direction TB
    G3_AF_CL_C[[ClientC]]
    G3_AF_CL_B[[ClientB]]
    G3_AF_IF_C[104.198.53.124/255.255.255.128]
    G3_AF_IF_B[104.198.53.123/255.255.255.128]
    G3_AF_S{SwichS}
    G3_AF_CL_C-->G3_AF_IF_C-->G3_AF_S-->G3_AF_IF_B-->G3_AF_CL_B
end

Goal2_before-->Goal2_after
subgraph Goal2_before
    direction TB
    G2_BE_CL_A[[ClientA]]
    G2_BE_CL_C[[ClientC]]
    G2_BE_IF_A[104.198.53.125/0.0.0.0]
    G2_BE_IF_C[0.0.0.0/255.255.255.128]
    G2_BE_S{SwichS}
    G2_BE_CL_A-->G2_BE_IF_A-->G2_BE_S-->G2_BE_IF_C-->G2_BE_CL_C
end

subgraph Goal2_after
    direction TB
    G2_AF_CL_A[[ClientA]]
    G2_AF_CL_C[[ClientC]]
    G2_AF_IF_A[104.198.53.125/255.255.255.128]
    G2_AF_IF_C[104.198.53.124/255.255.255.128]
    G2_AF_S{SwichS}
    G2_AF_CL_A-->G2_AF_IF_A-->G2_AF_S-->G2_AF_IF_C-->G2_AF_CL_C
end


Goal1_before-->Goal1_after
subgraph Goal1_before
    direction TB
    G1_BE_CL_A[[ClientA]]
    G1_BE_CL_B[[ClientB]]
    G1_BE_IF_A[104.198.53.125/0.0.0.0]
    G1_BE_IF_B[0.0.0.0/0.0.0.0]
    G1_BE_S{SwichS}
    G1_BE_CL_A-->G1_BE_IF_A-->G1_BE_S-->G1_BE_IF_B-->G1_BE_CL_B
end

subgraph Goal1_after
    direction TB
    G1_AF_CL_A[[ClientA]]
    G1_AF_CL_B[[ClientB]]
    G1_AF_IF_A[104.198.53.125/255.255.255.128]
    G1_AF_IF_B[104.198.53.123/255.255.255.128]
    G1_AF_S{SwichS}
    G1_AF_CL_A-->G1_AF_IF_A-->G1_AF_S-->G1_AF_IF_B-->G1_AF_CL_B
end
```
