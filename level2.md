# level2

## Goal1
* <font color="red">***InterfaceB1***</font>と<font color="blue">***InterfaceA1***</font>のサブネットマスクが違うため、サブネットマスクを揃える。（サブネットを揃えなくても良いが説明は省略）
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="blue">***InterfaceA1***</font>に設定する。

## Goal2
* <font color="red">***InterfaceC1***</font>と<font color="blue">***InterfaceD1***</font>のサブネットマスクが同一。（/30のような書き方はサブネットマスクの短絡記法）
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="red">***InterfaceC1***</font>と<font color="blue">***InterfaceD1***</font>に設定する。（127.0.0.*というIPアドレスはループバックアドレスであるため使用できない）

## chart
```mermaid
flowchart
subgraph Goal2
    direction BT
    ClientC-->InterfaceC1--InterafaceC1.NW == InterafaceD1.NW <br>AND<br>InterafaceC1.IP != InterafaceD1.IP-->InterfaceD1-->ClientD
end
subgraph Goal1
    direction BT
    ClientA-->InterfaceA1--InterafaceA1.NW == InterafaceB1.NW <br>AND<br>InterafaceA1.IP != InterafaceB1.IP-->InterfaceB1-->ClientB
end
```
## example
```mermaid
flowchart

Goal2_before-->Goal2_after
subgraph Goal2_before
    direction BT
    ComputerC-->127.0.0.1/255.255.255.252-- InterafaceC1.NW == InterafaceD1.NW <br>AND<br>InterafaceC1.IP != InterafaceD1.IP-->127.0.0.4//30-->ComputerD
end

subgraph Goal2_after
    direction BT
    _ComputerC-->_111.111.111.1/255.255.255.252-- InterafaceC1.NW == InterafaceD1.NW <br>AND<br>InterafaceC1.IP != InterafaceD1.IP-->_111.111.111.2//30-->_ComputerD
end


Goal1_before-->Goal1_after
subgraph Goal1_before
    direction BT
    ComputerA-->0.0.0.0/255.255.255.224-- InterafaceA1.NW == InterafaceB1.NW <br>AND<br>InterafaceA1.IP != InterafaceB1.IP-->192.168.77.222/0.0.0.0-->ComputerB
end

subgraph Goal1_after
    direction BT
    _ComputerA-->_192.168.77.221/255.255.255.224-- InterafaceA1.NW == InterafaceB1.NW <br>AND<br>InterafaceA1.IP != InterafaceB1.IP-->_192.168.77.222/255.255.255.224-->_ComputerB
end

```
