# level8

## How to solve
### Goal1
* 全てのサブネットマスクを255.255.255.240(/28)に統一する。（省略）
* <font color="red">***ClientC***</font>側のネットワーク、<font color="blue">***ClientD***</font>側のネットワーク、<font color="green">***InterfaceR21***</font>側のネットワークのそれぞれ別々のネットワーク部で同一ネットワーク内では別々のホスト部を設定する。<br>注意点として、<font color="yellow">***Internet***</font>が内部ネットワークへの返却アドレスとして指定しているサブネットマスクに/26が設定されており、<font color="skayblue">***InterfaceD1***</font>でサブネットマスクに255.255.255.240(/28)が設定されているため、二進数で上位26bitは共通で上位28bitがそれぞれのネットワークで異なる必要がある。
    * 例
        * 11111111.1111111.1111111.1100111
        * 11111111.1111111.1111111.1101111
        * 11111111.1111111.1111111.1110111
    * 上記の３つのIPアドレスは上位26bitは共通したビット列であるが、上位28bitは全て異なるビット列となる。
* <font color="red">***ClientC***</font>のデフォルトルートに<font color="#0F0F0F">***InterfaceR22***</font>のIPアドレスを設定する。
* <font color="blue">***ClientD***</font>のデフォルトルートに<font color="#F0F0F0">***InterfaceR23***</font>のIPアドレスを設定する。

### Goal2
* <font color="#886699">***RouterR1***</font>の一つ目のデフォルトルートを<font color="yellow">***Internet***</font>の内部ネットワークへの返却アドレスとして設定されているIPアドレスとサブネットマスク==><font color="#006699">***InterfaceR21***</font>のIPアドレスへの設定にする。
* <font color="yellow">***Internet***</font>の内部ネットワークへの返却アドレス設定に<font color="#3300FF">***InterfaceR12***</font>のIPアドレスを設定する。

### Goal3
* Goal1,Goal2の設定ができていればOK

## chart
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
subgraph LEVEL8
    direction TB
    NET{{Internet}}
    CC[[ClientC]]
    CD[[ClientD]]
    RR1(((RouterR1)))
    RR2(((RouterR2)))
    IC[InterfaceC1]
    ID[InterfaceD1]
    IR12[InterfaceR12]
    IR13[InterfaceR13]
    IR21[InterfaceR21]
    IR22[InterfaceR22]
    IR23[InterfaceR23]

    NET-->IR12-->RR1-->IR13-->IR21-->RR2-->IR21-->IR13-->RR1-->IR12-->NET
    CD-->ID-->IR23-->RR2-->IR23-->ID-->CD
    CC-->IC-->IR22-->RR2-->IR22-->IC-->CC
