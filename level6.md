# level6

## How to solve
### Goal1
* <font color="red">***InterfaceA1***</font>に<font color="blue">***InterfaceR1***</font>と同じサブネットマスクを設定する。（省略）
* <font color="blue">***InterfaceR1***</font>に<font color="red">***InterfaceA1***</font>ネットワーク部が同じでホスト部が異なるIPアドレスを設定する。
* <font color="green">***ClientA***</font>のデフォルトルートに<font color="blue">***InterfaceR1***</font>のIPアドレスを設定する。
* <font color="yellow">***Internet***</font>から内部ネットワークへの送り先ネットワークアドレスに<font color="red">***InterfaceA1***</font>と同じネットワークアドレス値を設定する。

## chart
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
subgraph LEVEL6
    direction BT
    CA[[ClientA]]
    RR(((RouterR)))
    IA[InterfaceA1]
    IR1[InterfaceR1]
    IR2[InterfaceR2]
    IS[InterfaceSomewhere]
    S{SwitchS}
    NET{{Internet}}
    CA-->IA-->S-->IR1-->RR-->IR2-->NET-.->IS
    IS-.->NET-->IR2-->RR-->IR1-->S-->IA-->CA
end
```

## example
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
Goal1_before-->Goal1_after
subgraph Goal1_before
    direction TB
    BE_CL_A[[ClientA<br>0.0.0.0/0 => 0.0.0.0]]
    BE_RR(((RouterR<br>0.0.0.0/8 => 163,172.250.1)))
    BE_IF_A[91.141.41.227/0.0.0.0]
    BE_IF_R1[0.0.0.0/255.255.255.128]
    BE_IF_R2[163.172.250.12/255.255.255.240]
    BE_IF_S[Somewhere<br>8.8.8.8/16]
    BE_S{SwitchS}
    BE_NET{{Internet<br>0.0.0.0/0 => 163.172.250.12}}
    BE_CL_A<-->BE_IF_A<-->BE_S<-->BE_IF_R1<-->BE_RR<-->BE_IF_R2<-->BE_NET<-->BE_IF_S   
end

subgraph Goal1_after
    direction TB
    AF_RR(((RouterR<br>0.0.0.0/0 => 163,172.250.1)))
    AF_CL_A[[ClientA<br>0.0.0.0/0 => 91.141.41.228]]
    AF_IF_A[91.141.41.227/255.255.255.128]
    AF_IF_R1[91.141.41.228/255.255.255.128]
    AF_IF_R2[163.172.250.12/255.255.255.240]
    AF_IF_S[Somewhere<br>8.8.8.8/16]
    AF_S{SwitchS}
    AF_NET{{Internet<br>91.141.41.128/25 => 163.172.250.12}}
    AF_CL_A<-->AF_IF_A<-->AF_S<-->AF_IF_R1<-->AF_RR<-->AF_IF_R2<-->AF_NET<-->AF_IF_S   
end
```
