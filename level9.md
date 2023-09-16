# level9

## Goal1
* 全てのサブネットマスクを255.255.255.240(/28)に統一する。（省略）
* <font color="red">***ClientC***</font>側のネットワーク、<font color="blue">***ClientD***</font>側のネットワーク、<font color="green">***InterfaceR21***</font>側のネットワークのそれぞれ別々のネットワークアドレスで同一ネットワーク内では別々のホストアドレスを設定する。<br>注意点として、<font color="yellow">***Internet***</font>が内部ネットワークへの返却アドレスとして指定しているサブネットマスクに/26が設定されており、<font color="skayblue">***InterfaceD1***</font>でサブネットマスクに255.255.255.240(/28)が設定されているため、二進数で上位26bitは共通で上位28bitがそれぞれのネットワークで異なる必要がある。

## chart
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
subgraph LEVEL9
    direction TB
    NET{{Internet}}
    RR1(((RouterR1)))
    RR2(((RouterR2)))
    S{Switch}
    CA[[ClientA]]
    CB[[ClientB]]
    CC[[ClientC]]
    CD[[ClientD]]
    IA[InterfaceA1]
    IB[InterfaceB1]
    IC[InterfaceC1]
    ID[InterfaceD1]
    IR11[InterfaceR11]
    IR12[InterfaceR12]
    IR13[InterfaceR13]
    IR21[InterfaceR21]
    IR22[InterfaceR22]
    IR23[InterfaceR23]

    CB-->IB-->S-->IB-->CB
    CA-->IA-->S-->IA-->CA
    S-->IR11-->RR1-->IR11-->S
    NET-->IR12-->RR1-->IR12-->NET
    RR1-->IR13-->IR21-->RR2-->IR21-->IR13-->RR1
    CD-->ID-->IR23-->RR2-->IR23-->ID-->CD
    CC-->IC-->IR22-->RR2-->IR22-->IC-->CC
