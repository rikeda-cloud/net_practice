# level2

## Goal1
* <font color="red">***InterfaceB1***</font>と<font color="blue">***InterfaceA1***</font>のサブネットマスクが違うため、サブネットマスクを揃える。（サブネットを揃えなくても良いが説明は省略）
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="blue">***InterfaceA1***</font>に設定する。

## Goal2
* <font color="yellow">***InterfaceC1***</font>と<font color="skayblue">***InterfaceD1***</font>のサブネットマスクが同一。（/30のような書き方はサブネットマスクの短絡記法）
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="yellow">***InterfaceC1***</font>と<font color="skayblue">***InterfaceD1***</font>に設定する。（127.0.0.*というIPアドレスはループバックアドレスであるため使用できない）

## chart
```mermaid
%%{init:{'theme': 'dark'}}%%
%%{init:{'flowchart': {'curve': 'natural'}}}%%
flowchart
subgraph LEVEL2
    direction TB
    CL_C[[ClientC]]
    CL_D[[ClientD]]
    IF_C[InterfaceC1]
    IF_D[InterfaceD1]
    CL_C-->IF_C-->IF_D-->CL_D
end
subgraph Goal1
    direction TB
    CL_A[[ClientA]]
    CL_B[[ClientB]]
    IF_A[InterfaceA1]
    IF_B[InterfaceB1]
    CL_A-->IF_A-->IF_B-->CL_B
end
```

## example
```mermaid
%%{init:{'theme': 'dark'}}%%
%%{init:{'flowchart': {'curve': 'natural'}}}%%
flowchart

Goal2_before-->Goal2_after
subgraph Goal2_before
    direction TB
    BE_CL_C[[ClientC]]
    BE_CL_D[[ClientD]]
    BE_IF_C[127.0.0.1/255.255.255.252]
    BE_IF_D[127.0.0.4//30]
    BE_CL_C-->BE_IF_C-->BE_IF_D-->BE_CL_D
end

subgraph Goal2_after
    direction TB
    AF_CL_C[[ClientC]]
    AF_CL_D[[ClientD]]
    AF_IF_C[111.111.111.1/255.255.255.252]
    AF_IF_D[111.111.111.2//30]
    AF_CL_C-->AF_IF_C-->AF_IF_D-->AF_CL_D
end


Goal1_before-->Goal1_after
subgraph Goal1_before
    direction TB
    BE_CL_A[[ClientA]]
    BE_CL_B[[ClientB]]
    BE_IF_A[0.0.0.0/255.255.255.224]
    BE_IF_B[192.168.77.222/0.0.0.0]
    BE_CL_A-->BE_IF_A-->BE_IF_B-->BE_CL_B
end

subgraph Goal1_after
    direction TB
    AF_CL_A[[ClientA]]
    AF_CL_B[[ClientB]]
    AF_IF_A[192.168.77.221/255.255.255.224]
    AF_IF_B[192.168.77.222/255.255.255.224]
    AF_CL_A-->AF_IF_A-->AF_IF_B-->AF_CL_B
end
```
