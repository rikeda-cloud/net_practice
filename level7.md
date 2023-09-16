# level7

## Goal1
* 全てのInterfaceに共通したサブネットマスクを持たせる。（省略）level7では、<font color="skayblue">***InterfaceA1***</font>と<font color="blue">***InterfaceR11***</font>が属するネットワークと、<font color="red">***InterfaceR12***</font>と<font color="green">***InterfaceR21***</font>が属するネットワークと、<font color="#FF00FF">***InterfaceC1***</font>と<font color="yellow">***InterfaceR22***</font>が属する３ネットワークで１ネットワークに２つのIPを付与できるネットワークアドレスを付ければいいので、サブネットマスクを４つ作れる/30(255.255.255.248)を設定。
* <font color="skayblue">***InterfaceA1***</font>に<font color="blue">***InterfaceR11***</font>と同じネットワークアドレスで異なるホストアドレスのIPアドレスを設定する。
* <font color="#00FFFF">***ClientA***</font>のデフォルトゲートウェイに<font color="blue">***InterfaceR11***</font>のIPアドレスを設定する。
* <font color="green">***InterfaceR21***</font>に<font color="red">***InterfaceR12***</font>と同じネットワークアドレスで異なるホストアドレスのIPアドレスを設定する。
* <font color="#00FFFF">***RouterR1***</font>のデフォルトゲートウェイに<font color="green">***InterfaceR21***</font>のIPアドレスを設定する。
* <font color="#0FFF0F">***RouterR2***</font>のデフォルトゲートウェイに<font color="red">***InterfaceR12***</font>のIPアドレスを設定する。
* <font color="#FF00FF">***InterfaceC1***</font>と<font color="yellow">***InterfaceR22***</font>に他のネットワークのネットワークアドレスと異なるネットワークアドレスで、且つ、互いにホストアドレスの異なるIPアドレスを設定する。
* <font color="#0FFFF0">***ClientC***</font>のデフォルトゲートウェイに<font color="yellow">***InterfaceR22***</font>のIPアドレスを設定する。


## chart
```mermaid
%%{init:{'theme': 'dark'}}%%
flowchart
subgraph LEVEL7
    direction TB
    CA[[ClientA]]
    CC[[ClientC]]
    RR1(((RouterR1)))
    RR2(((RouterR2)))
    IA[InterfaceA1]
    IC[InterfaceC1]
    IR11[InterfaceR11]
    IR12[InterfaceR12]
    IR21[InterfaceR21]
    IR22[InterfaceR22]
    CA-->IA-->IR11-->RR1-->IR12-->IR21-->RR2-->IR22-->IC-->CC
    CC-->IC-->IR22-->RR2-->IR21-->IR12-->RR1-->IR11-->IA-->CA
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
    BE_CL_C[[ClientC<br>0.0.0.0/0 => 0.0.0.0]]
    BE_RR1(((RouterR1<br>0.0.0.0/0 => 0.0.0.0)))
    BE_RR2(((RouterR2<br>0.0.0.0/0 => 0.0.0.0)))
    BE_IF_A[0.0.0.0/0.0.0.0]
    BE_IF_C[0.0.0.0/0.0.0.0]
    BE_IF_R11[107.198.14.1/0.0.0.0]
    BE_IF_R12[107.198.14.254/0.0.0.0]
    BE_IF_R21[0.0.0.0/0.0.0.0]
    BE_IF_R22[0.0.0.0/0.0.0.0]
    BE_CL_A<-->BE_IF_A<-->BE_IF_R11<-->BE_RR1<-->BE_IF_R12<-->BE_IF_R21<-->BE_RR2<-->BE_IF_R22<-->BE_IF_C<-->BE_CL_C
end

subgraph Goal1_after
    direction TB
    AF_CL_A[[ClientA<br>0.0.0.0/0 => 107.198.14.1]]
    AF_CL_C[[ClientC<br>0.0.0.0/0 => 107.198.14.129]]
    AF_RR1(((RouterR1<br>0.0.0.0/0 => 107.198.14.253)))
    AF_RR2(((RouterR2<br>0.0.0.0/0 => 107.198.14.254)))
    AF_IF_A[107.198.14.2//30]
    AF_IF_C[107.198.14.130//30]
    AF_IF_R11[107.198.14.1//30]
    AF_IF_R12[107.198.14.254//30]
    AF_IF_R21[107.198.14.253//30]
    AF_IF_R22[107.198.14.129//30]
    AF_CL_A<-->AF_IF_A<-->AF_IF_R11<-->AF_RR1<-->AF_IF_R12<-->AF_IF_R21<-->AF_RR2<-->AF_IF_R22<-->AF_IF_C<-->AF_CL_C
end
```