end
```

## example
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
Goal3_before-->Goal3_after
subgraph Goal3_before
    direction TB
    G3_BE_CL_D[[ClientD<br>default => 0.0.0.0]]
    G3_BE_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.29.250.1)))
    G3_BE_RR2(((RouterR2<br>0.0.0.0/0 => 130.63.70.62)))
    G3_BE_IF_D[0.0.0.0/255.255.255.240]
    G3_BE_IF_R12[163.29.250.12/255.255.255.240]
    G3_BE_IF_R13[0.0.0.0/0]
    G3_BE_IF_R21[0.0.0.0/0]
    G3_BE_IF_R23[0.0.0.0/0]
    G3_BE_NET{{Internet<br>130.63.70.0/26 => 0.0.0.0}}
    G3_BE_CL_D<-->G3_BE_IF_D<-->G3_BE_IF_R23<-->G3_BE_RR2<-->G3_BE_IF_R21<-->G3_BE_IF_R13<-->G3_BE_RR1<-->G3_BE_IF_R12<-->G3_BE_NET
end

subgraph Goal3_after
    direction TB
    G3_AF_CL_D[[ClientD<br>default => 130.63.70.2]]
    G3_AF_RR1(((RouterR1<br>130.63.70.0/26 => 130.63.70.61<br>0.0.0.0/0 => 163.29.250.1)))
    G3_AF_RR2(((RouterR2<br>default => 130.63.70.62)))
    G3_AF_IF_D[130.63.70.1/255.255.255.240]
    G3_AF_IF_R12[163.29.250.12/255.255.255.240]
    G3_AF_IF_R13[130.63.70.62/255.255.255.252]
    G3_AF_IF_R21[130.63.70.61/255.255.255.252]
    G3_AF_IF_R23[130.63.70.2/255.255.255.240]
    G3_AF_NET{{Internet<br>130.63.70.0/26 => 163.29.250.12}}
    G3_AF_CL_D<-->G3_AF_IF_D<-->G3_AF_IF_R23<-->G3_AF_RR2<-->G3_AF_IF_R21<-->G3_AF_IF_R13<-->G3_AF_RR1<-->G3_AF_IF_R12<-->G3_AF_NET
end


Goal2_before-->Goal2_after
subgraph Goal2_before
    direction TB
    G2_BE_CL_C[[ClientC<br>default => 0.0.0.0]]
    G2_BE_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0<br>0.0.0.0/0 => 163.29.250.1)))
    G2_BE_RR2(((RouterR2<br>0.0.0.0/8 => 130.63.70.62)))
    G2_BE_IF_C[0.0.0.0/0.0.0.0]
    G2_BE_IF_R12[163.29.250.12/255.255.255.240]
    G2_BE_IF_R13[0.0.0.0/0]
    G2_BE_IF_R21[0.0.0.0/0]
    G2_BE_IF_R22[0.0.0.0/0]
    G2_BE_NET{{Internet<br>130.63.70.0/26 => 0.0.0.0}}
    G2_BE_CL_C<-->G2_BE_IF_C<-->G2_BE_IF_R22<-->G2_BE_RR2<-->G2_BE_IF_R21<-->G2_BE_IF_R13<-->G2_BE_RR1<-->G2_BE_IF_R12<-->G2_BE_NET
end

subgraph Goal2_after
    direction TB
    G2_AF_CL_C[[ClientC<br>default => 130.63.70.34]]
    G2_AF_RR1(((RouterR1<br>130.63.70.0/26 => 130.63.70.61<br>0.0.0.0/0 => 163.29.250.1)))
    G2_AF_RR2(((RouterR2<br>default => 130.63.70.62)))
    G2_AF_IF_C[130.63.70.33/255.255.255.252]
    G2_AF_IF_R12[163.29.250.12/255.255.255.240]
    G2_AF_IF_R13[130.63.70.62/255.255.255.252]
    G2_AF_IF_R21[130.63.70.61/255.255.255.252]
    G2_AF_IF_R22[130.63.70.34/255.255.255.252]
    G2_AF_NET{{Internet<br>130.63.70.0/26 => 163.29.250.12}}
    G2_AF_CL_C<-->G2_AF_IF_C<-->G2_AF_IF_R22<-->G2_AF_RR2<-->G2_AF_IF_R21<-->G2_AF_IF_R13<-->G2_AF_RR1<-->G2_AF_IF_R12<-->G2_AF_NET
end


Goal1_before-->Goal1_after
subgraph Goal1_before
    direction TB
    G1_BE_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    G1_BE_CL_D[[ClientD<br>default => 0.0.0.0]]
    G1_BE_RR2(((RouterR2<br>0.0.0.0/8 => 130.63.70.62)))
    G1_BE_IF_C[0.0.0.0/0]
    G1_BE_IF_D[0.0.0.0/255.255.255.240]
    G1_BE_IF_R22[0.0.0.0/0]
    G1_BE_IF_R23[0.0.0.0/0]
    G1_BE_CL_C<-->G1_BE_IF_C<-->G1_BE_IF_R22<-->G1_BE_RR2<-->G1_BE_IF_R23<-->G1_BE_IF_D<-->G1_BE_CL_D
end

subgraph Goal1_after
    direction TB
    G1_AF_CL_C[[ClientC<br>0.0.0.0/0 => 130.63.70.34]]
    G1_AF_CL_D[[ClientD<br>default => 130.63.70.2]]
    G1_AF_RR2(((RouterR2<br>default => 130.63.70.62)))
    G1_AF_IF_C[130.63.70.33/255.255.255.252]
    G1_AF_IF_D[130.63.70.1/255.255.255.240]
    G1_AF_IF_R22[130.63.70.34/255.255.255.252]
    G1_AF_IF_R23[130.63.70.2/255.255.255.240]
    G1_AF_CL_C<-->G1_AF_IF_C<-->G1_AF_IF_R22<-->G1_AF_RR2<-->G1_AF_IF_R23<-->G1_AF_IF_D<-->G1_AF_CL_D
end
```