end
```

## example
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart

Goal6_before-->Goal6_after
subgraph Goal6_before
    direction TB
    G6_BE_NET{{Internet<br>0.0.0.0/0 => 163.172.250.12<br>0.0.0.0/0 => 163.172.250.12<br>0.0.0.0/0 => 163.172.250.12}}
    G6_BE_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G6_BE_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G6_BE_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    G6_BE_IF_C[0.0.0.0/0.0.0.0]
    G6_BE_IF_R12[163.172.250.12/255.255.255.240]
    G6_BE_IF_R13[0.0.0.0/0.0.0.0]
    G6_BE_IF_R21[0.0.0.0/255.255.255.252]
    G6_BE_IF_R22[0.0.0.0/0.0.0.0]
    G6_BE_CL_C-->G6_BE_IF_C-->G6_BE_IF_R22-->G6_BE_RR2-->G6_BE_IF_R21-->G6_BE_IF_R13-->G6_BE_RR1-->G6_BE_IF_R12-->G6_BE_NET
end

subgraph Goal6_after
    direction TB
    G6_AF_NET{{Internet<br>0.0.0.0/0 => 163.172.250.12<br>0.0.0.0/0 => 163.172.250.12<br>0.0.0.0/0 => 163.172.250.12}}
    G6_AF_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G6_AF_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G6_AF_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    G6_AF_IF_C[0.0.0.0/0.0.0.0]
    G6_AF_IF_R12[163.172.250.12/255.255.255.240]
    G6_AF_IF_R13[0.0.0.0/0.0.0.0]
    G6_AF_IF_R21[0.0.0.0/255.255.255.252]
    G6_AF_IF_R22[0.0.0.0/0.0.0.0]
    G6_AF_CL_C-->G6_AF_IF_C-->G6_AF_IF_R22-->G6_AF_RR2-->G6_AF_IF_R21-->G6_AF_IF_R13-->G6_AF_RR1-->G6_AF_IF_R12-->G6_AF_NET
end


Goal5_before-->Goal5_after
subgraph Goal5_before
    direction TB
    G5_BE_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G5_BE_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G5_BE_S{Switch}
    G5_BE_CL_B[[ClientB<br>0.0.0.0/0 => 0.0.0.0]]
    G5_BE_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    G5_BE_IF_B[0.0.0.0/0.0.0.0]
    G5_BE_IF_C[0.0.0.0/0.0.0.0]
    G5_BE_IF_R11[0.0.0.0/255.255.255.128]
    G5_BE_IF_R13[0.0.0.0/0.0.0.0]
    G5_BE_IF_R21[0.0.0.0/255.255.255.128]
    G5_BE_IF_R22[0.0.0.0/0.0.0.0]
    G5_BE_CL_B-->G5_BE_IF_B-->G5_BE_S-->G5_BE_IF_R11-->G5_BE_RR1-->G5_BE_IF_R13-->G5_BE_IF_R21-->G5_BE_RR2-->G5_BE_IF_R22-->G5_BE_IF_C-->G5_BE_CL_C
end

subgraph Goal5_after
    direction TB
    G5_AF_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G5_AF_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G5_AF_S{Switch}
    G5_AF_CL_B[[ClientB<br>0.0.0.0/0 => 0.0.0.0]]
    G5_AF_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    G5_AF_IF_B[0.0.0.0/0.0.0.0]
    G5_AF_IF_C[0.0.0.0/0.0.0.0]
    G5_AF_IF_R11[0.0.0.0/255.255.255.128]
    G5_AF_IF_R13[0.0.0.0/0.0.0.0]
    G5_AF_IF_R21[0.0.0.0/255.255.255.128]
    G5_AF_IF_R22[0.0.0.0/0.0.0.0]
    G5_AF_CL_B-->G5_AF_IF_B-->G5_AF_S-->G5_AF_IF_R11-->G5_AF_RR1-->G5_AF_IF_R13-->G5_AF_IF_R21-->G5_AF_RR2-->G5_AF_IF_R22-->G5_AF_IF_C-->G5_AF_CL_C
end


Goal4_before-->Goal4_after
subgraph Goal4_before
    direction TB
    G4_BE_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G4_BE_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G4_BE_S{Switch}
    G4_BE_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    G4_BE_CL_D[[ClientD<br>0.0.0.0/0 => 96.94.26.75]]
    G4_BE_IF_A[0.0.0.0/0.0.0.0]
    G4_BE_IF_D[0.0.0.0/0.0.0.0]
    G4_BE_IF_R11[0.0.0.0/255.255.255.128]
    G4_BE_IF_R13[0.0.0.0/0.0.0.0]
    G4_BE_IF_R21[0.0.0.0/255.255.255.252]
    G4_BE_IF_R23[0.0.0.0//18]
    G4_BE_CL_A-->G4_BE_IF_A-->G4_BE_S-->G4_BE_IF_R11-->G4_BE_RR1-->G4_BE_IF_R13-->G4_BE_IF_R21-->G4_BE_RR2-->G4_BE_IF_R23-->G4_BE_IF_D-->G4_BE_CL_D
    
end

subgraph Goal4_after
    direction TB
    G4_AF_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G4_AF_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G4_AF_S{Switch}
    G4_AF_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    G4_AF_CL_D[[ClientD<br>0.0.0.0/0 => 96.94.26.75]]
    G4_AF_IF_A[0.0.0.0/0.0.0.0]
    G4_AF_IF_D[0.0.0.0/0.0.0.0]
    G4_AF_IF_R11[0.0.0.0/255.255.255.128]
    G4_AF_IF_R13[0.0.0.0/0.0.0.0]
    G4_AF_IF_R21[0.0.0.0/255.255.255.252]
    G4_AF_IF_R23[0.0.0.0//18]
    G4_AF_CL_A-->G4_AF_IF_A-->G4_AF_S-->G4_AF_IF_R11-->G4_AF_RR1-->G4_AF_IF_R13-->G4_AF_IF_R21-->G4_AF_RR2-->G4_AF_IF_R23-->G4_AF_IF_D-->G4_AF_CL_D
end


Goal3_before-->Goal3_after
subgraph Goal3_before
    direction TB
    G3_BE_NET{{Internet}}
    G3_BE_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G3_BE_S{Switch}
    G3_BE_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    G3_BE_IF_A[0.0.0.0/0.0.0.0]
    G3_BE_IF_R11[0.0.0.0/255.255.255.128]
    G3_BE_IF_R12[163.172.250.12/255.255.255.240]
    G3_BE_CL_A-->G3_BE_IF_A-->G3_BE_S-->G3_BE_IF_R11-->G3_BE_RR1-->G3_BE_IF_R12-->G3_BE_NET
end

subgraph Goal3_after
    direction TB
    G3_AF_NET{{Internet}}
    G3_AF_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.172.250.1)))
    G3_AF_S{Switch}
    G3_AF_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    G3_AF_IF_A[0.0.0.0/0.0.0.0]
    G3_AF_IF_R11[0.0.0.0/255.255.255.128]
    G3_AF_IF_R12[163.172.250.12/255.255.255.240]
    G3_AF_CL_A-->G3_AF_IF_A-->G3_AF_S-->G3_AF_IF_R11-->G3_AF_RR1-->G3_AF_IF_R12-->G3_AF_NET
end


Goal2_before-->Goal2_after
subgraph Goal2_before
    direction TB
    G2_BE_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G2_BE_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    G2_BE_CL_D[[ClientD<br>0.0.0.0/0 => 96.94.26.75]]
    G2_BE_IF_C[0.0.0.0/0.0.0.0]
    G2_BE_IF_D[0.0.0.0/0.0.0.0]
    G2_BE_IF_R22[0.0.0.0/0.0.0.0]
    G2_BE_IF_R23[0.0.0.0//18]
    G2_BE_CL_C-->G2_BE_IF_C-->G2_BE_IF_R22-->G2_BE_RR2-->G2_BE_IF_R23-->G2_BE_IF_D-->G2_BE_CL_D
end

subgraph Goal2_after
    direction TB
    G2_AF_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    G2_AF_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    G2_AF_CL_D[[ClientD<br>0.0.0.0/0 => 96.94.26.75]]
    G2_AF_IF_C[0.0.0.0/0.0.0.0]
    G2_AF_IF_D[0.0.0.0/0.0.0.0]
    G2_AF_IF_R22[0.0.0.0/0.0.0.0]
    G2_AF_IF_R23[0.0.0.0//18]
    G2_AF_CL_C-->G2_AF_IF_C-->G2_AF_IF_R22-->G2_AF_RR2-->G2_AF_IF_R23-->G2_AF_IF_D-->G2_AF_CL_D
end


Goal1_before-->Goal1_after
subgraph Goal1_before
    direction TB
    G1_BE_S{Switch}
    G1_BE_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    G1_BE_CL_B[[ClientB<br>0.0.0.0/0 => 0.0.0.0]]
    G1_BE_IF_A[0.0.0.0/0 => 0.0.0.0]
    G1_BE_IF_B[0.0.0.0/0.0.0.0]
    G1_BE_CL_A-->G1_BE_IF_A-->G1_BE_S-->G1_BE_IF_B-->G1_BE_CL_B
end

subgraph Goal1_after
    direction TB
    G1_AF_S{Switch}
    G1_AF_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    G1_AF_CL_B[[ClientB<br>0.0.0.0/0 => 0.0.0.0]]
    G1_AF_IF_A[0.0.0.0/0 => 0.0.0.0]
    G1_AF_IF_B[0.0.0.0/0.0.0.0]
    G1_AF_CL_A-->G1_AF_IF_A-->G1_AF_S-->G1_AF_IF_B-->G1_AF_CL_B
end
```
