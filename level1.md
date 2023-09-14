# level1

## Goal1
* <font color="red">InterfaceA1</font>と<font color="blue">InterfaceB1</font>のサブネットマスクが同一。
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="red">InterfaceA1</font>に設定する。

## Goal2
* <font color="red">InterfaceC1</font>と<font color="blue">InterfaceD1</font>のサブネットマスクが同一。
* IPアドレスのネットワークアドレスが一致し、且つ、ホストアドレスが異なるIPアドレスを<font color="red">InterfaceD1</font>に設定する。

```mermaid
flowchart
subgraph Goal2
    direction BT
    ClientC1-->InterfaceC1-- InterafaceC1.NW == InterafaceD1.NW <br>AND<br>InterafaceC1.IP != InterafaceD1.IP-->InterfaceD1-->ClientD1
end
subgraph Goal1
    direction BT
    ClientA1-->InterfaceA1-- InterafaceA1.NW == InterafaceB1.NW <br>AND<br>InterafaceA1.IP != InterafaceB1.IP-->InterfaceB1-->ClientB1
end
```
